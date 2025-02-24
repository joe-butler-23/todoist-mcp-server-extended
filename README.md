# Todoist MCP Server Extended

[![smithery badge](https://smithery.ai/badge/@Chrusic/todoist-mcp-server-extended)](https://smithery.ai/server/@Chrusic/todoist-mcp-server-extended)

An MCP (Model Context Protocol) server implementation that integrates Claude with Todoist, enabling natural language task management. This server allows Claude to interact with your Todoist tasks, projects, sections, and labels using everyday language.

## Features

* **Project Management**: Create, update, and manage Todoist projects
* **Section Organization**: Create and manage sections within projects
* **Task Management**: Create, update, complete, and delete tasks using everyday language
* **Label Management**: Create, update, and manage personal labels and task labels
* **Smart Search**: Find tasks and labels using partial name matches
* **Flexible Filtering**: Filter tasks by project, section, due date, priority, and labels
* **Rich Task Details**: Support for descriptions, due dates, priority levels, and project/section assignment

For a complete list of available tools and enhancements as well as their usage, see [tools.md](tools.md) in doc.

## Quick Installation Guide

Comprehensive installation guide can be found in the doc\Howto - Setting up Claude Todoist MCP on Windows.md.

### Installing via Smithery

To install Todoist Extended Server for Claude Desktop automatically via [Smithery](https://smithery.ai/server/@Chrusic/todoist-mcp-server-extended):

```bash
npx -y @smithery/cli install @Chrusic/todoist-mcp-server-extended --client claude
```

Also compatible with cline or windsurf, by changing last parameter to  `--client cline` or `--client windsurf`

## Setup

### Getting a Todoist API Token

1. Log in to your Todoist account
2. Navigate to Settings → Integrations
3. Find your API token under "Developer"

For more information about the Todoist API, visit the [official Todoist API documentation](https://developer.todoist.com/guides/#developing-with-todoist).

## Example Usage

### Basic Operations

``` text
"Create task 'Review PR' in project 'Work' section 'To Do'"
"Add label 'Important' to task 'Review PR'"
"Show all tasks with label 'Important' in project 'Work'"
"Move task 'Documentation' to section 'In Progress'"
"Mark the documentation task as complete"
```

## Development

### Building from source

```bash
# Clone the repository
git clone https://github.com/Chrusic/todoist-mcp-server-extended.git

# Navigate to directory
cd todoist-mcp-server-extended

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

If you encounter any issues or need support, please file an issue on the [GitHub repository](https://github.com/Chrusic/todoist-mcp-server-extended/issues).
