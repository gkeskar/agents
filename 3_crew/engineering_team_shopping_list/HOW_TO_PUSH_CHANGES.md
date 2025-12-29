# How to Push Changes to Smart Grocery Manager Repository

**⚠️ IMPORTANT:** Never push from this directory directly! This is part of the course repo with 800+ commits.

## 🎯 Goal
Push **only** the grocery app changes to:
- **Remote Repo:** https://github.com/gkeskar/smart-grocery-manager
- **Git Remote Name:** `grocery` (uses `git@github.com-personal`)

---

## 📋 Step-by-Step Workflow

### Step 1: Delete Bad Branch (if you accidentally pushed everything)

```bash
# If you pushed the whole course repo by mistake, delete the branch:
git push grocery --delete feature/branch-name
```

### Step 2: Clone smart-grocery-manager to Temp Location

```bash
cd /tmp
git clone git@github.com-personal:gkeskar/smart-grocery-manager.git grocery-work
cd grocery-work
```

### Step 3: Create New Branch

```bash
# Use descriptive branch names like:
# - feature/fix-catalog-selection
# - feature/add-email-functionality
# - bugfix/quantity-update
git checkout -b feature/your-descriptive-name
```

### Step 4: Copy ONLY Changed Files

```bash
# Copy specific files you modified (adjust paths as needed)
cp /Users/gkeskar/projects/agents/3_crew/engineering_team_shopping_list/output/app.py ./app.py
cp /Users/gkeskar/projects/agents/3_crew/engineering_team_shopping_list/output/grocery_app.py ./grocery_app.py

# If you added new files:
cp /Users/gkeskar/projects/agents/3_crew/engineering_team_shopping_list/output/new_file.py ./new_file.py
```

### Step 5: Review Changes

```bash
# Check what changed
git status
git diff app.py
```

### Step 6: Commit and Push

```bash
# Add files
git add app.py grocery_app.py

# Commit with descriptive message
git commit -m "Your commit message here

- Bullet point of change 1
- Bullet point of change 2
- Bullet point of change 3"

# Push to remote
git push origin feature/your-descriptive-name
```

### Step 7: Create Pull Request

Go to: https://github.com/gkeskar/smart-grocery-manager

You'll see a prompt to create a Pull Request from your new branch.

### Step 8: Clean Up Temp Directory

```bash
cd ~
rm -rf /tmp/grocery-work
```

---

## 📝 Example Commit Messages

### Good Examples:

```
Fix catalog selection double-fire bug and quantity update dropdown

- Fixed Gradio DataFrame select event firing twice per click
- Changed from toggle to add-only selection behavior  
- Fixed quantity update dropdown to properly refresh with current items
```

```
Add email functionality for shopping lists

- Integrated Resend API for sending emails
- Added email configuration in Settings tab
- Support for multiple email recipients (comma-separated)
```

### Bad Examples:

```
❌ "update"
❌ "fix bug"
❌ "changes"
```

---

## 🚫 What NOT to Do

1. ❌ **Don't** run `git push grocery branch-name` from `/Users/gkeskar/projects/agents/3_crew/engineering_team_shopping_list`
   - This pushes the ENTIRE course repo (844 commits!)

2. ❌ **Don't** commit from the course repo directory
   - Always use the temp `/tmp/grocery-work` workflow

3. ❌ **Don't** push to `main` directly
   - Always create a feature branch first

---

## ✅ Quick Reference

```bash
# Full workflow in one go:
cd /tmp && \
git clone git@github.com-personal:gkeskar/smart-grocery-manager.git grocery-work && \
cd grocery-work && \
git checkout -b feature/your-branch-name && \
cp /Users/gkeskar/projects/agents/3_crew/engineering_team_shopping_list/output/app.py ./app.py && \
git add app.py && \
git commit -m "Your commit message" && \
git push origin feature/your-branch-name && \
cd ~ && \
rm -rf /tmp/grocery-work
```

---

## 📁 Files in This Project

**Main Files:**
- `output/app.py` - Gradio UI (main file, 1100+ lines)
- `output/grocery_app.py` - Backend logic (GroceryManager class)
- `grocery_catalog.json` - Store catalogs (auto-saved)
- `requirements.txt` - Python dependencies

**Configuration:**
- `.env` - API keys (not committed to git)
- `HOW_TO_PUSH_CHANGES.md` - This file!

---

## 🔧 Troubleshooting

### Problem: "This branch is 844 commits ahead"
**Solution:** You pushed from the course repo. Follow Step 1 to delete and start over.

### Problem: "Permission denied (publickey)"
**Solution:** Your SSH key for `github.com-personal` is not set up. Check `~/.ssh/config`.

### Problem: "Everything up-to-date" but changes not pushed
**Solution:** You forgot to `git add` your files before committing.

---

**Last Updated:** 2025-11-13
**Project:** Smart Grocery Manager
**Remote:** https://github.com/gkeskar/smart-grocery-manager

