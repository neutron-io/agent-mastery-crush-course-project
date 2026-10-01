# Agentic Development Workshop

## Contents

- [Bring an Idea](#bring-an-idea)
- [Development Workflow](#development-workflow)
- [Course Session Planner | Initial webapp prompt example](#course-session-planner--initial-webapp-prompt-example)
- [Agent File Organizer | Initial desktop app prompt example](#agent-file-organizer--initial-desktop-app-prompt-example)
- [Workspace Plan Assistant | Initial VS Code extension prompt example](#workspace-plan-assistant--initial-vs-code-extension-prompt-example)
- [Batch File Renamer | Initial CLI prompt example](#batch-file-renamer--initial-cli-prompt-example)

Join us to build a greenfield project from scratch and learn how to use and refine the instructions, skills, and prompts prepared for the workshop.

## Bring an Idea

Choose something useful or interesting with a manageable first step. It could be a web or desktop app, a browser or VS Code extension, a CLI tool, an automation script, or another project of your choice.

Some possibilities:

- A personal expense tracker
- A desktop app for organizing photos or files
- A browser extension for saving and organizing bookmarks
- A VS Code extension for inserting team code snippets
- A CLI tool for renaming files in bulk
- A course CMS similar to the Kursadmin/Kurssystem we are building today
- A TODO list app if you need a default idea

Feel free to share your idea and discuss its scope. No detailed preparation is needed.

## Development Workflow

- Begin with an initial prompt that introduces the repository's idea, goal, and purpose in general terms. It can be organized into:
	- Introduction
	- Tech stack
	- Business description of the core logic and features

[See example](#course-session-planner--initial-webapp-prompt-example)

- Generate plan files
- Generate implementation files
- Start implementation:
	- Phase 1: approve the changes, then commit and push
	- Phase 2: refine the changes, approve them, then commit and push
	- Repeat the cycle for subsequent phases

## Course Session Planner | Initial webapp prompt example

```text
Create a responsive course-session planning web app for small teams that run internal workshops.

Purpose:
- Help organizers manage a catalog of workshops and schedule upcoming sessions.

Tech stack:
- React with Vite
- 100% TypeScript
- ESLint
- Prettier
- Vitest for unit tests of business logic
- Cypress for end-to-end tests of key user workflows
- Storybook to document reusable UI components and their states

Core features:
- Create, edit, and remove workshops with a title, description, and topic
- Schedule sessions with a date, time, location, and seat capacity
- Browse upcoming sessions and filter them by topic

Scope and first steps:
- Store data locally; do not add accounts or a backend
- Make the interface responsive and accessible

-------
Start with generating plans files
```

## Agent File Organizer | Initial desktop app prompt example

```text
Create a desktop MVP that uses an AI agent to help people organize files in a folder they select. Design it to support future iOS, Android, and web app clients.

Purpose:
- Suggest a useful folder structure and file organization while keeping the user in control.

Tech stack:
- Electron with React and Vite
- 100% TypeScript
- ESLint and Prettier
- Vitest for unit tests
- Storybook for reusable UI components

Core features:
- Let the user choose a folder with the native folder picker
- Use file names, types, and dates to suggest groups without reading file contents
- Show the agent's proposed moves and any name conflicts before applying changes
- Apply changes only after explicit approval, and support undoing the last operation

Scope and safety:
- Keep core logic platform-independent and isolate platform-specific UI and file-system access so future clients can reuse it; build only the desktop app for now
- Limit file access to the folder selected by the user; do not upload files
- Never modify files before the user approves the preview

-------
Start with generating plans files
```

## Workspace Plan Assistant | Initial VS Code extension prompt example

```text
Create a VS Code extension that lets the integrated Copilot agent use a focused workspace-planning tool.

Purpose:
- Help developers turn a task into a reviewable implementation plan from inside VS Code.

Tech stack:
- TypeScript and the VS Code Extension API
- The supported VS Code Language Model Tools API for tools available to the integrated agent
- ESLint, Prettier, and Vitest for unit tests

Core features:
- Register a tool the agent can use to draft a plan from the user's task and relevant workspace context
- Show the proposed plan before writing it to the workspace
- Save an approved plan under agent/plans/ and report the created file to the user

Scope and safety:
- Start with plan generation only; do not let the extension edit application source files
- Read only relevant files in the active workspace and require approval before writing
- Do not add a separate chat UI or connect to an external model provider

-------
Start with generating plans files
```

## Batch File Renamer | Initial CLI prompt example

```text
Create a Python CLI that safely renames files in a user-selected directory using a prefix and sequential numbering.

Purpose:
- Make repetitive file renaming quick, predictable, and reversible to review before changes are made.

Tech stack:
- Python 3.12+
- argparse for command-line argument parsing
- pytest for tests
- Ruff for linting and formatting

Core features:
- Accept a directory, filename prefix, and optional starting number
- Print a preview mapping each current filename to its proposed name
- Apply the rename only when the user passes an explicit apply option
- Detect name collisions and never overwrite existing files

Scope and safety:
- Operate only on files directly in the specified directory
- Keep preview as the default and return a nonzero exit code if a rename cannot be completed safely

-------
Start with generating plans files
```