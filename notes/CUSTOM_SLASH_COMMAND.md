Saved Prompts invokked with a simpe `/command_name` syntax

Used to execute any repeatable workflow in your project.

Stored as Markdown files inside the `.claude` folder.

Two types:
 - `Project-scopped` - stored in `.claude/commands/` , avaliable only within that project.

 - `User-scopped` - stored in `~./claude/commands`, available across all yours projects.

Example:
-  `/review`: run a code review checklist on the current file
- `/commit` : generates a git commit message based on staged changes.
- `test` :  runs the test suite and analyse any features
- `/security-scan` : scans the codebase for common vulnerablities like SQL injection or exposed credentials.



