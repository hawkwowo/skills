---
name: git-commit-message
description: Review relevant git changes, carefully stage only the files that belong in the current commit, and read staged diffs plus recent commit subjects to draft a concise single-line commit message that matches the repository's style and language. Use when Codex needs to summarize staged changes, recommend a commit subject, or prepare a commit message from `git diff --cached`.
---

# Git Commit Message

## Overview

Generate one recommended commit subject from the staged diff only.
Before drafting the message, review the working tree and stage only the files that clearly belong in the current commit.
Match the repository's recent prefix, tone, and language unless the user asks for a specific format.

## Workflow

1. Review the working tree and stage relevant files.
   Run `git status --short`.
   Check whether all changes for the current task are already staged.
   If relevant files are still unstaged or untracked, inspect them before staging.
   Run `git add <specific-files>` for the files that clearly belong in the current commit.
   Do not use broad staging that may capture unrelated work unless the user explicitly requests it.
   If the working tree includes unrelated edits, generated artifacts, or temporary files, leave them unstaged and mention the risk if it affects the commit summary.

2. Confirm staged status.
   Run `git status --short` again if staging changed.
   If nothing is staged after review, stop and explain that there is no staged content to summarize.
   Ask whether to stage files first or switch to an unstaged diff.

3. Read the staged changes.
   Run `git diff --cached`.
   Add `git diff --cached --stat` first when the patch is large and use the full staged diff only as needed.
   Base the message only on staged content, never on unstaged changes.

4. Learn the local style.
   Run `git log -5 --pretty=format:%s`.
   Reuse the dominant subject style from recent history, including prefix conventions such as `feat:`, `fix:`, `refactor:`, or custom prefixes.
   Keep the language aligned with nearby commits unless the user explicitly requests another language.

5. Draft the message.
   Choose the most important staged change as the verb and subject.
   Keep the result to a single line.
   Prefer a short `type:summary` style when the repository uses typed prefixes.
   If the user provides a required format such as `behavior:short description`, follow that format first and adapt wording to the staged diff.
   If staged changes mix unrelated work, recommend splitting the commit before suggesting a combined message.

6. Reply with the result.
   Output only the recommended commit message on one line.
   If useful, add one short reminder that unstaged changes are excluded.

## Guardrails

Never infer commit content from the working tree when it is not staged.
Never stage unrelated files, temporary files, or generated artifacts just to make the commit message easier to write.
Never output multiple alternative messages unless the user explicitly asks for options.
Never add a body, footer, or explanation unless the user asks for it.
