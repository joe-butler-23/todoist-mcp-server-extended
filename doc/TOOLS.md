# Todoist MCP Server Tools

This document describes the available tools in the Todoist MCP Server. These tools allow interacting with Todoist projects, sections, tasks, and labels.

## Task Tools

The task tools have been significantly enhanced compared to the base Todoist API to optimize for LLM usage, batch operations, and input/output handling.

### todoist_create_task

Create one or more tasks in Todoist with full parameter support.

Single task:

- `content` (string, required): The content/title of the task
- `description` (string, optional): Detailed description of the task
- `project_id` (string, optional): ID of the project to add the task to
- `section_id` (string, optional): ID of the section to add the task to
- `parent_id` (string, optional): ID of the parent task for subtasks
- `order` (number, optional): Position in the project or parent task
- `labels` (array of strings, optional): Array of label names to apply to the task
- `priority` (number, optional): Task priority from 1 (normal) to 4 (urgent)
- `due_string` (string, optional): Natural language due date like 'tomorrow', 'next Monday'
- `due_date` (string, optional): Due date in YYYY-MM-DD format
- `due_datetime` (string, optional): Due date and time in RFC3339 format
- `due_lang` (string, optional): 2-letter language code for due date parsing
- `assignee_id` (string, optional): User ID to assign the task to
- `duration` (number, optional): The duration amount of the task
- `duration_unit` (string, optional): The duration unit ('minute' or 'day')
- `deadline_date` (string, optional): Deadline date in YYYY-MM-DD format

Batch tasks:

- `tasks` (array, required): Array of task objects, each containing the fields above (content is required for each). Allows creating multiple tasks in a single request, improving efficiency for LLM interactions.

### todoist_get_tasks

Get a list of tasks from Todoist with various filters. Handles both single and batch retrieval.

- `project_id` (string, optional): Filter tasks by project ID
- `section_id` (string, optional): Filter tasks by section ID
- `label` (string, optional): Filter tasks by label name
- `filter` (string, optional): Natural language filter like 'today', 'tomorrow', 'next week', 'priority 1', 'overdue'
- `lang` (string, optional): IETF language tag defining what language filter is written in
- `ids` (array of strings, optional): Array of specific task IDs to retrieve. Useful when an LLM has previously interacted with certain tasks and wants to retrieve those again.
- `priority` (number, optional): Filter by priority level (1-4). Very useful for an LLM to retrieve high-priority or urgent tasks. *Not part of the base Todoist API.*
- `limit` (number, optional): Maximum number of tasks to return (client-side filtering), default 10. Prevents an LLM from being overwhelmed if there are many matching tasks. The LLM can increment the limit if needed. *Not part of the base Todoist API.*

### todoist_update_task

Update one or more tasks in Todoist with full parameter support.

Single task:

- `task_id` (string, preferred): ID of the task to update
- `task_name` (string): Name/content of the task to search for (if ID not provided). Allows flexibly identifying tasks, as the exact ID may not always be available to an LLM, but the task name is more likely known.
- Rest of parameters same as todoist_create_task

Batch tasks:

- `tasks` (array, required): Array of task objects to update, each containing:
  - `task_id` (string, preferred): ID of the task to update  
  - `task_name` (string): Name/content of the task to search for (if ID not provided)
  - Rest of parameters same as todoist_create_task
- Allows updating multiple tasks in a single request, improving efficiency for LLM interactions.

### todoist_delete_task

Delete one or more tasks from Todoist.

Single task:

- `task_id` (string, preferred): ID of the task to delete
- `task_name` (string): Name/content of the task to search for and delete (if ID not provided). Allows flexibly identifying tasks.

Batch tasks:

- `tasks` (array, required): Array of task objects to delete, each containing:
  - `task_id` (string, preferred): ID of the task to delete
  - `task_name` (string): Name/content of the task to search for and delete (if ID not provided)  
- Allows deleting multiple tasks in a single request.

### todoist_complete_task

Mark one or more tasks as complete in Todoist.

Single task:

- `task_id` (string, preferred): ID of the task to complete
- `task_name` (string): Name/content of the task to search for and complete (if ID not provided). Allows flexibly identifying tasks.

Batch tasks:

- `tasks` (array, required): Array of task objects to mark as complete, each containing:
  - `task_id` (string, preferred): ID of the task to complete
  - `task_name` (string): Name/content of the task to search for and complete (if ID not provided)
- Allows completing multiple tasks in a single request.

**Batch Operation Responses**
For batch task creation, update, deletion and completion requests, the response will contain:

- `summary` field with the count of total, succeeded, and failed operations
- `results` array with the detailed outcome for each individual task

