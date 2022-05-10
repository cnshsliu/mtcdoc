# About MongoDB

[Manual](https://www.mongodb.com/docs/manual/core/document/)

# About Mongoose

[Mongoose Manual](https://mongoosejs.com/docs/)

## Schema

定义数据库
[Schmes](https://mongoosejs.com/docs/guide.html#schemas)

## 数据操作

- 操作文档对象，而不是操作记录

# Schema Definition with Mongoose

[Schema Types](https://mongoosejs.com/docs/schematypes.html)

Example:

- 例一

```
var Mongoose = require("mongoose"),
  //The document structure definition
  Schema = Mongoose.Schema;

//Same fields as Parse.com
var BlockSchema = new Schema(
  {
    //一个对象参考
    doc: { type: Mongoose.Schema.Types.ObjectId, ref: "Document" },
    //是否必须
    etype: { type: String, required: [true, "不能为空"] },
    //单属性索引
    nodeid: { type: String, required: [true, "不能为空"], index:true },
    lastupdate: { type: Number },
    //缺省值
    lock: { type: Boolean, default: false },
    content: { type: Buffer },
    mdnote: { type: Buffer },
  },
  //自动添加时间戳  createdAt, updatedAt
  { timestamps: true }
);
//组合索引
BlockSchema.index({ doc: 1, nodeid: 1 }, { unique: true });

//mongodb中的collection(sql:table)名称, Block小写加s， db.blocks.find()
var block = Mongoose.model("Block", BlockSchema);

module.exports = block;
```

- 例二

```
import Mongoose from "mongoose";
const schema = new Mongoose.Schema(
  {
    tenant: { type: Mongoose.Schema.Types.ObjectId, ref: "Tenant" },
    rehearsal: { type: Boolean, default: false },
    who: { type: String, required: true },
    towhom: { type: String, required: true },
    //限定值的范围
    objtype: {
      type: String,
      enum: ["SITE", "TENANT", "TEMPLATE", "WORKFLOW", "WORK", "TODO", "COMMENT"],
      default: "TENANT",
    },
    objid: { type: String },
    threadid: { type: String },
    //数组类型，及缺省值
    people: { type: [String], default: [] },
    content: { type: String, default: "" },
    //子文档
    context: {
      wfid: String,
      workid: String,
      todoid: String,
    },
    // 定类型的 子文档数组
    //attachments: { type: [ fileId: string, fileName: string }], default: [] },
    // 不定类型的 子文档数组
    //attachments: { type: [Mongoose.Schema.Types.Mixed], default: [] },
  },
  { timestamps: true }
);

export default Mongoose.model("Comment", schema);
```

# 数据操作

## INSERT 添加

- new 一个对象然后 save

```
    let comment = new Comment({
      tenant: tenant,
      rehearsal: rehearsal,
      who: doer,
      towhom: toWhom,
      objtype: objtype,
      objid: objid,
      people: thePeople,
      content: content,
      context: context,
      threadid: threadid ? threadid : "",
    });
    comment = await comment.save();
    // _id
    // _v:1
    //createAt, updatedAt
    //....
    comment.towhom = "another people";
    await comment.save(); //upate
    // _v:2
```

- findOneAndUpdate
  [findOneAndUpdate](https://mongoosejs.com/docs/api.html#model_Model.findOneAndUpdate)

```
  let comment = await Comment.findOneAndUpdate({tenant:tenant, _id: "1234"},
    {$set: {towhom: 'zhangsan', ...}},
    {upsert: true, new:true});
```

cRud

## Find 查询

findOne: null, Object, findMany: [], [object]

基本结构：
findOne(文档条件, 返回哪些文档属性，其它参数）
[Mongoose Query](https://mongoosejs.com/docs/queries.html)

## All Quries

[Quries](https://mongoosejs.com/docs/api.html#Query)

exmaples: exists:

[Exists](https://mongoosejs.com/docs/api.html#query_Query-exists)

find:
[Find](https://mongoosejs.com/docs/api.html#query_Query-find)
[Find one](https://mongoosejs.com/docs/api.html#query_Query-findOne)

## 使用 WHERE

[Use Where](https://mongoosejs.com/docs/api.html#model_Model.where)

## Count and count the number of documents

[Count Documents](https://mongoosejs.com/docs/api.html#query_Query-count)

```
let number = await Comment.countDocuments({author: "chengaoyang@xihuanwu.com"});

let cmts = await Comment.find({{author: "chengaoyang@xihuanwu.com"}).lean();
nubmer = cmts.length;

```

## Using Lean

By default, Mongoose queries return an instance of the Mongoose Document class. Documents are much heavier than vanilla JavaScript objects, because they have a lot of internal state for change tracking. Enabling the lean option tells Mongoose to skip instantiating a full Mongoose document and just give you the POJO.
[Lean](https://mongoosejs.com/docs/tutorials/lean.html#using-lean)

## Skip and limit

let retObjs = await Workflow.find(filter, fields).sort(sortBy).skip(skip).limit(limit).lean();

## Update

- 方法一： 查询得出文档，修改文档属性，调用 save()
- 方法二： 使用 updated:updateOne, updateMany

```
  // Get lastName from http post
    let lastName=req.payload.lastName;
    //Build regexp: 如lastName="chen", 则 chenABC 可以， chenABCD不可以，  achenABC不可以
    let regex = new RegExp(`^${lastName}.{3}$`);
  let updatedComment = await Comment.updateMany({author: regex}, {$set:{content: "byChen", num:1, author: "liu"} }, {new:true}).lean();
```

## Aggrgateion mongodb Powerful

Aggregation can do many of the same things that queries can. For example, below is how you can use aggregate() to find docs where name.last = 'Ghost':

```
//Query: await Person.find({'name.last': 'Ghost'});
// aggregate([])
const docs = await Person.aggregate(
[
    { $match: { 'name.last': 'Ghost' } }
]
);
```

However, just because you can use aggregate() doesn't mean you should. In general, you should use queries where possible, and only use aggregate() when you absolutely need to.

Unlike query results, Mongoose does not hydrate() aggregation results. Aggregation results are always POJOs, not Mongoose documents.

hydate() or not example:

```
// 取一个Tenant
tenant = await Tenant.findOne({name: 'xihuanwu'});
let tntId = tenant._id;
//用tentId 做查询， 使用Query
 await Comment.findMany({tenant: tntId});
 or
 await Comment.findMany({tenant: new ObjectId(tntId)});
//用tentId 做查询， 使用Aggreate
 await Comment.aggregate([
  {$match:
    {'tenant': new ObjectId(tenant._id)}
  }
 ])
```

```
const docs = await Person.aggregate([{ $match: { 'name.last': 'Ghost' } }]);

docs[0] instanceof mongoose.Document; // false
```

Also, unlike query filters, Mongoose also doesn't cast aggregation pipelines. That means you're responsible for ensuring the values you pass in to an aggregation pipeline have the correct type.

```
const doc = await Person.findOne();

const idString = doc._id.toString();

// Finds the `Person`, because Mongoose casts `idString` to an ObjectId
const queryRes = await Person.findOne({ _id: idString }, {"_id":0, "name":0});


// Does **not** find the `Person`, because Mongoose doesn't cast aggregation
// pipelines.
const aggRes = await Person.aggregate([{ $match: { _id: idString } }])
```

```
      let todoFilter = {status: 'ST_RUN'};
      let todoGroup = await Todo.aggregate([
        { $match: todoFilter },
        { $group: { _id: "$wfid", count: { $sum: 1 } } },
      ]);

      //
      [
        { _id: "wfid_1", count: 100},
        { _id: "wfid_2", count: 150},
      ]
      let WfsIamIn = todoGroup.map((x) => x._id);
```

```
Workflow:
{
_id: ObjectId,
  wfid:String,
  doc: String,
  attachments: [
    {
      serverId: String,
      realName: String
    }
  ]
}

{wfid: "wfid_1", attachments: [ { serverId: '1-1', realName: 'name-1-1'}, { serverId: '1-2', realName: 'name-1-2'}]}

{wfid: "wfid_2", attachments: [ { serverId: '2-1', realName: 'name-2-1'}, { serverId: '2-2', realName: 'name-2-2'}]}



找出 包含serverID= 1-2的workflow，并且，返回值中只包含1-2

    let wf = await Workflow.aggregate([
      { $match: { tenant: new Mongoose.Types.ObjectId(tenant), "attachments.serverId": serverId } },
      { $project: { _id: 0, doc: 0 } },

       // {wfid: "wfid_1", attachments: [ { serverId: '1-1', realName: 'name-1-1'}, { serverId: '1-2', realName: 'name-1-2'}]}

      { $unwind: "$attachments" },
      /*
        [
        {wfid: "wfid_1", attachments: { serverId: '1-1', realName: 'name-1-1'}},
        {wfid: "wfid_1", attachments: { serverId: '1-2', realName: 'name-1-2'}}
        ]
        */
      { $match: { "attachments.serverId": serverId } },
      /*
      [
        {wfid: "wfid_1", attachments: { serverId: '1-2', realName: 'name-1-2'}}
      ]
      */
    ]);
    if (wf[0]) {
      throw new EmpError("CANNOT_DELETE", "File is used in workflow");
    }
```

```
    let ret = await Todo.aggregate([
      { $match: filter },
      {
        //lastdays， 当前活动已经持续了多少天，如果是ST_RUN或者ST_PAUSE，跟当前时间相比；
        //否则，用updatedAt - createdAt
        $addFields: {
          lastdays: {
            $cond: {
              if: {
                $or: [{ $eq: ["$status", "ST_RUN"] }, { $eq: ["$status", "ST_PAUSE"] }],
              },
              then: {
                $dateDiff: { startDate: "$createdAt", endDate: "$$NOW", unit: "day" },
              },
              else: {
                $dateDiff: { startDate: "$createdAt", endDate: "$updatedAt", unit: "day" },
              },
            },
          },
        },
      },
      { $sort: sortByJson },
      { $skip: skip },
      { $limit: limit },
    ]);
```

```
  let blkops = await BlockOp.aggregate([
    {
      $match: {doc: {$eq: Mongoose.Types.ObjectId(doc_id)}},
    },
    {
      $group: {
        _id: "$userid",
        logs: {$addToSet: "$nodeid"},
      },
    },
    {
      $lookup: {
        from: "users",
        localField: "_id",
        foreignField: "userid",
        as: "fromItems",
      },
    },
    {
      $replaceRoot: {
        newRoot: {
          $mergeObjects: [{$arrayElemAt: ["$fromItems", 0]}, "$$ROOT"],
        },
      },
    },
    {
      $project: {
        fromItems: 0,
        pwd: 0,
      },
    },
  ]).allowDiskUse(true);
```

## Populate

[Populate](https://mongoosejs.com/docs/populate.html)
MongoDB has the join-like $lookup aggregation operator in versions >= 3.2. Mongoose has a more powerful alternative called populate(), which lets you reference documents in other collections.

Population is the process of automatically replacing the specified paths in the document with document(s) from other collection(s). We may populate a single document, multiple documents, a plain object, multiple plain objects, or all objects returned from a query.

```
    let result = await GoodsBuy.find(filter).populate("doc", {
      _id: 1,
      name: 1,
      pub: 1,
      desc: 1,
    });
```

## Transaction

[Transaction](https://mongoosejs.com/docs/transactions.html#transactions-in-mongoose)
Transactions are new in MongoDB 4.0 and Mongoose 5.2.0. Transactions let you execute multiple operations in isolation and potentially undo all the operations if one of them fails. This guide will get you started using transactions with Mongoose.
