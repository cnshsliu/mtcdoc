# Workflow Designer

Workflow Designer provides graphical user interface to design a workflow by dragging and dropping.

![desginer](/img/workflow_designer.png)

On the left is the toolbox, click each tool to use it.

## Pointer <img src="/img/svg/POINTER.svg" width="24px" height="24px"/>

_Press ESC anytime to select Pointer._

Under Pointer mode, you are able to:

1. select a node or connect by clicking it.
2. open property window of node or connect by shift-clicking it
3. move a node by dragging it.
4. move a connection by clicking it while holding ALT key (Opt key on Mac OsX), release ALT key, then click on another node should be connected.
5. pan canvas by clicking on blank area of canvas then dragging it.

## Activity <img src="/img/svg/ACTION.svg" width="24px" height="24px"/>

_Press 1 anytime to select Activity_

An activity is a task need to be done by human.

### Operations

- Click on canvas to place an Activity node
- Shift-Click on an Activity to open it's properties
- Drag it to move to another location

### Participant

Define task participants with Role Definition String,

See [RDS page](/designer/rds) for details

### Instruction

Give some instructions to people who take part in this task

Simple html tag is allowed

Handlbars format is used to include process variables. If a previous node has a variable named "days", then, {{days.value}} can be included in instruction to embed it's value in instruction.

### Variables

define variable name, type etc.

- **Name**: variable name.
  - should be a valid javascript varible name, invalid name will make your workflow fails to run.
  - varialbe name prefix determin its input type:
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
    - "select\_" : an select type input.
    - "sl\_" : an select type input.
    - "sel\_" : an select type input.
    - "file\_" : an file type input.
    - "radio\_" : a radio type input.
    - "textarea\_" : a textarea type input.
    - "checkbox\_": a checkbox type input.
    - "cb\_": a checkbox type input.
    - any other name : a normal input.
- **Option**: the pickable options for this variable input
  - options should be delimited by semicolon (;), for example "option1;option2;option3";
  - options can also get from a pre-defined list.
    - A list is defined in a list group.
    - A list has it's own key in a list group.
    - "R:list_group_name" to get default items from a list group.
    - "R:list_group_name:key" to get items from a list group by key.
    - list can be cascaded To make cascaded list. you may:
      - use T:cascade_list_name, for example, you may have province list "select_A" defined as "R:province_list;T:select_B", then, you may define select_B as "R:city_list", then, once use pick a province from select_A, select_B will get the selected value from select_A, and use it as list key to refresh options for select_B, say, get all cities of the selected city.
- **Value**: the default value of this input.
  - for normal input, the default value will be set in the input box.
  - for select/checkbox/radio, the default value will be selected.
- **Label**: The label of this vairable.
- **Placeholder**: the placeholder for input or textarea
- **Break Row**: add a new line after this variable
- **ID**: give it an optional ID
- **Required**: this variable's value must be provided.

## Inform <img src="/img/svg/INFORM.svg" width="24px" height="24px"/>

- Press 2 at anytime to use Inform
  An Inform node is used to send message to people.

### Operations

- Click on canvas to place an Inform node
- Shift-Click on an Inform to open it's properties
- Drag it to move to another location

### Recipiants:

- Who will receive emails, define use [RDS](/designer/rds)

### Subject and Content:

- may use simple html or Handlebars to embed process variables

## Script <img src="/img/svg/SCRIPT.svg" width="24px" height="24px"/>

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

## Timer <img src="/img/svg/TIMER.svg" width="24px" height="24px"/>

A TIMER node is used to control process running time, the process only run through this node when

- From Start: how long after the start of the whole process
- From Now: how long after the invoking of this Timer node(end of previous node).
- Fix: Specific date and time

## Sub Process <img src="/img/svg/SUB.svg" width="24px" height="24px"/>

An sub-processs will be invoked to run, and the parent process will continue only when the sub-process has been completed.

sub-process's last return value will be taken as the return value from sub-process, parent process will use it to determine where to go after it.

An sub-process can also run in standalone mode, therefore, the parent process will not wait for it's completing.

## AND <img src="/img/svg/AND.svg" width="24px" height="24px"/>

An AND node will make process wait for completion of all it's precedent nodes.

## OR <img src="/img/svg/OR.svg" width="24px" height="24px"/>

Any precedent node is completed, an OR node will be went through, process will navigate to the following nodes of OR.

An AND node will make process wait for completion of all it's precedent nodes.

## Ground <img src="/img/svg/GROUND.svg" width="24px" height="24px"/>

A Ground node could have no folloing nodes, means the routing is grounded or sink.

## Connect <img src="/img/svg/CONNECT.svg" width="24px" height="24px"/>

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

### Cancel connecting

If you would like to cancel while connecting, double click on blank area of canvas, or press ESC

### Delete a connection

- Mouse over a connection, then press Backspace or Delete

### Give connection a value

- Hold Shift key, click on a connection, input is option value in the pop-up.
