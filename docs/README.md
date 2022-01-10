# Welcome to MetatoCome

MetatoCome is a Hyper Automation Platform as a Service.

Metatocome disptachs tasks to human and computer systems, you may use it in a tranditional workflow system way, it provides full-fledged workflow system functionalities. It is actually a modern Hyper-automation platform, it can be used to automate everything in an organizaiton that can be auomated.

## Basic Concept

MetatoCome is a PaaS service, developers write their own applications, no matter they are iOS App, Android APP, a H5 application with MatatoCome APIs to start a workflow, check worklist, provide UI for endusers to do their own work, or bridge MetatoCome to their own enterprise applications.

MetatoCome is also a SaaS sedrvice, if you don't want to develop your own UI, MetatoCome already has full-fledged user interface for enterprise users use it as a SaaS application.

Administrators or authorized workflow designer will design workflow template first, then, everyone in the organizaiton can initiate a workflow process, MetatoCome will dispatch task to either human or any IT system via Restful APIs, then MetatoCome receive and check feedback from human or system, decide how to route the process to next step based on the decision of human or system. These steps will continue until it run to the END node of the workflow template.

## Quick Start

### For personal use

Individual may use MetatoCome as a personal Get Things Done tool. Steps involved in this scenario are:

1. [Register](account/register.md)
2. Design workflow
3. Run workflow
4. Check worklist
5. Do tasks

### For small-team use

Small team may use MetatoCome to collaborate with internal team members or external collaborators.

A formal orgchart structure with formal position definition is normally unnecessary for a small team. Small team may use flexible Teaming insteadly. Teaming defines role-person mapping. you may define as many flexible team as required. you assign a task to a role in template, when initiate this template into a instance process you also pick a team, then MetatoCome will inteperate
the team and resolve which person will be responsible for the task based on the role-person mapping defined in the flexible team. Steps that may be involved in this scenario are:

1. [Register your team](account/register.md)
2. Invite people to join your team
3. Approve join applications
4. Define flexible team
5. Design workflow
6. Run workflow
7. Collaborating with worklist

### For organization use

An organizaiton normally has formal orgchart defined, people are located in diffrent OU (organizaiton unit) and holding different positions.

While flexible teaming is also available for organization users, you may find using RFS (Role Definition String) to locate people in your orgchart is a much more flexible way. Steps that may be involved in this scenario are:

1. [Register your org](account/register.md)
2. Import your orgchart
3. Design workflow
4. Run workflow
5. Collaborating with worklist
