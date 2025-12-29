# How to Deploy to HuggingFace Spaces

**🚀 Deploy your Grocery Manager app to HuggingFace Spaces using Gradio**

---

## 📋 Prerequisites

1. **HuggingFace Account**: Sign up at https://huggingface.co/join
2. **HuggingFace Token**: Get your write token from https://huggingface.co/settings/tokens
3. **Gradio installed**: Should already be in your virtual environment

---

## 🎯 Deployment Steps

### Step 1: Navigate to Output Directory

```bash
cd /Users/gkeskar/projects/agents/3_crew/engineering_team_shopping_list/output
```

### Step 2: Run Gradio Deploy Command

```bash
uv run gradio deploy
```

Or if not using `uv`:

```bash
gradio deploy
```

### Step 3: Follow the Interactive Prompts

You'll be asked for:

```
Enter Spaces app title [output]: grocery-list-manager
Enter Gradio app file [app.py]: [Press Enter - default is correct]
Enter Spaces hardware [cpu-basic]: [Press Enter - default is fine]
Any Spaces secrets (y/n) [n]: n [unless you need API keys]
Create Github Action [n]: n [or y if you want auto-deploy]
```

### Step 4: Get Your Live URL

After deployment completes, you'll see:

```
Space available at https://huggingface.co/spaces/YOUR-USERNAME/grocery-list-manager
```

---

## 🔐 Adding API Keys (Optional)

If you want **email functionality** to work on HuggingFace:

1. Go to your Space: https://huggingface.co/spaces/gandhalikeskar/grocery-list-manager
2. Click **Settings** tab
3. Scroll to **Repository secrets**
4. Add new secret:
   - **Name**: `RESEND_API_KEY`
   - **Value**: Your Resend API key from `.env` file
5. Click **Save**
6. Restart the Space

---

## 📦 Required Files

Make sure these files exist in the `output/` directory:

- ✅ `app.py` - Main Gradio application
- ✅ `grocery_app.py` - Backend GroceryManager class
- ✅ `requirements.txt` - Python dependencies
- ✅ `grocery_catalog.json` - Store catalog data

### requirements.txt Contents:

```txt
gradio
pandas
python-dotenv
resend
```

---

## 🔄 Updating Your Deployed App

### Option 1: Redeploy (Simple)

Just run `gradio deploy` again from the output directory:

```bash
cd /Users/gkeskar/projects/agents/3_crew/engineering_team_shopping_list/output
uv run gradio deploy
```

### Option 2: Git Push (If you enabled Github Action)

```bash
cd /Users/gkeskar/projects/agents/3_crew/engineering_team_shopping_list/output
git add .
git commit -m "Update app"
git push
```

The Space will automatically rebuild!

---

## 🛠️ Troubleshooting

### Problem: "gradio: command not found"

**Solution**: Make sure you're using `uv run` or activate your virtual environment:

```bash
source .venv/bin/activate  # or 'uv shell'
gradio deploy
```

### Problem: "Authentication required"

**Solution**: Login to HuggingFace:

```bash
huggingface-cli login
```

Then paste your write token from https://huggingface.co/settings/tokens

### Problem: App crashes on HuggingFace

**Solution**: Check the logs in the Space's **Logs** tab. Common issues:
- Missing dependencies in `requirements.txt`
- File paths that work locally but not in the cloud
- Missing API keys (add them as Secrets)

### Problem: "No space left on device"

**Solution**: Your `grocery_catalog.json` might be too large. The free tier has limited storage.

---

## 📁 Files NOT Deployed (For Security)

These files are automatically excluded:

- ❌ `.env` - Contains API keys (add as Secrets instead)
- ❌ `.git/` - Version control (unless using Github Action)
- ❌ `__pycache__/` - Python cache files
- ❌ `.DS_Store` - Mac system files

---

## 🌐 Your Live Apps

**Current Deployment:**
- **URL**: https://huggingface.co/spaces/gandhalikeskar/grocery-list-manager
- **Username**: gandhalikeskar
- **Space Name**: grocery-list-manager

---

## 📊 Space Settings You Can Change

On HuggingFace Space settings page:

1. **Visibility**: 
   - Public (anyone can use)
   - Private (only you and collaborators)

2. **Hardware**:
   - `cpu-basic` (free, default)
   - `cpu-upgrade` (paid, faster)
   - GPU options (for ML models)

3. **Sleep Mode**:
   - Free tier: sleeps after 48h of inactivity
   - Paid tier: always on

4. **Custom Domain**:
   - Available on paid plans

---

## 🎨 Adding a Custom README

To add a nice description on your Space page, edit the `README.md` in your Space repo:

```markdown
---
title: Smart Grocery Manager
emoji: 🛒
colorFrom: blue
colorTo: green
sdk: gradio
sdk_version: 4.x.x
app_file: app.py
pinned: false
---

# Smart Grocery Manager

Manage your grocery shopping list with ease!

Features:
- 📦 Multiple store catalogs
- 🛒 Shopping list management
- 📧 Email your list
- 🏷️ Category filtering
- ✏️ Easy catalog editing
```

---

**Last Updated**: 2025-11-16
**Project**: Smart Grocery Manager
**HuggingFace Space**: https://huggingface.co/spaces/gandhalikeskar/grocery-list-manager

