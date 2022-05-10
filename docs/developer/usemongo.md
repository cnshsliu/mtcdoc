# About MongoDB

[Manual](https://www.mongodb.com/docs/manual/core/document/)

# About Mongoose

[Mongoose Manual](https://mongoosejs.com/docs/)

## Schema

定义数据库

## 数据操作

- 操作文档对象，而不是操作记录

# Schema Definition with Mongoose

Example:

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

//mongodb中的collection名称, Block小写加s， db.blocks.find()
var block = Mongoose.model("Block", BlockSchema);

module.exports = block;
```

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
    // 不定类型的 子文档数组
    //attachments: { type: [Mongoose.Schema.Types.Mixed], default: [] },
    // 定类型的 子文档数组
    //attachments: { type: [ fileId: string, fileName: string }], default: [] },
  },
  { timestamps: true }
);

export default Mongoose.model("Comment", schema);
```

# 数据操作

## INSERT 添加

new 一个

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
```

```
    let result = await GoodsBuy.find(filter).populate("doc", {
      _id: 1,
      name: 1,
      pub: 1,
      desc: 1,
    });
```
