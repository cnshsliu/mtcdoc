# PDS: Participant Definition String

A PDS(Participant Definition String) is used to resolve who will participate a task.

A PDS is composed of one to many PDSP(PDS Parts) delimited with empty characters, a semicolon or a comma.

Task always go to workflow process starter if PDS is absent or blank.

PDSPs are strings represent a User ID, a Team Participant, a Team, a Peer Query, a Leader Query or a Staff Query.

## Specific User

a User ID PDSP starts with "@", like "@steve" where steve should have an MTC account as steve@emailDomain, the emailDomain of steve is the same as emailDomain of the current user.

## Flexible Team

A Team Participant PDSP is specified as "role_name", a role named "role_name" will be used to resolve participants, the role-participants relations are defined in the process-level team specified at current workflow process starting, or in the PDS-level team specified in the current PDS. PDS-level team is used when it exits. last PDS-level team is used when multiple PDS-level teams exist.

A Team PDSP is specified as "T:team_name".

## Internal Role

Please a script node in the template, write code to specify following role participant, the following works will use these dynamic definition. For example,

```
// read Hyperflow Developer's Guide for details
setRoles({
SGT: "userA@company.com",
DIRECTOR: "userB@company.com",
});
ret='DEFAULT';
```

If one following work's PDS is "SGT", then this work will be delivered to "userA@company.com".
If another following work's PDS contains "DIRECTOR", then "DIRECTOR" will be inteperated to "userB@company.com".

## Peer

A Peer Query PDSP starts with "P:" followed by one or many positions separated by colon. positions are searched at the current org level of the user. We can say they are positions in the current deparments.

## Leader

A Leader Query PDSP starts with "L:" followed by one or many positions separated with colon, positions are searched upwards starting from the current org level all the way to the root level. Dor example, let's say the orgchart is "Root-BU1-Department1", and people are: "Root-Lucas-CEO", "BU1-Steve-VP:CFO", "Department1-John-Director", and the current user is "Department1-Lisa", then, a "P:Director" will be resolved as John, a "P:Director:VP" wil be resolved to John and Steve. a "P:CFO" will be
resolved to Steve. A Postion Query is useful in the approval liking scenarios where some specific positions are normally required to approve something.

## Query

An Staff Query PDSP starts with "Q:", which is the most flexible PDSP which can replace Peer or Leader PDSP. A Staff PDSP is defined as '&' separated query parameters, a query parameter is defined as: "OU_regexp/positions_separated_by_colon", like

```
   ouReg1/pos1:pos2&ouReg2/pos3:pos4
```

which means, find people who hold position of pos1 in ouReg1, who hold position of pos2 in ouReg1, who hold position of pos3 in ouReg2, and who hold position of pos4 in ouReg2.

ouReg1 and ouReg2 are Regexp.

### Same OU Matching

if "ouReg" is ommitted in a query parameter, for exmaple: "/pos1:pos2", people who in the same department of the current user and hold positions of pos1 and pos2 will be resolved to. The same as what a Peer Query PDSP does.

### Upwards One Matching

To get the first matching people of the specified position searching upwards, use double slash: '//', for example, "Q:000010000200003//pos", if there is 'pos' in department '000010000200003', use it, or else, seach 'pos' in department '0000100002', if exist, use it, if not, continually search 'pos' in department '00001', exist? use it, no? search 'root' for 'pos', exist? use it, no? use the process starter as 'pos' participants.

### Upwards All Matching

To get all matching people of the specified position searching upward, use three slash: '///', for example, 'Q:000010000200003///pos', if Sam is 'pos' in '00001000200003' , Steve and Angela are 'pos' in '0000100002', Paul is 'pos' in '00001', Jenny is 'pos' in 'root', then 'Q:000010000200003///pos' will resolve to all of them: Sam, Steve, Angela, Paul and Jenny.

### All Matching

if "ouReg/" is ommitted in a query parameter, for example, "pos1:pos2", or use "\*" as ouReg, for example, "\*/pos1:pos2", people who hold positions of pos1 and pos2 in the whole organization will be resolved to.

### 'staff'

use 'staff' position to query all staff who have no specified positional role.

### 'all'

use 'all' to get all people of an OU, for example, "Q:DEP2/all" will get all people in DEP2 and its lower level department

### Variables

You may include variable values in PDS. the format is "[var_name]", the [var_name] will be repalced with the value of var_name. For example, if there is a var named "dep" which has a value of "finance", then PDS "Q:[dep]/director" will be replace with "Q:finance/director".
There are several internal variables are always available, they are:

- starter:
  the email of the starter
- starterCN
  the name of the starter
- ou_SOU
  the OU code of the starter

So, "Q:[ou_SOU]/director" will always point to "director" in starter's own department.

## Exclude operator:

"-" stands for "exclude operator", it will exlude matched users.

