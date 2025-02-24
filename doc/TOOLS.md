# Available Tools

This document provides a comprehensive list of all available tools in the Todoist Extended Server.

## Project Management Tools

### todoist_get_projects

Get a list of all projects:

* Lists all projects with their IDs and attributes
* Example: "Show all my projects"

### todoist_create_project

Create new projects:

* Required: name
* Optional: parent_id (for nested projects), color, favorite status
* Example: "Create project 'Work Tasks' with color blue"

### todoist_update_project

Update existing projects:

* Required: project_id
* Optional: name, color, favorite status
* Example: "Update project 'Work Tasks' to be a favorite"

## Section Management Tools

### todoist_get_project_sections

Get sections within a project:

* Required: project_id
* Lists all sections in the specified project
* Example: "Show sections in project 'Work Tasks'"

### todoist_create_section

Create new sections:

* Required: project_id, name
* Optional: order
* Example: "Create section 'In Progress' in project 'Work Tasks'"

## Task Management Tools

### todoist_create_task

Create new tasks with various attributes:

* Required: content (task title)
* Optional: description, due date, priority level (1-4), project_id, section_id
* Example: "Create task 'Team Meeting' in project 'Work' section 'Planning'"

### todoist_get_tasks

Retrieve and filter tasks:

* Filter by project, section, due date, priority
* Natural language date filtering
* Optional result limit
* Example: "Show high priority tasks in project 'Work' due this week"

### todoist_update_task

Update existing tasks using natural language search:

* Find tasks by partial name match
* Update any task attribute (content, description, due date, priority, project, section)
* Example: "Move meeting task to project 'Work' section 'In Progress'"

### todoist_move_task

Move tasks between projects and sections:

* Find tasks by name
* Optional: project_id, section_id
* Example: "Move task 'Documentation' to section 'Done'"

### todoist_complete_task

Mark tasks as complete using natural language search:

* Find tasks by partial name match
* Confirm completion status
* Example: "Mark the documentation task as complete"

### todoist_delete_task

Remove tasks using natural language search:

* Find and delete tasks by name
* Confirmation messages
* Example: "Delete the PR review task"

## Label Management Tools

### todoist_get_personal_labels

Get all personal labels:

* Lists all labels with their IDs and attributes
* Example: "Show all my labels"

### todoist_create_personal_label

Create new personal labels:

* Required: name
* Optional: color, order, is_favorite
* Example: "Create label 'Important' with color red"

### todoist_get_personal_label

Get a specific personal label:

* Required: label_id
* Returns detailed information about a specific label
* Example: "Show details of label 'Important'"

### todoist_update_personal_label

Update existing personal labels:

* Required: label_id
* Optional: name, color, order, is_favorite
* Example: "Update label 'Important' to color blue"

### todoist_delete_personal_label

Delete a personal label:

* Required: label_id
* Example: "Delete label 'Obsolete'"

### todoist_update_task_labels

Update the labels of a task:

* Required: task_name, labels (array of label names)
* Example: "Add labels 'Important' and 'Urgent' to task 'Review PR'"

## Parameter Precedence and Edge Cases

### Due Date Parameter Precedence

When creating or updating tasks, Todoist API has a specific precedence order when multiple due date parameters are provided simultaneously:

* When both `due_date` and `due_string` are provided, the API will prioritize the `due_date` parameter (the exact date in YYYY-MM-DD format).
* More specific date formats take precedence over natural language descriptions.
* The API does not reject requests with conflicting due date parameters but silently chooses the highest precedence parameter.

For best results and predictable behavior:
* Use only one due date parameter per request (`due_string`, `due_date`, or `due_datetime`).
* When using batch operations, ensure each task in the batch has consistent parameter usage.

### Batch Operations

All task tools support both single operations and batch operations through the `tasks` array parameter. When using batch operations:

* Empty arrays will be rejected with appropriate error messages.
* Operations continue processing even if some individual items fail.
* Results include detailed success/failure information for each item in the batch.
