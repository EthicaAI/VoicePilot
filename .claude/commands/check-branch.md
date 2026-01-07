Scan a branch or PR for dangerous changes before merging.

Usage: `/check-branch <branch-name>` or `/check-branch <pr-url>`

This command will:

1. **Check for deleted critical files:**
   - `src/App.jsx` (main app component)
   - `index.html` (entry point)
   - `package.json` (dependencies)
   - `vite.config.js` (build config)
   - `tailwind.config.js` (styling config)

2. **Compare with main branch:**
   - List files that exist in main but are deleted in target branch
   - List files that are modified
   - Show summary of additions/deletions

3. **Provide recommendations:**
   - ✅ Safe to merge: No critical files deleted, normal feature work
   - ⚠️ Warning: Some important files modified significantly
   - 🚨 Danger: Critical files deleted, do NOT merge without review

4. **For GitHub PRs:**
   - Fetch PR details via `gh pr view`
   - Check mergeable status
   - List all changed files with additions/deletions

**Examples:**

```bash
# Check a local branch
/check-branch feature/new-hero

# Check a GitHub PR
/check-branch https://github.com/EthicaAI/VoicePilot/pull/1
```

**Output Format:**

```
## /check-branch Scan Results

**Branch:** feature/new-hero → main

### File Changes Summary
| File | Additions | Deletions | Notes |
|------|-----------|-----------|-------|
| src/App.jsx | +50 | -10 | Modified |
| src/components/Hero.jsx | +200 | — | New file |

### Critical Files Check
| Check | Status |
|-------|--------|
| Critical files deleted | ✅ None |
| Mergeable | ✅ Yes |

### ✅ SAFE TO MERGE
```

**Recovery (if critical files deleted):**
```bash
git checkout branch-name
git restore src/App.jsx index.html
git add -A
git commit -m "Restore deleted files"
```