This allows an LLM to efficiently process multiple tasks while still being able to understand and respond to the result of each one.

**Robust Error Handling**
All task tools have extensive error handling:

- For single task operations, if the request fails, the response content will contain a clear error message that an LLM can process and respond to
- For batch operations, the `results` array will indicate which specific tasks failed and provide an error message for each failure

This error handling allows an LLM to gracefully handle failures, retry requests, or modify its approach based on the specific errors.

## Project Tools

### todoist_get_projects

Get projects with optional filtering and hierarchy information.

- `project_ids` (array of strings, optional): Specific project IDs to retrieve
- `include_sections` (boolean, optional, default false): Include sections within each project
- `include_hierarchy` (boolean, optional, default false): Include full parent-child relationships

### todoist_create_project

Create one or more projects with support for nested hierarchies.

Single project:

- `name` (string, required): Name of the project
- `parent_id` (string, optional): Parent project ID
- `color` (string, optional): Color of the project icon (see Colors guide for options)
- `is_favorite` (boolean, optional): Whether the project is a favorite
- `view_style` (string, optional): View style of the project ('list' or 'board')

Batch projects:

- `projects` (array, required): Array of project objects to create, each containing:
  - `name` (string, required): Name of the project
  - `parent_id` (string, optional): Parent project ID
  - `parent_name` (string, optional): Name of the parent project (will be created or found automatically)
  - `color` (string, optional): Color of the project icon
  - `is_favorite` (boolean, optional): Whether the project is a favorite
  - `view_style` (string, optional): View style of the project ('list' or 'board') 
  - `sections` (array of strings, optional): Sections to create within this project

### todoist_update_project

Update one or more projects in Todoist.

Single project:

- `project_id` (string, required): ID of the project to update
- `name` (string, optional): New name for the project
- `color` (string, optional): New color for the project icon
- `is_favorite` (boolean, optional): Whether the project should be a favorite
- `view_style` (string, optional): View style of the project ('list' or 'board')

Batch projects:

- `projects` (array, required): Array of project objects to update, each containing:
  - `project_id` (string, preferred): ID of the project to update
  - `project_name` (string): Name of the project to update (if ID not provided)
  - `name` (string, optional): New name for the project
  - `color` (string, optional): New color for the project icon 
  - `is_favorite` (boolean, optional): Whether the project should be a favorite
  - `view_style` (string, optional): View style of the project ('list' or 'board')

### todoist_delete_project

Delete one or more projects from Todoist.

Single project:

- `project_id` (string): ID of the project to delete
- `project_name` (string): Name of the project to delete (if ID not provided)
- User will be prompted to confirm deletion by project name for safety

Batch projects:  

- `projects` (array, required): Array of project objects to delete, each containing:
  - `project_id` (string, preferred): ID of the project to delete
  - `project_name` (string): Name of the project to delete (if ID not provided)
  - User will be prompted to confirm the number of projects to be deleted for safety

### todoist_get_project_sections

Get all sections in a Todoist project.

- `project_id` (string, required): ID of the project

## Section Tools

### todoist_create_section

Create a new section in a Todoist project.  

- `project_id` (string, required): ID of the project
- `name` (string, required): Name of the section
- `order` (number, optional): Order of the section


## Label Tools

### todoist_get_personal_labels

Get all personal labels from Todoist.

### todoist_create_personal_label

Create a new personal label in Todoist.

- `name` (string, required): Name of the label
- `color` (string, optional): Color of the label. Allowed values: berry_red, red, orange, yellow, olive_green, lime_green, green, mint_green, teal, sky_blue, light_blue, blue, grape, violet, lavender, magenta, salmon, charcoal, grey, taupe
- `order` (number, optional): Order of the label
- `is_favorite` (boolean, optional): Whether the label is a favorite

### todoist_get_personal_label

Get a personal label by ID.

- `label_id` (string, required): ID of the label to retrieve

### todoist_update_personal_label

Update an existing personal label in Todoist.

- `label_id` (string, required): ID of the label to update
- `name` (string, optional): New name for the label
- `color` (string, optional): New color for the label. Allowed values: berry_red, red, orange, yellow, olive_green, lime_green, green, mint_green, teal, sky_blue, light_blue, blue, grape, violet, lavender, magenta, salmon, charcoal, grey, taupe
- `order` (number, optional): New order for the label
- `is_favorite` (boolean, optional): Whether the label is a favorite

### todoist_delete_personal_label

Delete a personal label from Todoist.

- `label_id` (string, required): ID of the label to delete  

### todoist_update_task_labels

Update the labels of a task in Todoist.

- `task_name` (string, required): Name/content of the task to update labels for
- `labels` (array of strings, required): Array of label names to set for the task
