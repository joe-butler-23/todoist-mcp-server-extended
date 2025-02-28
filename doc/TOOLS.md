# Todoist MCP Server Tools

This document describes the available tools in the Todoist MCP Server. These tools allow interacting with Todoist projects, sections, tasks, labels, and shared labels.

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
- `favorite` (boolean, optional): Whether the project is a favorite
- `view_style` (string, optional): View style of the project ('list' or 'board')

Batch projects:

- `projects` (array, required): Array of project objects to create, each containing:
  - `name` (string, required): Name of the project
  - `parent_id` (string, optional): Parent project ID
  - `parent_name` (string, optional): Name of the parent project (will be created or found automatically)
  - `color` (string, optional): Color of the project icon 
  - `favorite` (boolean, optional): Whether the project is a favorite
  - `view_style` (string, optional): View style of the project ('list' or 'board')
  - `sections` (array of strings, optional): Sections to create within this project

### todoist_update_project  

Update one or more projects in Todoist.

Single project:

- `project_id` (string, required): ID of the project to update 
- `name` (string, optional): New name for the project
- `color` (string, optional): New color for the project icon
- `favorite` (boolean, optional): Whether the project should be a favorite
- `view_style` (string, optional): View style of the project ('list' or 'board')

Batch projects:

- `projects` (array, required): Array of project objects to update, each containing:  
  - `project_id` (string, preferred): ID of the project to update
  - `project_name` (string): Name of the project to update (if ID not provided)
  - `name` (string, optional): New name for the project 
  - `color` (string, optional): New color for the project icon
  - `favorite` (boolean, optional): Whether the project should be a favorite  
  - `view_style` (string, optional): View style of the project ('list' or 'board')

### todoist_delete_project

Delete one or more projects from Todoist.

Single project:

- `project_id` (string): ID of the project to delete
- `project_name` (string): Name of the project to delete (if ID not provided)

Batch projects:

- `projects` (array, required): Array of project objects to delete, each containing:
  - `project_id` (string, preferred): ID of the project to delete
  - `project_name` (string): Name of the project to delete (if ID not provided)

### todoist_get_project_sections

Get sections from one or more projects in Todoist.

Single project:

- `project_id` (string): ID of the project to get sections from  
- `project_name` (string): Name of the project to get sections from (if ID not provided)

Batch projects:

- `projects` (array, required): Array of project objects to get sections from, each containing:
  - `project_id` (string, preferred): ID of the project 
  - `project_name` (string): Name of the project (if ID not provided)

- `include_empty` (boolean, optional, default true): Whether to include sections with no tasks

### todoist_create_project_section

Create one or more sections in Todoist projects.

Single section:

- `project_id` (string, required): ID of the project to create section in
- `name` (string, required): Name of the section  
- `order` (number, optional): Order of the section

Batch sections:

- `sections` (array, required): Array of section objects to create, each containing:
  - `project_id` (string): ID of the project (one of `project_id` or `project_name` required)
  - `project_name` (string): Name of the project (one of `project_id` or `project_name` required) 
  - `name` (string, required): Name of the section
  - `order` (number, optional): Order of the section

## Task Tools

*Unchanged from previous version*

## Personal Label Tools

### todoist_get_personal_labels

Get all personal labels from Todoist.

### todoist_create_personal_label

Create one or more personal labels in Todoist.

Single label:

- `name` (string, required): Name of the label
- `color` (string, optional): Color of the label icon
- `order` (number, optional): Order of the label
- `is_favorite` (boolean, optional): Whether the label is a favorite

Batch labels:

- `labels` (array, required): Array of label objects to create, each containing:
  - `name` (string, required): Name of the label
  - `color` (string, optional): Color of the label icon
  - `order` (number, optional): Order of the label 
  - `is_favorite` (boolean, optional): Whether the label is a favorite

### todoist_get_personal_label

Get a personal label by ID.

- `label_id` (string, required): ID of the label to retrieve

### todoist_update_personal_label

Update one or more existing personal labels in Todoist.

Single label:

- `label_id` (string): ID of the label to update
- `label_name` (string): Name of the label to search for and update (if ID not provided)
- `name` (string, optional): New name for the label
- `color` (string, optional): New color for the label icon 
- `order` (number, optional): New order for the label
- `is_favorite` (boolean, optional): Whether the label is a favorite

Batch labels:

- `labels` (array, required): Array of label objects to update, each containing:
  - `label_id` (string, preferred): ID of the label to update 
  - `label_name` (string): Name of label to search for and update (if ID not provided)
  - `name` (string, optional): New name for the label
  - `color` (string, optional): New color for the label icon
  - `order` (number, optional): New order for the label
  - `is_favorite` (boolean, optional): Whether the label is a favorite

### todoist_delete_personal_label  

Delete a personal label from Todoist.

- `label_id` (string, required): ID of the label to delete

## Shared Label Tools

### todoist_get_shared_labels

Get all shared labels from Todoist.

- `omit_personal` (boolean, optional): Whether to exclude the names of the user's personal labels from the results (default: false)

### todoist_rename_shared_labels  

Rename one or more shared labels in Todoist.

Single label:

- `name` (string, required): The name of the existing label to rename 
- `new_name` (string, required): The new name for the label

Batch labels:

- `labels` (array, required): Array of label rename operations, each containing:
  - `name` (string, required): The name of the existing label to rename
  - `new_name` (string, required): The new name for the label 

### todoist_remove_shared_labels

Remove one or more shared labels from Todoist tasks.

Single label:

- `name` (string, required): The name of the label to remove

Batch labels: 

- `labels` (array, required): Array of shared label names to remove, each containing:
  - `name` (string, required): The name of the label to remove

## Task Label Tools  

### todoist_update_task_labels

Update the labels of one or more tasks in Todoist.

Single task:

- `task_id` (string, preferred): ID of the task to update labels for
- `task_name` (string): Name/content of the task to search for and update labels (if ID not provided) 
- `labels` (array of strings, required): Array of label names to set for the task

Batch tasks:

- `tasks` (array, required): Array of task label update operations, each containing:  
  - `task_id` (string, preferred): ID of the task to update labels for
  - `task_name` (string): Name/content of the task to search for and update labels (if ID not provided)
  - `labels` (array of strings, required): Array of label names to set for the task
