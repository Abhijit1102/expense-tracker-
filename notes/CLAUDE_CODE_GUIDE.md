## Good Practice for using Claude Code

- One session per feature.
- `/compact` (Proactively, Not Reactively).
- Write focused, specific Prompts.
- Use `/subagents` for isolated or Exploratory work.
- Use `.claudeignore` to keep Irrelevant Files Out.

## CLAUDE.md

LLMs don't have memory. Claude Code can't remember things from prev sessions. Need to repeat instructions across sessions. Repeating instructions is cumbersome and error prone. Leads to inconsistent code generation.

The `CLAUDE.md` file is a special project-level instruction file used by Claude Code to guide how it behaves while working on your codebase.

Think of it as a persistent system prompt for your project.

Instead of repeatedly telling Claude:
- How your project is structured.
- Coding conventions.
- How to run/build/test.
- What tools to use.

We put all of that into CLAUDE.md and Claude automatically uses it as context every time.

- How to make `CLAUDE.md` using `claude init` or manually.
- **What to write in a CLAUDE.md**
  - A short description of the project so Claude immediately understands what it is building or modifying.
  - Example:
    - This is a Flask web app for tracking personal expenses using SQLite for persistence.

## Architecture

Explains how the codebase is structured and where things belong.

Example for this project:
```
expense-tracker/
├── app.py              # Flask app with all routes
├── database/
│   └── db.py           # SQLite helpers
├── templates/          # Jinja2 HTML templates
├── static/
│   ├── css/
│   └── js/             # Vanilla JS only
└── requirements.txt
```

## Code Style

This section tells Claude how code should look and what conventions to follow.

Example for this project:
- Python: PEP 8, snake_case for variables and functions.
- Templates: Jinja2 with `url_for()` for internal links.
- DB queries: parameterized with `?` placeholders — never f-strings.
- Error handling: use `abort()` for HTTP errors.

## Preferred Libraries

Constrains what tools/frameworks should be used.

Example for this project:
- Use Flask for web framework.
- Use SQLite for database.
- Use Vanilla JS for frontend.
- Do not introduce new dependencies unless necessary.

## Commands

List exact commands for running, testing, and maintaining the project.

For this project:
```bash
# Setup
python -m venv venv
venv\Scripts\activate       # Windows
pip install -r requirements.txt

# Run dev server (port 5001)
python app.py

# Run tests
pytest
```

## Critical Rules

Highlights critical warnings, edge cases, and things to avoid.

For this project:
- Never use raw string returns for stub routes — always render a template.
- Never hardcode URLs in templates — always use `url_for()`.
- Never put DB logic in route functions — belongs in `database/db.py`.
- FK enforcement is manual — `PRAGMA foreign_keys = ON` on every connection.
- App runs on port 5001, not default 5000.

## .claude

The `.claude` folder is Claude Code's local configuration directory that controls how Claude behaves — either for a specific project or across all projects on your machine.

It stores config for skills, custom slash commands, sub-agents, etc.

### Types of .claude Folder

- `project-level`: scoped to one project, committed to repo, shared with team.
- `Global or User-level`: scoped to your machine, applies to every project, personal to you.

### What's in Project-level .claude

- `/commands`: Custom slash commands.
- `settings.local.json`: Local settings.
- `/rules`: Rule files.
- `/skills`: Custom skills.
- `/agents`: Sub-agent definitions.

## Types of CLAUDE.md

- `./CLAUDE.md` — Root level, committed to repo.
- `./.claude/CLAUDE.md` — Inside .claude folder.
- `CLAUDE.local.md` — Personal tweaks, gitignored.
- `~/.claude/CLAUDE.md` — Global, applies to all projects.
- `./some/folder/CLAUDE.md` — Subdirectory scoped.

## Good Practices

- Start with `/init` then prune aggressively.
- Only put universally applicable things in it.
- Use emphasis sparingly for critical rules — if everything is important, nothing is.
- CLAUDE.md needs to be under 200-300 lines. Quality decreases uniformly with increase in instruction count.

## Rules Folder

We can split the CLAUDE.md file into topic-specific files:

```
.claude/rules/
├── code-style.md
├── testing.md
├── security.md
└── api-conventions.md
```

## Use @ Import to Reference External Docs

- `@docs/api-guidelines.md`
- `@docs/contributing.md`

## Auto-Memory

Auto memory is a persistent directory where Claude records learnings, patterns, and insights as it works.

When Claude discovers something about your project, it saves that to auto-memory. Next session, it already knows. No more repeating yourself.

Location: `~/.claude/projects/<project-path>/memory/`
