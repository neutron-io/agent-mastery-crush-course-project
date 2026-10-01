# Agentic Development Workshop

## Contents

- [Bring an Idea](#bring-an-idea)
- [Development Workflow](#development-workflow)
- [Initial prompt example](#initial-prompt-example)

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

[See example](#initial-prompt-example)

- Generate plan files
- Generate implementation files
- Start implementation:
	- Phase 1: approve the changes, then commit and push
	- Phase 2: refine the changes, approve them, then commit and push
	- Repeat the cycle for subsequent phases

## Initial prompt example

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