```
Q:*/all;-Q:G*/all
```

Will query all staffs first, then exlude those staffs in OU which ou_id starts with "G"

## Examples:

```
director
```

Resolve to:

- people who take the role of director in the process-level team. (director)

```
director:facilitator;T:TeamA;teacher:T:TeamB
```

Resolve to:

- people who take the role of director in the team of TeamB. (director, T:TeamB. TeamA is ignored since TeamB is the last PDS-level team)
- people who take the role of facilitator in the team of TeamB. (director, T:TeamB)
- people who take the role of teacher in the team of TeamB. (director, T:TeamB)

```

director;T:TeamA
```

Resolve to:

- people who take the role of director in the team of TeamA. (director, T:TeamA)

```
director;@steve
```

Resolve to:

- people who take the role of director in the process-level team (director)
- a user who's email is
  steve@compnay_email_domain (@steve)

```
director;@steve;P:director
```

Resolve to:

- people who take the role of director in the process-level team (director)
- a user who's email is steve@your_email_domain (@steve)
- users who hold director position in users' department (P:director)

```
director;@steve;P:director:leader
```

Resolve to:

- people who take the role of director in the process-level team (director)
- a user who's email is steve@your_email_domain (@steve)
- users who hold director position in users' department (P:director)
- users who hold leader position in users' department (:leader)

```
director;@steve;L:director:CEO
```

Resolve to:

- people who take the role of director in the process-level team (director)
- a user who's email is steve@your_email_domain (@steve)
- users who hold director position in users' department and upper level departments (L:director)
- users who hold CEO position in users' department and upper level departments (:CEO)

```
director;@steve;L:director:CEO;Q:FINAN/Director&LAWDP/Director
```

Resolve to:

- people who take the role of director in the process-level team (director)
- a user who's email is steve@your_email_domain (@steve)
- users who hold director position in users' department and upper level departments (L:director)
- users who hold CEO position in users' department and upper level departments (:CEO)
- Director of department FINAN (Q:FINAN/Director)
- Director of department LAWDP (LAWDP/Director)

```
director;@steve;L:director:CEO;Q:FINAN/Director&LAWDP/Director&/AA:timekeeper
```

Resolve to:

- people who take the role of director in the process-level team (director)
- a user who's email is steve@your_email_domain (@steve)
- users who hold director position in users' department and upper level departments (L:director)
- users who hold CEO position in users' department and upper level departments (:CEO)
- Director of department FINAN (Q:FINAN/Director)
- Director of department LAWDP (LAWDP/Director)
- AA of current depargment (/AA)
- timekeeper of current deprtment (:timekeeper)

```
director;@steve;L:director:CEO;Q:FINAN/Director&LAWDP/Director&/AA:timekeeper&CFO:CTO
```

Resolve to:

- people who take the role of director in the process-level team (director)
- a user who's email is steve@your_email_domain (@steve)
- users who hold director position in users' department and upper level departments (L:director)
- users who hold CEO position in users' department and upper level departments (:CEO)
- Director of department FINAN (Q:FINAN/Director)
- Director of department LAWDP (LAWDP/Director)
- AA of current depargment (/AA)
- timekeeper of current deprtment (:timekeeper)
- CFO
- CTO

```
director;@steve;L:director:CEO;Q:FINAN/Director&LAWDP/Director&/AA:timekeeper&CFO:CTO
```

Resolve to:

- people who take the role of director in the process-level team (director)
- a user who's email is steve@your_email_domain (@steve)
- users who hold director position in users' department and upper level departments (L:director)
- users who hold CEO position in users' department and upper level departments (:CEO)
- Director of department FINAN (Q:FINAN/Director)
- Director of department LAWDP (LAWDP/Director)
- AA of current depargment (/AA)
- timekeeper of current deprtment (:timekeeper)
- CFO
- CTO

```
Q:*/Director:AA
```

Resolve to:

- Director of any department
- AA of any department

```
Q:*/一面:二面:三面:终面
```

Resolve to:

- 所有面试官

```
Q:/staff
```

Resolve to:

- Those who hold no special postion of the current department

```
Q:DEP1/staff
```

Resolve to:

- Those who hold no special postion of DEP1 department

```
Q:DEP1/all
```

Resolve to:

- All people in DEP1 department

```
Q:*/all
```

Resolve to:

- all people of your organizaiton

```
Q:DEP1//AA
```

Resolve to:

- first matched AA in DEP1 and all upper level OU

```
Q:DEP1///AA
```

Resolve to:

- all matched AA in DEP1 and all upper level OU

```
Q:[ou_SOU]/director
```

Revoled to:

- The 'director' in starter's department

```
Q:[ou_SOU]/[position]
```

Resolved to:

- [position] will be replaced to the value of 'position' var, if the value if 'leader', will be the leader of starter's department
