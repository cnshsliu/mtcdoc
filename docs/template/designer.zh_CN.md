# 工作流设计器

工作流设计器提供图形方式用户界面，用于设计一个流程

![desginer](../img/workflow_designer.png)

左侧的工具箱里的工具，用于切换当前工具，每种工具对应一种节点类型

## 指针 <img src="../img/svg/POINTER.svg" width="24px" height="24px"/>

_在编辑状态下任意时间按下 ESC 键即可回到指针._
在指针模式下，你可以：

1. 通过点击一个节点或连接，以选中它
2. 按住 SHIFT 的同时点击一个节点或连接，以打开它的属性窗口
3. 按住鼠标拖动一个节点
4. 在空白区域按下并拖动鼠标，以拖动画板

除指针以外的其它工具对应一个流程节点类型

## 节点类型

设计器支持多种节点类型，包括活动、通知、脚本、定时器、子流程、与、或、接地及连接

- 活动：指一个需要人来完成的动作
- 通知：指发送邮件给特定人
- 脚本：用于在流程中嵌入 Javascript 脚本，用于控制流程的运行逻辑，通过 WebAPI 调用运行在其它系统上的服务
- 定时器：用于暂停流程一段时间，当需要在一个节点完成以后，暂停一段时间再继续下一个节点时，在两个节点之间插入一个定时器即可。定时器在间隔时间后循环运行一段流程时也非常有用。
- 子流程：用于在一个流程中嵌入另一个流程；或者触发另一个流程的独立运行。
- 与： 是一个用于判断前序节点是否已经全部完成的逻辑控制
- 或： 是一个用于判断前序节点中任意一个已经完成的逻辑控制
- 接地： 用于标识当前路径的结束，无须运行到 END 节点
- 连接： 前后两个节点之间的连线。

## 节点属性

大部分节点类型都有属性，Shift-点击一个节点即可打开它的属性窗口，在没有 Shift 键的移动设备上，可以通过点击选中节点右上侧的小箭头图示来打开它的属性窗口。

连接也有属性，通过 Shift-点击来打开

## 节点 ID

每个节点都有一个自动生成的唯一 ID。
当我们在脚本中需要参考一个节点时，可能发现自动生成的 ID 不能表达意图，此时，最好的方法是把自动生成的 ID 修改为一个有业务意义的名字。

在节点的属性窗口中，输入自定义 ID，然后其右侧的“设置”按钮。这个按钮必须刻意点，否则，即便你点了属性窗口的确定按钮，ID 也不会被改变。

在设计器顶部菜单的最右侧，你可以通过切换 checkbox 来在设计器画布上显示和隐藏每个节点的 ID 信息

## 活动 <img src="../img/svg/ACTION.svg" width="24px" height="24px"/>

_任意时间按下 1 即可选中活动_

活动是指一个需要由人来完成的动作

- 点击画布上任意位置即可放置一个活动
- 按下鼠标可拖动节点到其它位置
- Shift-点击一个节点打开它的属性

** Title **

活动的名称中可嵌入流程参数值，用方括号括起来的变量名，连同方括号一起，将被替换为变量的值，如果变量不存在，替换后方括号也会被去除。

例如，如果流程中有一个变量名为"Interviewee_name", 它的值是"张三", 活动名称“请审批[Interviewee_name]的录用通知书”将被替换为“请审批张三的录用通知书”

** 参与人 **

可视使用“参与人定义字符串”（PDS）来定义活动有哪些人参与执行

详细信息请参考 [PDS page](/designer/pds)

** 投票 **

如果一个活动有多个参与人，此时每个参与人的决策可能不同，而这个节点的最终决策值可以使用投票模型来计算所得

<img src="https://cdn.jsdelivr.net/gh/cnshsliu/static.xhw.mtc/img/doc/vote_en.png" width="100px"/>
<img src="https://cdn.jsdelivr.net/gh/cnshsliu/static.xhw.mtc/img/doc/vote_zh-CN.png" width="100px"/>

投票模型包括：

- 依最后一人的选择:

  > 总是使用最后一个人的选择为投票结果

- 依最多人的选择

  > 用最多人的选择作为投票结果

- 依最少人的选择

  > 用最少人的选择作为投票结果

