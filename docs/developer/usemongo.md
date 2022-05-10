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
    let result = await GoodsBuy.find(filter).populate("doc", {
      _id: 1,
      name: 1,
      pub: 1,
      desc: 1,
    });
```
