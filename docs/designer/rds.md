# RDS: Role Definition String

A RDS(Role Definition String) is used to resolve who will participate a task.

A RDS is composed of one to many RDSP(RDS Parts) delimited with empty characters, a semicolon or a comma.

Task always go to workflow process starter if RDS is absent or blank.

RDSPs are strings represent a User ID, a Team Role, a Team, a Peer Query, a Leader Query or a Staff Query.

a User ID RDSP starts with "@", like "@steve" where steve should have an MTC account as steve@emailDomain, the emailDomain of steve is the same as emailDomain of the current user.

A Team Role RDSP is specified as "role_name", a role named "role_name" will be used to resolve participants, the role-participants relations are defined in the process-level team specified at current workflow process starting, or in the RDS-level team specified in the current RDS. RDS-level team is used when it exits. last RDS-level team is used when multiple RDS-level teams exist.

A Team RDSP is specified as "T:team_name".

A Peer Query RDSP starts with "P:" followed by one or many positions separated by colon. positions are searched at the current org level of the user. We can say they are positions in the current deparments.

A Leader Query RDSP starts with "L:" followed by one or many positions separated with colon, positions are searched upwards starting from the current org level all the way to the root level. Dor example, let's say the orgchart is "Root-BU1-Department1", and people are: "Root-Lucas-CEO", "BU1-Steve-VP:CFO", "Department1-John-Director", and the current user is "Department1-Lisa", then, a "P:Director" will be resolved as John, a "P:Director:VP" wil be resolved to John and Steve. a "P:CFO" will be
resolved to Steve. A Postion Query is useful in the approval liking scenarios where some specific positions are normally required to approve something.

An Staff Query RDSP starts with "Q:", which is the most flexible RDSP which can replace Peer or Leader RDSP. A Staff RDSP is defined as '&' separated query parameters, a query parameter is defined as: "OU_regexp/positions_separated_by_colon", like

```
   ouReg1/pos1:pos2&ouReg2/pos3:pos4
```

which means, find people who hold position of pos1 in ouReg1, who hold position of pos2 in ouReg1, who hold position of pos3 in ouReg2, and who hold position of pos4 in ouReg2.

ouReg1 and ouReg2 are Regexp.

if "ouReg" is ommitted in a query parameter, for exmaple: "/pos1:pos2", people who in the same department of the current user and hold positions of pos1 and pos2 will be resolved to. The same as what a Peer Query RDSP does.

if "ouReg/" is ommitted in a query parameter, for example, "pos1:pos2", people who in the same department or the upper level orgchart of the current user and hold positions of pos1 and pos2 will be resolved to. The same as what a Leader Query RDSP does. So, 'Q:CEO:CTO' and 'L:CEO:CTO' will be resolved to the same people.

## Exaples:

```
director
```

Resolve to:

- people who take the role of director in the process-level team. (director)

```
director:facilitator;T:TeamA;teacher:T:TeamB
```

Resolve to:

- people who take the role of director in the team of TeamB. (director, T:TeamB. TeamA is ignored since TeamB is the last RDS-level team)
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