- 选择全部一样，否则

  > 如果所有人选择都一样，则这个唯一选择作为投票结果
  > 否则，使用“否则”设置为投票结果

- 某一选择超过比例，否则

  > 使用比例超过所设置数值的选项为投票结果
  > 否则，使用“否则”设置为投票结果

- 只要有选，否则依最后

  > 只要有人选择某个选项，则投票结束，投票结果为这个值
  > 否则，依最后一个人的选择为投票结果

- 只要有选，否则依最多

  > 只要有人选择某个选项，则投票结束，投票结果为这个值
  > 否则，依最多人的选择为投票结果

- 只要有选，否则依最少

  > 只要有人选择某个选项，则投票结束，投票结果为这个值
  > 否则，依最少人的选择为投票结果

- 只要有选，否则依统一选择 或最多

  > 只要有人选择某个选项，则投票结束，投票结果为这个值
  > 否则，依全体人的选择为投票结果
  > 如果，全体人员选择不统一， 则使用最多人的选择为投票结果

- 只要有选，否则

  > 只要有人选择某个选项，则投票结束，投票结果为这个值
  > 否则，使用“否则”设置为投票结果

** 任务要求说明 **

参与人接收到任务后，需要一些指引来对当前工作要求作出说明

说明中可以使用部分 HTML 标签，也可以使用 Handlebars 模版方式或者方括号中的变量名方式嵌入流程参数

- HTML 标签
  安全起见，只有部分 HTML 标签是可以使用的，包括：

  > "b", "i", "em", "strong", "a", "blockquote", "li", "ol", "ul", "br", "code", "span", "sub", "sup", "table", "thead", "th", "tbody", "tr", "td", "div", "p", "h1", "h2", "h3", "h4"

- Handlebars

  Handlebars format is used to include process variables. If a previous node has a variable named "days", then, {{days.value}} can be included in instruction to embed it's value in instruction.

#### Var in square brackets

[var_name] will be replaced with var value.

You may also have {{days.title}} {{days.type}} etc. included if required.

## Variables

define variable name, type etc.

### **Name**:

Variable name should be a valid javascript varialbe name,
that means, a variable name should start with an alphabetic, or underscore, followed by one to many alphabetic or underscore or numbers.
invalid name will make your workflow fails to run.

### **Variable Type**:

Variable name with specific prefix also indicates variable types, determing how Metatocome SaaS show it to end-users.

- "email\_" : an email type input.
- "password\_" : an password type input.
- "url\_" : an url type input.
- "range\_" : an range type input.
- "number\_" : an number type input.
- "dt\_" : an datetime type input.
- "datetime\_" : an datetime type input.
- "date\_" : an date type input.
- "time\_" : an time type input.
- "color\_" : an color type input.
- "search\_" : an search type input.
- "select\_" : a select type input.
- "sl\_" : a select type input.
- "sel\_" : a select type input.
- "file\_" : an file type input.
- "radio\_" : a radio type input.
- "textarea\_" : a textarea type input.
- "ta\_" : a textarea type input.
- "checkbox\_": a checkbox type input.
- "cb\_": a checkbox type input.
- "ou\_": an Organization Unit Selector
- "usr\_": an user ID type input
- "user\_": an user ID type input
- "tbl\_": an table type input.
- any other name : a normal input.

#### OU selector

A variable starts with "ou\_" will provide user a selection box of the current orgchart. The orgchart is configurable by people who have the correspoinding access right, normally, the MTC admin of your organizaiton.

An OU selector variable named as "ou_varname", it can have opitons like "top_ou_id;[yes|no]", the string before ';' is the ouid of the first organizaitonal unit, the string after ';' is used to indicate whether the selection list should include the top item or not.

#### User selector

A variable starts with "usr\_" or "user\_" provides user with a input box for input and validate user id, MTC keep validating while you are inputting, and give you feedback of the result.

#### File uploader

A variable starts with "file\_" will show the user a file drop area, use can drag a file and drop it onto the drop area to upload a local file to MTC. Later, other users could view it, or download it.

#### Internal Variables

Following variables you may use directly without being explicitly defined.

Process level variables:

- **starter**: the uid of the process starter,
- **starterCN**: the Name of the process starter.
- **ou_SOU**: the OU code of the process starter,
- **ou_user_XYZ**: the OU code of a user_XYZ variable

System variables:

