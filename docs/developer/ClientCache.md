# MTC Client Cache Best Practice

Invalidate cache and naming is two most difficult things

## 两种场景：浏览器请求与应用请求

### 浏览器控制

    		- 如<img src="...."/>
    		- 参考HTTP Caching文档
    		- https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching
    		- 彻底不缓存
    			-
    			  ```
    			  				  Cache-Control: no-store
    			  ```
    		- 缓存，但每次都去服务器检查是否有新数据
    			-
    			  ```
    			  				  Cache-Control: no-cache
    			  ```
    		- 缓存，但在一定时间内不去服务器检查是否有新数据
    			- 使用If-Modified-Since来判断
    				- 服务器端下发：
    				-
    				  ```
    				  					  HTTP/1.1 200 OK
    				  					  Content-Type: text/html
    				  					  Content-Length: 1024
    				  					  Date: Tue, 22 Feb 2022 22:22:22 GMT
    				  					  Last-Modified: Tue, 22 Feb 2022 22:00:00 GMT
    				  					  Cache-Control: max-age=3600

    				  ```
    				- 浏览器端后续请求：
    				-
    				  ```
    				  					  GET /index.html HTTP/1.1
    				  					  Host: example.com
    				  					  Accept: text/html
    				  					  If-Modified-Since: Tue, 22 Feb 2022 22:00:00 GMT

    				  ```
    			- 使用 If-None-Match来判断
    				- 服务器端下发：
    				-
    				  ```
    				  					  HTTP/1.1 200 OK
    				  					  Content-Type: text/html
    				  					  Content-Length: 1024
    				  					  Date: Tue, 22 Feb 2022 22:22:22 GMT
    				  					  ETag: "deadbeef"
    				  					  Cache-Control: max-age=3600
    				  ```
    				- 浏览器端后续请求：
    				-
    				  ```
    				  					  GET /index.html HTTP/1.1
    				  					  Host: example.com
    				  					  Accept: text/html
    				  					  If-None-Match: "deadbeef"
    				  ```
    		- MTC实践中，使用的是 ETag/If-None-Match, 数据有变化时，只在Redis中生成一个新的ETag，后续处理判断ETag是否一致
    			-
    		- 例一：MTC  Avatar
    			- 服务器端：
    				-
    				  ```
    				  					  	return (
    				  					  		h
    				  					  			.response(fs.createReadStream(avatarinfo.path))
    				  					  			.header("Content-Type", avatarinfo.media)
    				  					  			.header("X-Content-Type-Options", "nosniff")
    				  					  			.header("Cache-Control", "max-age=600, private")
    				  					  			//.header("Cache-Control", "no-cache, private")
    				  					  			.header("ETag", avatarinfo.etag)
    				  					  	);

    				  ```
    			- 客户端：
    			- ![image.png](../assets/image_1653372390390_0.png)
    			- 结果：
    				- 1. 浏览器在10分钟内，不再重新请求，而是使用缓存中的图片
    					2. 请求时，只有在图片变化时，服务器返回新图片，否则返回304头，浏览器使用已有缓存

    		- 例二：MTC Template Cover
    			- 服务器端：
    				-
    			- 客户端：
    				- ![image.png](../assets/image_1653372490395_0.png){:height 160, :width 662}
    			- 结果：
    				- 1. 浏览器每次都请求新数据
    					2. 请求时，只有在图片变化时，服务器返回新图片，否则返回304头，浏览器使用已有换

### 应用控制

    		- 如使用Fetch, Axios, HttpClient等请求数据，包括移动APP客户端从服务器获取数据
    		- 需要自行设计实现
    		- 两个问题：
    			- 是否有更合理的方案？
    			- APP中如何实现？

## 业务层刷新机制：

    	- 操作后，需要刷新
    		- 当前用户操作，及时刷新
    		- 其它用户，可定时刷新
    		- 如完成一件工作后的工作列表刷新
    	- 页面切换，使用本地缓存数据，无须重新拉取
    	- 自动刷新
    		- 设置合理的间隔
    		- 设置总刷新次数，防止用户不活动
    			- 或检测用户鼠标动作，上次鼠标移动在多长时间内，断定为用户活跃，继续自动刷新，否则停止。
    	- 提供手动刷新按钮（APP移动端为下拉）

## 技术层刷新机制：

    	- 服务端对每种业务对象设置ETag Key， 每次返回数据，带上一个ETag
    		- 这个ETag，用于表示某种数据是否刷新，仅在有变化时，修改这个ETag
    	- 客户端对每次请求的结果，进行缓存，同时记录ETag；
    	- 客户端每次请求，通过 If-None-Match带入 ETag
    	- 服务端相应请求时，判断服务端带来的ETag是否与服务端的ETag一致
    		- 如一致，返回304
    		- 如不一致，返回200，及最新数据
    	- 客户端根据Header是否是304，决定使用已缓存数据，还是更新缓存为服务端返回数据。

