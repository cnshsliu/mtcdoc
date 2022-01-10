# Work Page

Work page provide user interface to end users to complete their work or give feedback.

![workpage](/img/workpage.png)
User may:

- Input value as required;
- Leave comments for following steps
- Make decision (for example, approve or reject an application)
- Push back a task to it's precedent steps
- Revoke a task if it has not been done
- Create any adhoc task to anyone in your organizaiton.
- Transfer current task to anohter one if it is transferable.
- Check rehearsal information
- Check process information
- Reveiw process logs

## Input

Any label with a \* means it's value must be provided.

## Comments

Leave comments, may contain simple HTML tag, and embed process variables with {{var_name.label}} {{var_name.value}} {{var_name.type}} {{var_name.name}}.

use @somebody to send notification to that person.

## Decisions

If the connects coming from this node has case value, the case values will be provided for user to choose here.

For example, when we design the template, nodeA points to nodeB and nodeC, the connect between nodeA and nodeB has a case value of "Approve", the connect between nodeA and nodeC has a case value of "Reject", then, on the work page of nodeA, user will be provided two buttons to choose, one labled "Approve", another labled "Reject".

In the above exmaple illustration, there are two decisions, "I am okay with it" and "I reject".

## Push back

Push task back to previous nodes.
If you are an approver, sometimes, you may find an employee didn't give enough information when the process come to you, then you could push the task back to allow him/her to provide more information.

The Push back button is only available when a task status is running

## Revoke.

If you are an employee and applying leave, when you find by yourself you didn't give enough information, you could revoke an approval task, as long as your manager has not complete it.

The Revoke button is only available when a task status is running and you are the owner of it's previous task.

## Create adhoc task

While you are on the work page, you may realize that you need to ask someone else (or yourself) to do something, You could create an adhoc task for this sort of requirement.

![createadhoc](/img/createadhoc.png)

You could specify Adhoc task's participants with [RDS](/designer/rds.md). However, you may send to wrong people accidentally or imprudentyly, to prevent it's happening, we have a Check button to show who are the right people, and give you two buttons to select.

## Transfer Work

You may transfer your work to another colleague, if the task is tranferable.

## Rehearsal Information

Have an insight of what the workflow engine see this task, such as doable or not, status, revocable or not, what's the Role and to whom the role has been assigned.

## Process context

A brief introduction of the the process to which this task belong.

## Work log

The current task is highlighted in the work log.

Click on participant in a work log to go to that task.

## For Developer

Developer might integrate work page in to your own application with MetatoCome APIs, and provide your own User Interface to your user.
