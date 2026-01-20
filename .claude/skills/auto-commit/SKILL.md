---
name: auto-commit
description: Auto-generate commit message, commit changes, and push to remote. Use when user invokes /auto-commit or asks to automatically commit and push changes.
---

# Auto Commit

Automatically generate a commit message based on staged/unstaged changes, commit, and push to remote.

## Workflow

1. **Check git status and diff**
   - Run `git status` to see untracked and modified files
   - Run `git diff` and `git diff --staged` to understand changes
   - Run `git log --oneline -5` to see recent commit message style

2. **Generate commit message**
   - Analyze the changes
   - Create a concise commit message following the repository's style
   - Use conventional commits format if the repo uses it, otherwise match existing style
   - Focus on "what" and "why", not "how"

3. **Stage, commit, and push**
   - Stage all changes: `git add -A`
   - Commit with generated message (use HEREDOC for proper formatting):
     ```bash
     git commit -m "$(cat <<'EOF'
     <commit message>

     Co-Authored-By: Claude <noreply@anthropic.com>
     EOF
     )"
     ```
   - Push to remote: `git push`

4. **Report result**
   - Show the commit hash and message
   - Confirm push success

## Safety Rules

- NEVER commit files that look like secrets (.env, credentials.json, *_secret*, etc.)
- If secrets are detected, warn the user and exclude them
- NEVER use `--force` push
- NEVER skip pre-commit hooks unless user explicitly requests
