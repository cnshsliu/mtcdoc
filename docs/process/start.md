# Start a workflow process

Once a workflow is started, a new workflow process is initiated, and tasks will be disptached to people or systems.

<img src="/img/start_workflow.png" width="300"/>

## Simply Start

Just click "Start it" or "Rehearsal" without any optional context information, a Workflow could start:

- With a default title since you don't give it one, default title is "Your user name/template name".
- No PBO
- Flexible team role will be resolved to the starter (you, normally) since you don't give it a team.

## Full Context Start

So, to start a full-fledged workflow process, normally, you should provid extra contextual information:

### PBO

PBO means Primary Business Object, it's a URL pointing to any meaningful object within your business context. it could be a Project Plan, a PRD, a UED Design Doc, a Git checkin request, a IT maintainance doc etc.

With PBO presents, people normally will understand the process as it is surrounding the PBO, the process is fully dedicated to this PBO, that is what the word "Primary" means.

When users receive a new work item, the PBO will alwasy be available for them to review. they may discuss it, modify it with the support of other IT tools, when they finish their work, they click "Done" in MetatoCome.

We simple use URL to present PBO but do not hosting PBO in MetatoCome itself, is trying to make the integeration among MetatoCome and other IT systems much more flexible and secure.

The authoring, authorization, and authentication of PBO is fully managed by it correspoinding IT system, no matter who will receive a task with a PBO bundled, the right of them to access the PBO is fully managed by that IT system, not MetatoCome, this way, we make it much secure.

If you use MetatoCome in a Internet environment (MetatoCome can be deployed as a public SaaS, or a private cloud host, or a pure intranet host). you rest assured to have an Intranet PBO within your workflow process, since the access right is managed by your Intranet applicaiton not MetatoCome.

### Workflow Title

Give it meaningful process title, will make life easier later.

### Start with team

If you'd like to assign task to people flexibly, you may use teaming machenism. MetatoCome will inteperate assignee from role mapping of a flexible team.

Recent used team will be displayed for easy input.

## Rehearsal

Starting a not-fully-test-workflow may make mass and confuse people, so we suggest to use Rehearsal first.

The diffrences of Rehearsal and Start include:

1. While all tasks assigned to other people, the Starter always get them in his/her own worklist, and can do it by himself/herself.
2. All emails to other people will not be sent out to reduce confusion.
3. Rehearsal information will be availale on the work page.
4. Variable details will be visible on the work page for debug purpose
