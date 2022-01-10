# Template Explorer

MetatoCome Workflow Template explorer shows all templates your template library, as well as allowing management of workflow templates.

## User Interface

![Template Explorer](/img/template_explorer.png)

### Top buttons

There are three buttons on the top-right corner of template explorer.

- Create: show create template form
- Import : import a template from your local disk
- Reset Query: clear out any specified query criteria

#### Create a template

![Create template form](/img/template_create_form.png)

_Above: Create template form_

Give your new template an unique name, one more more tags separated by spaces, semicolons, or commas.

#### Import a template

If you have ever exported a template to your local disk, or received a exported template file from others, you may import it to your template library

#### Reset Query

Template explorer provides detailed criteria for searching, Reset Query is used to reset query critera by one-click

### Tags Area

Tags are used to categorize templates, organizaiton level tags are defined by administrators and available for everyone in your organizaiton, while personal tags are available individually.

- organizaitonal tags are displayed in a round corner rectangle, like ![orgtags](/img/template_tag_orglevel.png)
- personal tags are displayed in a pill, like ![personaltags](/img/template_tag_personal.png)

Click a tag, explorer will search those templates with the tag and display them in the result table at the lower part of explorer.

A template can have more than one tags, hold shift key while clicking a tag will make tag multi-selection. thus, MetatoCome will search for those templates which has all the highlighted tags.

### Recent Templates Area

List out recent viewed/started templates for quick accessing.
![recentstemplate](/img/template_recents.png)

### Extra Filter Area

Filter results with template name or author, use "Me" and "Any" to quickly toggle author of yourself and others

![extrafilter](/img/template_extra_filter.png)

### Result Table

List out any templates meet above requirements: tag, name filter, author etc.

![resulttable](/img/template_result_table.png)

- The first column is the link to template designer
- The second column is the author
- The third column is shortcut to start it
- The fourth column is a popup menu contains more actions available for the current template

![actions](/img/template_explorer_actions.png)

Those actions are:

    - Start it: start this workflow
    - See Workflows: list out workflow processes of this template
    - See Worklist: list out work items belong to this template
    - Add Description: show description form to add or modify template description.
    - Set TAGS: show tag editting form to add, modify or delete tags of this template
    - Delete This Template: as it says

### Pagination

![pagination](/img/pagination.png)

Use pagination to browser more results