- **$$isoWeek**: No. of week in year (number)
- ** $$isoWeeksInISOWeekYear**: How many weeks in this year
- ** $$isoWeekYear**: Year (based on full week)
- ** $$isoWeekDesc**: Description of isoWeek such as W1, W13...
- ** $$isoWeekDescFull**: Full description, like W1/52-2022

These internal varaibles are aslo available for:

- Workflow Context variables display,
- Handlebars in comments input, for example: "{{starterCN.value}}"
- Activity title, for example: "Activity started by [starterCN]"

#### **Selection Option**

For a variable named like "select\_", "sel\_", "sl\_", or "ou\_",

- options should be delimited by semicolon (;), for example "option1;option2;option3";
- For "ou\_" variable, the first option will be used as the top OU id, the second is "yes" or "not", whether to include the top itself.
- options can also get from a pre-defined list.
  - A list is defined in a list group.
  - A list has it's own key in a list group.
  - "R:list_group_name" to get default items from a list group.
  - "R:list_group_name:key" to get items from a list group by key.
- list can be cascaded To make cascaded list. you may:
  - use T:cascade_list_name, for example, you may have province list "select_A" defined as "R:province_list;T:select_B", then, you may define select_B as "R:city_list", then, once use pick a province from select_A, select_B will get the selected value from select_A, and use it as list key to refresh options for select_B, say, get all cities of the selected city.

#### **Table**

A table allow users to input values with a table row by row, column by column.
Table variable name starts with tbl\_

Table columns are defined with a string delimited by |,

- individual column can have prefix to define its type:
  - "date\_" (for date input),
  - "dt\_" (for date time input).
  - "sel\_" (for selection)
    - Options for this selection are given as (OPT1:OPT2:OPT3)
- column's title can be defined with [title=TITLE], or the variable name without prefix. for example, variable dt_THIS with have a title THIS automatically.
- default value can be defined with [default=DEFAULT_VALUE],
- if average value of the column is required, mark it with [avg]
- if sum value of the column is required, mark it with [sum]
- get how many days between two date type column, define it with =datediff function.
- get how many days lasting between two datetime, define it with =lastingdays function.

##### Example:

```
date\_开始时间[title=开始日期]|date\_结束时间|从哪里[default=机场]|到哪里[default=公司]|sel\_出行方式(飞机:高铁:长途汽车:出租车)[default=高铁]|=datediff(date\_开始时间,date\_结束时间)+1[title=出差天数(天)][default=0][avg]|dt\_开始时点|dt\_结束时点|=lastingdays(dt\_开始时点,dt\_结束时点,0.5)[title=请假天数][default=0][sum]|number\_报销金额[sum][avg]
```

The table above has following columns:

- 开始时间：
  - 类型：日期
  - Title: 开始日期
- 结束时间
  - 类型：日期
  - Title: 结束时间
- 从哪里
  - 缺省值：机场
- 到哪里
  - 缺省值：公司
- 出行方式
  - 类型：选择列表
  - 可选项：飞机，高铁，长途汽车，出租车
  - 缺省值：高铁
- 出差天数（天）
  - 类型：公式
  - 值：开始日期，与结束日期的天数差别+1，如为同一天，则值为 1.
  - 缺省值：0
  - 计算平均值
- 开始时点
  - 类型：datetime
- 结束时点
  - 类型：datetime
- 请假天数
  - 类型：公式
  - 值：开始时点，与结束时点的差别，规整到 0.5 天
  - 缺省值：0
  - 计算总和
- 报销金额
  - 类型：数字
  - 求总
  - 求平均

### **Value**

the default value of this input.

- for normal input, the default value will be set in the input box.
- for select/checkbox/radio, the default value will be selected.

### **Formula**

You may use formula for a variable which we have a value the same as the the result of its formula.

Formula is defined in the variable's value field, starts with an "=".

Formula is Javascript expression. try simple expression is strongly recommanded.

Examples:

```
=first_name + " " + last_name
```

If first_name varialbe has a value of "John", last_name is "Smith", then the result should be "John Smith".

```
=first_name.substring(1)
```

If first_name is "John", the result will be "ohn";

### **Label**

The label of this vairable.

### **Placeholder**

the placeholder for input or textarea

### **Break Row**

add a new line after this variable

