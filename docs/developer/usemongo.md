# About MongoDB

[Manual](https://www.mongodb.com/docs/manual/core/document/)

# About Mongoose

[Mongoose Manual](https://mongoosejs.com/docs/)

## Schema

定义数据库

## 数据操作

- 操作文档对象，而不是操作记录

# Schema Definition with Mongoose

```
var Mongoose = require("mongoose"),
  //The document structure definition
  Schema = Mongoose.Schema;

//Same fields as Parse.com
var BlockSchema = new Schema(
  {
    //一个对象参考
    doc: { type: Mongoose.Schema.Types.ObjectId, ref: "Document" },
    etype: { type: String, required: [true, "不能为空"] },
    nodeid: { type: String, required: [true, "不能为空"], index:true },
    lastupdate: { type: Number },
    lock: { type: Boolean, default: false },
    content: { type: Buffer },
    mdnote: { type: Buffer },
  },
  { timestamps: true }
);
BlockSchema.index({ doc: 1, nodeid: 1 }, { unique: true });

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
