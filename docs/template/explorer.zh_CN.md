# 流程浏览

流程浏览器用于对组织的工作流模版进行管理

## 顶部按钮

- 新建: 显示新建流程模版的表单
- 导入 : 从本地电脑导入一个模版文件
- 重置查询: 清除查询条件，回归到缺省查询条件

### 新建模版

![Create template form](../img/template_create_form.png)

_Above: Create template form_

模版名称必须唯一， 模版的多个标签需要用英文空格，逗号或分号分隔开

### 导入一个模版

如果你曾导出一个模版到本地硬盘，或者从其它人那里收到一个模版的导出文件，你可以在这里将其导入

### 重置查询

回归缺省查询条件

![advance-query](https://cdn.jsdelivr.net/gh/cnshsliu/static.xhw.mtc/img/doc/template_advance_query.zh_CN.png)

## 标签区域

标签用于对模版进行分类，MTC 的标签有组织级和个人级两级，组织级的标签由管理员在管理界面中配置， 个人标签由个人针对单个模版进行配置。

- 组织级标签显示为圆角矩形 ![orgtags](../img/template_tag_orglevel.png)
- 个人级标签显示为药丸形状 ![personaltags](../img/template_tag_personal.png)

点击一个标签，浏览器将过滤出那些打了该标签的模版

按住 Shift 键，可以多选标签，MTC 将过滤出那些同时具有被选中标签的模版

## 最近使用模版

把最近使用过的模版列出来，方便快速选择经常使用的模板

## 过滤

根据模版名称、设计者来过滤模版

## 列表显示

列出符合查询条件的模版

![resulttable](https://cdn.jsdelivr.net/gh/cnshsliu/static.xhw.mtc/img/doc/tempplate_result_table.zh_CN.png)
在结果显示中，用户可以切换排序、选择每页显示个数，每行显示个数。
如果是管理员，还可以选择多个，并同时对多个模版进行删除等操作

## 流程操作

![actions](https://cdn.jsdelivr.net/gh/cnshsliu/static.xhw.mtc/img/doc/template_explorer_actions.zh_CN.png)

这些操作动作包括：

    - 查看该模版所启动的相关进程
    - 查看改模版相关的工作任务
    - 设置：对该模版进行设置
    - 编辑历史： 查看谁编辑、修改了这个模板
    - 执行计划： 定时重复启动这个流程
    - 删除这个模板
    - 流程挖掘：对流程的完成情况，执行效率等进行分析
    - 流程数据：导出该模版的所有进程的用户数据

## 流程设置

流程设置如下图所示

![Template Setting](https://cdn.jsdelivr.net/gh/cnshsliu/static.xhw.mtc/img/doc/template_setting.zh-CN.png)
其中，

### 描述信息

给流程添加描述说明，以便他人了解这个流程的设计目的，适用场景等

### 标签设置

给流程添加标签，用于归类查询，用户点击流程浏览器上部的标签时，按照标签对流程进行过滤

添加标签后，所有个人标签会显示在组织标签下方

### 指定作者

将流程作者指定为另一个人，直接输入该人的邮箱地址前缀即可

### 对谁可见

设定流程对哪些人可见。

如果不设置，则自动对组织内全体用户可见

新建流程时，流程自动只对作者本人可见，以防止在流程模版定稿之前他人直接适用。