### **ID**

give it an optional ID

### **Required**

this variable's value must be provided.

### **Visible**

use PDS to define whom this var should be visiable to

## About Visible

Sometime, some sensitive data might should be kept secret from some participants even they have been involved in the process. For instance, in a interview process, the offered salary may not be able to seen by interviewer, only HR and manager could see it, thus, we may use PDS to make this happend.

## Inform <img src="../img/svg/INFORM.svg" width="24px" height="24px"/>

- Press 2 at anytime to use Inform
  An Inform node is used to send message to people.

### Operations

- Click on canvas to place an Inform node
- Shift-Click on an Inform to open it's properties
- Drag it to move to another location

### Recipiants:

- Who will receive emails, define use [PDS](/designer/pds)

### Subject and Content:

- may use simple html or Handlebars to embed process variables

## Script <img src="../img/svg/SCRIPT.svg" width="24px" height="24px"/>

- Press 3 at anytime to use Script

### Operations

- Click on canvas to place an Script node
- Shift-Click on an Script to open it's properties
- Drag it to move to another location

### Sync Mode

Run script in sync mode

### Async Mode

Run script in async mode, external program callback to MetatoCome later to make it continue.

### Code

Embed any javascript code in this node.

#### Return value

```

ret = RET_VALUE;

```

Return value is used as routing option to decide where to go after this script node.

#### Insert any variable

```

kvar(var_name, var_value, var_label)

```

After that, thsi variable named 'var_name' is available for following process.

\*\* If a varialbe named "var_name" exists, it's value will be overwrite with this one.

#### Get value of variable.

```

kvalue(var_name)

```

####Set inner Team
Dynamically set a team for use later.

```

setRoles({
SGT: "ab@email.com",
DIRECTOR: "cd@email.com",
});

```

For steps after this script, any task assigned to role 'SGT' will go to a person whose email is "ab@email.com", any task assigned to role "DIRECTOR" will go to a person whose email is "cd@email.com"

## Timer <img src="../img/svg/TIMER.svg" width="24px" height="24px"/>

A TIMER node is used to control process running time, the process only run through this node when

- From Start: how long after the start of the whole process
- From Now: how long after the invoking of this Timer node(end of previous node).
- Fix: Specific date and time

## Sub Process <img src="../img/svg/SUB.svg" width="24px" height="24px"/>

An sub-processs will be invoked to run, and the parent process will continue only when the sub-process has been completed.

sub-process's last return value will be taken as the return value from sub-process, parent process will use it to determine where to go after it.

An sub-process can also run in standalone mode, therefore, the parent process will not wait for it's completing.

## AND <img src="../img/svg/AND.svg" width="24px" height="24px"/>

An AND node will make process wait for completion of all it's precedent nodes.
并节点需要等待其前序节点，全部完成，才会进行后续工作。
如果流程中存在循环，AND 节点总是能做到使用最新一轮循环的结果。
例如，A 节点指向 B 和 C 节点，B 和 C 聚合到 AND， AND 后是 D， D 指向 END，D 也有一个路径指向到 A。 那么，当 BC 完成，AND 通过，D 完成后流向 A，开始新一轮。 新一轮中，B 完成，此时，即便上一轮的 C 有完成记录，AND 也不会通过，它会等待在新一轮中的 C 完成。

** 注意 **
对于一些复杂的流程，AND 的运行可能与你预期不同，比如，其中一个分支中的某个节点，在特殊条件下，流向了另一个分支，这样，上一个分支就没有机会流向到 AND。流程就会停在 AND 节点上。

** 注意 **
对于复杂流程，建议使用脚本节点来实现复杂并行控制。 参见脚本一节

## OR <img src="../img/svg/OR.svg" width="24px" height="24px"/>

只有前面有一条路径通过，OR 节点就会被通过。
其它路径上的工作会被忽略。

这一点与“衔接”节点不同，“衔接”节点的作用只是用于衔接线路，对运行逻辑不作控制

## Ground <img src="../img/svg/GROUND.svg" width="24px" height="24px"/>

A Ground node could have no folloing nodes, means the routing is grounded or sink.

## Connect <img src="../img/svg/CONNECT.svg" width="24px" height="24px"/>

