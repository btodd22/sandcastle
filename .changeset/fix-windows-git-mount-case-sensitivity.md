---
"@ai-hero/sandcastle": patch
---

fix: case-insensitive Windows path matching in Docker git mounts

On Windows, `patchGitMountsForWindows` used case-sensitive string comparison
to match the parent `.git` directory path from `resolveGitMounts` against the
`gitdir:` path read from the worktree's `.git` file. Because Windows paths are
case-insensitive, a mismatch (e.g. `C:/Dev/Repo/.git` vs `c:/dev/repo/.git`)
caused the parent `.git` mount to remain with a Windows-style sandbox path
(`C:/dev/repo/.git`). Docker rejects this as non-absolute for Linux containers.

Path comparisons in both `patchGitMountsForWindows` and `normalizeMounts` are
now case-insensitive when running on Windows.