## 具体实现方式：

    	- 服务端CORS设置中，允许ETag
    		-
    		  ``` `
    		  			  		routes: {
    		  			  			//Allow CORS for all
    		  			  			cors: {
    		  			  				origin: ["*"],
    		  			  				credentials: true,
    		  			  				additionalExposedHeaders: ["ETag", "X-Content-Type-Options"],
    		  			  			},
    		  			  			validate: {
    		  			  				failAction: (request: Request, h: ResponseToolkit, err) => {
    		  			  					console.error(err);
    		  			  					if (request.method === "post") {
    		  			  						console.error(request.path, JSON.stringify(request.payload));
    		  			  					}
    		  			  					throw err;
    		  			  				},
    		  			  			},
    		  			  		},

    		  ```
    	- 客户端请求，将请求内容组装为cacheKey，并MD5位cacheID
    		-
    		  ```
    		  			  	const cacheKey = { method: method, path: path, body: {}, token: '' };

    		  			  	if (data) {
    		  			  		opts.headers['Content-Type'] = 'application/json';
    		  			  		opts.body = JSON.stringify(data);
    		  			  		cacheKey.body = opts.body;
    		  			  	}

    		  			  	if (token) {
    		  			  	opts.headers['authorization'] = token;
    		  			  		cacheKey.token = token;
    		  			  	}

    		  			  	let returnCode304 = false;
    		  			  	let foundCache = false;
    		  			  	const cacheID = MD5(JSON.stringify(cacheKey));

    		  ```
    	- 请求可以带cache控制cacheFlag，用户控制：
    		- preDelete： 在请求前，删除已有缓存
    		- useIfExists： 如存在缓存，直接返回缓存，不执行服务器请求
    		- 否则，在请求头中添加 If-None-Match
    		-
    		  ```
    		  			  	if (cacheFlag === CACHE_FLAG.preDelete) {
    		  			  		console.log(path, 'Preclear cache');
    		  			  		delete theCache[cacheID];
    		  			  	}
    		  			  	if (theCache[cacheID]) {
    		  			  		foundCache = true;
    		  			  		//console.log(path, 'Found cacheID in cache, should add If-None-Match header');
    		  			  		opts.headers['If-None-Match'] = theCache[cacheID].etag;
    		  			  		// } else {
    		  			  		//   console.log(path, 'no local cache');
    		  			  		if (cacheFlag === CACHE_FLAG.useIfExists) {
    		  			  			console.log(path, 'Return cached');
    		  			  			return JSON.parse(theCache[cacheID].data);
    		  			  		}
    		  			  	}

    		  ```
    	- 服务器端检查到If-None-Match后，判断是否有改变，如没有改变，直接返回304，不用做数据库查询等操作
    		-
    		  ```
    		  			  		let ifNoneMatch = req.headers["if-none-match"];
    		  			  		let latestETag = await Cache.getETag(`ETAG:TODOS:${doer}`);
    		  			  		if (ifNoneMatch && latestETag && ifNoneMatch === latestETag) {
    		  			  			return h
    		  			  				.response({})
    		  			  				.code(304)
    		  			  				.header("Content-Type", "application/json; charset=utf-8;")
    		  			  				.header("Cache-Control", "no-cache, private")
    		  			  				.header("X-Content-Type-Options", "nosniff")
    		  			  				.header("ETag", latestETag);
    		  			  		}

    		  ```
    		- 以上代码中，服务器端只需要按照业务逻辑，刷新相应的的ETag Redis记录，数据有变化，就修改Redis中相应eTag key的值。 如没有修改，则直接返回304
    	- 客户端请求后接收到304， 则直接使用缓存中的数据
    		-
    		  ```
    		  			  			if (response.status === 304) {
    		  			  				//console.log(path, 'Got 304');
    		  			  				returnCode304 = true;
    		  			  				responseETag = response.headers.get('etag');
    		  			  				return theCache[cacheID].data;
    		  			  			}
    		  ```
    	- 客户端从服务器端接收到的不是304，则说明，有新数据，则写入客户端缓存（包括收到的etag， 用于下次请求时，通过 If-None-Match带到服务器上对比）
    		-
    		  ```
    		  			  						if (responseETag) {
    		  			  							theCache[cacheID] = {
    		  			  								path: path,
    		  			  								data: jsonText,
    		  			  								etag: responseETag ? responseETag : '',
    		  			  							};
    		  			  							fetchCache.set(theCache);

    		  ```
    	- 缓存数据的记录方法
    		- 浏览器自身： 不用管它
    		- Cookie： 数据量非常小
    		- localStorage： 跨浏览器Session，但是使用K-V
    		- indexedDB:  数据库，SQL差不多，比LS复杂， 也是跨浏览器Session
    		- localStorage 跟配置相关用LS，
    		- session： 浏览器关掉，就消失。只要在session内部，没有问题
    			- Svelte 自己的session， 内存
    - Benefits
    	- 用户体验提升， 快速展示，不存在刷新
    		- 直接使用缓存，不请求
    		- 请求，但得到的是304， 数据量非常小。直接是缓存
    	- 网络和服务器的压力
    		- 直接使用缓存，不请求时， 请求变少
    		- 返回304时， 304的判断是在Redis里做，直接304时， 后续的所有数据库操作都没有必要。大部分的请求，是可以直接304的。
    	- 整体平台的相应能力提升
    	- 整体的费用得以下降
    	-

-