Connect two nodes to define a route between them.
Click one node, then click another node, a curved line will be drawn between them.
Shift-clicking on a connection will bring up the connection property window, simply give the connection a Case Value (a string), the Case Value will be displayed alongside of the connection line.
this case value will be used to determine whether or not this route will be taken or not after it's FROM node has been completed.

A connection between two nodes has direction, it always point from one node (A) to another (B), means that the workflow should run from task A to task B.

A connection can have option, option define the route. for example, there is one connection between A to B, there is another connection between A to C, if we give A to B an option value 1, and give A to C an option value 2, then, if A return 2, the workflow will run to C, B will not be routed to. if A is an activity, the user who do that activity will be presented with option 1 and 2 to decide. if A is a script, you may use "ret=2" to return 2 from node A.

### Build a connection

- Select CONNECT tool , or simple press 9, the CONNECT tool will be highlighted
- Click on the first node A,
- Click on the second node B

### Move a connection

- Hold Alt key (MAC: Opt), click on the first half of a connection then pick another node to re-select its' staring node
- Hold Alt key (MAC: Opt), click on the second half of a connection then pick another node to re-select it's ending node

or:

- Mouse over a connection, press "cb" to move it's starting point;
- Mouse over a connection, press "ce" to move it's ending point;

### Cancel connecting

If you would like to cancel while connecting, double click on blank area of canvas, or press ESC

### Delete a connection

- Mouse over a connection, then press Backspace or Delete

### Give connection a value

- Hold Shift key, click on a connection, input is option value in the pop-up.
- While pointing at a connection, press 'ct' to clear it's value
- While pointing at a connection, press Ctrl-V to paste a value, after press Ctrl-C on an existing connection.

## Editing

Desginer support copy,paste, cut etc., as well as shortcut keys to ease your opertions..

### Nodes Copy and Paste

While mouse is hovering a node , press Ctrl-C (Win) / Cmd-C (Mac) to copy it, move mouse to any position on the canvas, proess Ctrl-V (Win) / Cmd-C (Mac) to paste it.

While mouse is hovering a node, press Ctrl-X (Win) /Cmd-X (Mac) to cut it, later, you may paste the cutted node at another location wil Ctrol-V (Win) /Cmd-V (Mac)

### Connext Routing Text

While mouse is hovering a connection , press Ctrl-C (Win) / Cmd-C (Mac) to copy it's routing label, move mouse to hover another connection, proess Ctrl-V (Win) / Cmd-C (Mac) to paste it.

### Keyboard shortcut

- d: Mouse over a node or a connection, press d to delete it.
- cb: Mouse over a connection, press "cb" to move it's starting point
- ce: Mouse over a connection, press "ce" to move it's ending point
- gt: Mouse over a node, press "gt" to link it to another node
- ct: Mouse over a connection, press "ct" to clear its text

### Copy / Cut / Paste

- Ctrl-C / Cmd-C to copy mouse overing node or connect
- Ctrl-X / Cmd-X to cut node or connect
- Ctrl-V / Cmd-V to paste nord or connect text

- To make a new copy of an exiting node
  - Move mouse to source node
  - press Ctrl-C on Windows or Cmd-C on Mac
  - Move mouse to blank area of the canvas
  - Press Ctrl-V on Windows or Cmd-V on Mac
- To make a node (destination node) the same as another (source node)
  - Move mouse to source node
  - press Ctrl-C on Windows or Cmd-C on Mac
  - Move mouse to the destination node
  - Press Ctrl-V on Windows or Cmd-V on Mac
- To make a connection (destionation) text the same as another (source connection)
  - Move mouse to source connection
  - press Ctrl-C on Windows or Cmd-C on Mac
  - Move mouse to the destination connection
  - Press Ctrl-V on Windows or Cmd-V on Mac

## Select

Select one: click the node to selecte it.

Select many: click the nodes while holding Win key or Command key

Select all: Ctrl-A or Command-A.

Select with mouse: hold down Shift-key, click and move mouse to select one to many nodes. Shift-Meta to add more to selected.

## Move

To move one signle node, click on it, move mouse while holding.

To move many nodes, click on them while holding Win key on Windows or Command key on Mac, while those nodes are in "selected" status, drop any of them to antoher location, other selected will move simultaneously.
To move all nodes, press Ctrl-A or Command-A to select all nodes, then click on one selected node to move them all together.
