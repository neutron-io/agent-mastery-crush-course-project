# Agentic Development Workshop

## Contents

- [Bring an Idea](#bring-an-idea)
- [Development Workflow](#development-workflow)
- [Use the Workshop Template](#use-the-workshop-template)
- [Initial prompts examples](#initial-prompts-examples)
	- [Course Session Planner | Initial webapp prompt example](#course-session-planner--initial-webapp-prompt-example)
	- [Figma Product Brief | Initial prompt for a designer and product manager](#figma-product-brief--initial-prompt-for-a-designer-and-product-manager)
	- [Agent File Organizer | Initial desktop app prompt example](#agent-file-organizer--initial-desktop-app-prompt-example)
	- [Workspace Plan Assistant | Initial VS Code extension prompt example](#workspace-plan-assistant--initial-vs-code-extension-prompt-example)
	- [Batch File Renamer | Initial CLI prompt example](#batch-file-renamer--initial-cli-prompt-example)
	- [Course Session Planner | Guided initial prompts example](#course-session-planner--guided-initial-prompts-example)

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

	[🧭 See initial prompts examples](#initial-prompts-examples)

- Generate plan files
- Generate implementation files
- Start implementation:
	- Phase 1: approve the changes, then commit and push
	- Phase 2: refine the changes, approve them, then commit and push
	- Repeat the cycle for subsequent phases


## Use the Workshop Template

To create your own private copy of the workshop repository, follow the [template usage guide](docs/using-the-template.md).

## Initial prompts examples

### Course Session Planner | Initial webapp prompt example

```text
Create a responsive course-session planning web app for small teams that run internal workshops.

Purpose:
- Help organizers manage a catalog of workshops and schedule upcoming sessions.

Tech stack:
- React with Vite
- 100% TypeScript
- ESLint
- Prettier
- Supabase for PostgreSQL and authentication
- Vitest for unit tests of business logic
- Cypress for end-to-end tests of key user workflows
- Storybook to document reusable UI components and their states

Core features:
- Create, edit, and remove workshops with a title, description, and topic
- Schedule sessions with a date, time, location, and seat capacity
- Browse upcoming sessions and filter them by topic

Scope and first steps:
- Authentication: Required; organizers sign in to access shared workshop data
- Store workshop and session data in Supabase PostgreSQL; do not use browser-local storage as the source of truth
- Keep the MVP to one shared workspace; defer invitations and role management
- Make the interface responsive and accessible

-------
Start with generating plans files
```

### Figma Product Brief | Initial prompt for a designer and product manager

```text
I'm a product manager and product designer. I work mostly in Figma and am new to turning designs into working software. Help me shape a small, useful web app from my product idea and design materials.

My product idea:
- [Describe the problem, intended users, and desired outcome]

My Figma materials:
- [Paste a link, attach screenshots, or describe the prototype]

Work with me in plain language:
- Ask one focused question at a time about users, goals, workflows, constraints, and what belongs in the MVP
- Treat Figma as a design reference, not a complete specification; if you cannot access a link, ask me for screenshots or exports, and do not invent unseen screens or behavior
- Help identify missing user flows, product rules, responsive layouts, and loading, empty, and error states
- Once requirements are clear, compare suitable technology and hosting or database options, including currently verified free tiers where available; explain tradeoffs without choosing for me
- Ask me to decide about authentication, data storage, and other major product choices before finalizing recommendations

After we agree on the product direction, draft a concise product brief and initial project prompt for my approval. Do not write code or begin implementation. After I approve the prompt, start with generating plans files, then wait for my approval before implementation.
```

### Agent File Organizer | Initial desktop app prompt example

```text
Create a desktop MVP that helps people organize files in a folder they select. Design it to support future iOS, Android, and web app clients.

Purpose:
- Suggest a useful folder structure and file organization while keeping the user in control.

Tech stack:
- Tauri 2 with Svelte and Vite
- TypeScript for the frontend and shared core logic; Rust only for native Tauri commands
- ESLint and Prettier
- Vitest for unit tests of shared logic
- Storybook for reusable Svelte components

Core features:
- Let the user choose a folder with the native folder picker
- Use file names, types, and dates to suggest groups without reading file contents
- Show the app's proposed moves and any name conflicts before applying changes
- Apply changes only after explicit approval, and support undoing the last operation

Scope and safety:
- Authentication: Not required; the app works locally without user accounts or sign-in
- Keep core logic platform-independent and isolate platform-specific UI and file-system access so future clients can reuse it; build only the desktop app for now
- No AI for the MVP; use deterministic local rules to suggest groups, with no AI service or backend
- Limit file access to the folder selected by the user; do not upload files
- Never modify files before the user approves the preview

-------
Start with generating plans files
```

### Workspace Plan Assistant | Initial VS Code extension prompt example

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
- Authentication: Do not implement extension-specific authentication; rely on VS Code and Copilot's existing sign-in and access requirements
- Start with plan generation only; do not let the extension edit application source files
- Read only relevant files in the active workspace and require approval before writing
- Do not add a separate chat UI or connect to an external model provider

-------
Start with generating plans files
```

### Batch File Renamer | Initial CLI prompt example

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
- Authentication: Not required; the CLI operates locally without user accounts or sign-in
- Operate only on files directly in the specified directory
- Keep preview as the default and return a nonzero exit code if a rename cannot be completed safely

-------
Start with generating plans files
```

### Course Session Planner | Guided initial prompts example

Send these prompts one at a time, responding to the agent between prompts. This keeps decisions manageable and lets the developer learn the tradeoffs before settling on a stack.

**1. Introduce the idea and ask for discovery questions**

```text
I want to build a web app for small teams to organize workshops and schedule course sessions. I'm new to choosing web technologies, so guide me through the early decisions in plain language.

Don't choose a tech stack or write code yet. First, ask me one focused question at a time about the intended users, their main workflows, and what the first version needs to do. Help me keep the MVP small.
```

**2. Clarify requirements and compare suitable options**

```text
Based on what I've told you, summarize the users, core workflows, and MVP features. Call out any assumptions or unanswered questions, especially about authentication, shared access, and where data should be stored.

Then recommend two or three suitable technology approaches. Explain the frontend, backend, and data-storage choices in beginner-friendly terms, including the tradeoffs and learning curve. Don't decide for me or write code.
```

**3. Explore free services and make decisions**

```text
For the options you recommended, identify relevant hosting, database, and authentication services with free tiers, if those services are needed for this MVP.

Check current official pricing and documentation. Explain free-tier limits, what could require payment, and any important lock-in or privacy tradeoffs. Don't describe a service as free without verifying its current terms.

Help me choose by asking one decision question at a time. Include whether this MVP actually needs accounts, a backend, or a hosted database. Summarize my choices and wait for my approval before locking them in.
```

**4. Create the agreed project prompt**

```text
Using the requirements and technical decisions I've approved, draft an initial project prompt with:
- An introduction describing the app's goal and users
- The selected tech stack and services
- The core business logic and MVP features
- Authentication, data-storage, privacy, and deployment decisions
- Testing, accessibility, and out-of-scope items

Use plain language, list remaining assumptions, and don't implement the app. First show me the draft for approval. After I approve it, start with generating plans files; do not start implementation until I approve a plan.
```