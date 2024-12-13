# Todoist MCP Server

An MCP (Model Context Protocol) server implementation that integrates Claude with Todoist, enabling natural language task management. This server allows Claude to interact with your Todoist tasks, projects, and sections using everyday language.

## Features

* **Project Management**: Create, update, and manage Todoist projects
* **Section Organization**: Create and manage sections within projects
* **Natural Language Task Management**: Create, update, complete, and delete tasks using everyday language
* **Smart Task Search**: Find tasks using partial name matches
* **Flexible Filtering**: Filter tasks by project, section, due date, priority, and other attributes
* **Rich Task Details**: Support for descriptions, due dates, priority levels, and project/section assignment
* **Intuitive Error Handling**: Clear feedback for better user experience

## Installation

```bash
npm install -g todoist-mcp-enhanced-server
```

## Tools

### Project Management

#### todoist_get_projects
Get a list of all projects:
* Lists all projects with their IDs and attributes
* Example: "Show all my projects"

#### todoist_create_project
Create new projects:
* Required: name
* Optional: parent_id (for nested projects), color, favorite status
* Example: "Create project 'Work Tasks' with color blue"

#### todoist_update_project
Update existing projects:
* Required: project_id
* Optional: name, color, favorite status
* Example: "Update project 'Work Tasks' to be a favorite"

### Section Management

#### todoist_get_project_sections
Get sections within a project:
* Required: project_id
* Lists all sections in the specified project
* Example: "Show sections in project 'Work Tasks'"

#### todoist_create_section
Create new sections:
* Required: project_id, name
* Optional: order
* Example: "Create section 'In Progress' in project 'Work Tasks'"

### Task Management

#### todoist_create_task
Create new tasks with various attributes:
* Required: content (task title)
* Optional: description, due date, priority level (1-4), project_id, section_id
* Example: "Create task 'Team Meeting' in project 'Work' section 'Planning'"

#### todoist_get_tasks
Retrieve and filter tasks:
* Filter by project, section, due date, priority
* Natural language date filtering
* Optional result limit
* Example: "Show high priority tasks in project 'Work' due this week"

#### todoist_update_task
Update existing tasks using natural language search:
* Find tasks by partial name match
* Update any task attribute (content, description, due date, priority, project, section)
* Example: "Move meeting task to project 'Work' section 'In Progress'"

#### todoist_move_task
Move tasks between projects and sections:
* Find tasks by name
* Optional: project_id, section_id
* Example: "Move task 'Documentation' to section 'Done'"

#### todoist_complete_task
Mark tasks as complete using natural language search:
* Find tasks by partial name match
* Confirm completion status
* Example: "Mark the documentation task as complete"

#### todoist_delete_task
Remove tasks using natural language search:
* Find and delete tasks by name
* Confirmation messages
* Example: "Delete the PR review task"

## Setup

### Getting a Todoist API Token
1. Log in to your Todoist account
2. Navigate to Settings → Integrations
3. Find your API token under "Developer"

### Usage with Cline

Add to your Cline MCP settings file:

```json
{
  "mcpServers": {
    "todoist": {
      "command": "npx",
      "args": ["todoist-mcp-enhanced-server"],
      "env": {
        "TODOIST_API_TOKEN": "your_api_token_here"
      }
    }
  }
}
```

## Example Usage

### Project Management
```
"Show all my projects"
"Create project 'Work Tasks'"
"Update project color to blue"
```

### Section Management
```
"Show sections in project 'Work'"
"Create section 'In Progress' in project 'Work'"
```

### Task Management
```
"Create task 'Review PR' in project 'Work' section 'To Do'"
"Move task 'Documentation' to section 'In Progress'"
"Show tasks in project 'Work' section 'Done'"
"Update meeting task priority to high and move to 'Planning' section"
```

## Development

### Building from source
```bash
# Clone the repository
git clone https://github.com/lotarcc/todoist-mcp-server.git

# Navigate to directory
cd todoist-mcp-server

# Install dependencies
npm install

# Build the project
npm run build
```

## Contributing
Contributions are welcome! Feel free to submit a Pull Request.

## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Issues and Support
If you encounter any issues or need support, please file an issue on the [GitHub repository](https://github.com/lotarcc/todoist-mcp-server/issues).
