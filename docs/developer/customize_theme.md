# 定制皮肤

MTC 的颜色，布局，使用 Bootstrap， 需要调整时，对 Boostrap 进行定制即可

## 颜色定制

```
cd ~/dev/bootstrap
vim scss/_variables.scss
```

按需要做出调整后，重新编译

```
cd ~/dev/bootstrape
npm run css
cp dist/css/bootstrap.min.* ~/dev/empfrt/static/css/bootstrap/dist
```
