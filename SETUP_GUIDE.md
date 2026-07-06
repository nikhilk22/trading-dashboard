# 🎯 Complete Setup Guide: GitHub Trading Dashboard

## Repository Structure

```
trading-dashboard/
├── .github/
│   └── workflows/
│       └── update-dashboard.yml      ← GitHub Actions automation config
├── .gitignore                        ← Tell Git what to ignore
├── generate_dashboard.py             ← Your Python script (reads from environment variables)
├── index.html                        ← Auto-generated dashboard (created by script)
├── README.md                         ← Project documentation
└── SETUP_GUIDE.md                    ← This file
```

---

## Step 1: Create GitHub Repository

### Option A: Create Fresh (Recommended)

1. Go to https://github.com/new
2. **Repository name:** `trading-dashboard`
3. **Description:** "Auto-updating trading dashboard"
4. **Public** (required for free GitHub Pages)
5. ❌ DO NOT initialize with README (we already have one)
6. Click **Create repository**

---

## Step 2: Push Your Code to GitHub

### If you created a new repo:

```bash
# Go to your trading-dashboard folder
cd /path/to/trading-dashboard

# Initialize git (if not already done)
git init
git config user.name "Nikhil Katrale"
git config user.email "your.email@gmail.com"

# Add all files (IMPORTANT: use updated files without hardcoded secrets!)
git add .

# Create first commit
git commit -m "🚀 Initial commit: Trading dashboard automation"

# Add GitHub as remote
git remote add origin https://github.com/YOUR-USERNAME/trading-dashboard.git

# Push to GitHub
git branch -M main
git push -u origin main
```

---

## Step 3: Add GitHub Secrets (IMPORTANT!)

**This is critical for security. Don't skip this!**

### Get Your Notion Token

1. Open https://www.notion.so/my-integrations
2. Click **+ New integration**
3. Name: `Trading Dashboard`
4. Click **Create integration**
5. Copy the **Internal Integration Token**

### Get Your Database ID

1. Open your Notion Trading database
2. Click **Share** button → Copy link
3. Extract ID from URL:
   ```
   https://notion.so/workspace/DATABASE_ID?v=...
                        ^^^^^^^^^^^^^^^^
   ```

### Add Secrets to GitHub

1. Go to your GitHub repo page → **Settings** (top right)
2. Left sidebar → **Secrets and variables** → **Actions**
3. Click **New repository secret**

**Add the following secrets:**

| Name | Value |
|------|-------|
| `NOTION_TOKEN` | (paste your Notion token) |
| `DATABASE_ID` | (paste your database ID) |

---

## Step 4: Enable GitHub Pages

1. Go to your GitHub repo → **Settings** (top right)
2. Left sidebar → **Pages**
3. Under "Source":
   - Select: **Deploy from a branch**
   - Branch: **main** → **/ (root)**
4. Click **Save**

⏳ Wait 30 seconds...

5. Refresh the page. You'll see:
   ```
   Your site is live at: https://YOUR-USERNAME.github.io/trading-dashboard
   ```

---

## Step 5: Trigger the First Run

### Option A: Let it Run Automatically

- Dashboard generates **daily at 12 PM UTC**
- Check back in 1 hour

### Option B: Manual Trigger (Instant)

1. Go to your repo → **Actions** tab (top of repo)
2. Left sidebar → **Update Trading Dashboard**
3. Click **Run workflow** → **Run workflow** (green button)
4. Wait 30 seconds...
5. Visit your dashboard URL

---

## Step 6: Access Your Dashboard

**Your live dashboard URL:**
```
https://YOUR-USERNAME.github.io/trading-dashboard
```

It will be **public** and **always online** ✅

---

## 🔄 How the Automation Works

```
Every day at 12 PM UTC:
    ↓
GitHub Actions runs generate_dashboard.py
    ↓
Python reads NOTION_TOKEN from GitHub Secrets (environment variable)
    ↓
Script connects to Notion API
    ↓
Fetches all your trades
    ↓
Generates index.html with embedded data
    ↓
Commits & pushes to GitHub
    ↓
GitHub Pages auto-deploys
    ↓
Your dashboard updates live!
```

**No local server needed.** No double-clicking. Just GitHub doing the work.

---

## ⚙️ Customize

### Change Update Time

Edit `.github/workflows/update-dashboard.yml`, line 9:

```yaml
schedule:
  - cron: '0 12 * * *'  # Change this
```

**Cron format:** `minute hour day-of-month month day-of-week`

**Examples:**
- `0 9 * * 1` = Every Monday at 9 AM UTC
- `0 6,18 * * *` = Twice daily: 6 AM & 6 PM UTC
- `*/30 * * * *` = Every 30 minutes

### Change Timezone

Edit `generate_dashboard.py`, search for `timeZone:`:

```javascript
{timeZone:'America/New_York'}     // Eastern Time (EST/EDT)
{timeZone:'Europe/London'}        // GMT/BST
{timeZone:'Asia/Singapore'}       // Singapore
{timeZone:'Australia/Sydney'}     // Sydney
```

Full list: https://en.wikipedia.org/wiki/List_of_tz_database_time_zones

---

## 🔐 Security: How Secrets Work

### What We Did

1. **Removed hardcoded token** from `generate_dashboard.py`
2. **Added secrets to GitHub** (encrypted on GitHub servers)
3. **Updated workflow** to pass secrets as environment variables
4. **Python reads from environment** using `os.getenv()`

### Why This Is Safe

✅ Token never appears in code  
✅ Token never pushed to GitHub  
✅ Token only exists on GitHub's secure servers  
✅ Workflow passes it securely to Python at runtime  
✅ Token never logged or exposed in build logs  

### Local Testing (Optional)

If you want to test locally:

```bash
# Set environment variables
export NOTION_TOKEN="your-token-here"
export DATABASE_ID="your-id-here"

# Run script
python generate_dashboard.py
```

---

## 🚨 Troubleshooting

### "GitHub Pages not showing"
✅ Check Settings → Pages → Source is set to **main / (root)**  
✅ Wait 1-2 minutes  
✅ Hard refresh: `Ctrl+Shift+R` (Windows) or `Cmd+Shift+R` (Mac)

### "Dashboard is empty / No trades"
✅ Check Actions tab for errors  
✅ Verify NOTION_TOKEN secret is set correctly  
✅ Verify DATABASE_ID secret is set correctly  
✅ Notion integration has read access to your database  
✅ Your Notion database actually has trades

### "Workflow keeps failing"
1. Go to **Actions** tab
2. Click the failed workflow run
3. Click the job to see error logs
4. Common issues:
   - ❌ Invalid Notion token → Get a new one from integrations
   - ❌ Wrong database ID → Double-check your database URL
   - ❌ Secrets not set → Go to Settings → Secrets and add them
   - ❌ Database not shared → Re-share with the integration

### "Push protection error about secrets"
This happened because you had hardcoded tokens. You fixed it by:
1. Using environment variables in code
2. Moving secrets to GitHub Secrets
3. Updating workflow to pass secrets securely

Now you're following best practices! ✅

---

## 📚 Interview Prep: Git Concepts

Since you're preparing for Git interviews, here are key concepts used here:

### 1. **Git Init & Config**
```bash
git init                 # Initialize a new repo
git config user.name     # Set your identity
```

### 2. **Staging & Committing**
```bash
git add .                # Stage all changes
git commit -m "message"  # Commit with message
```

### 3. **Remote & Push**
```bash
git remote add origin <URL>   # Add remote
git branch -M main            # Rename branch (master → main)
git push -u origin main       # Push & track upstream
```

### 4. **GitHub Features Used**
- **Actions** - CI/CD automation (runs your script on schedule)
- **Secrets** - Secure credential storage (your Notion token)
- **Pages** - Free static hosting (serves your dashboard)
- **Push Protection** - Prevents committing secrets (security!)

### 5. **Best Practices Demonstrated**
- ✅ Never hardcode secrets
- ✅ Use environment variables
- ✅ Use GitHub Secrets for sensitive data
- ✅ Keep code in version control, not credentials
- ✅ Automate repetitive tasks with Actions

---

## ✅ Checklist

- [ ] GitHub repo created (Public)
- [ ] Files pushed to GitHub (with updated code)
- [ ] GitHub Secrets added (NOTION_TOKEN + DATABASE_ID)
- [ ] Workflow file updated with env section
- [ ] GitHub Pages enabled (Settings → Pages)
- [ ] Main branch set as source in Pages
- [ ] First workflow run triggered manually
- [ ] Dashboard URL working
- [ ] Schedule customized (if needed)

---

## 📞 Next Steps

1. **Copy the updated files** to your local repo
2. **Run git commands** to push
3. **Add GitHub Secrets** (don't skip this!)
4. **Enable GitHub Pages**
5. **Trigger first run**
6. **View your live dashboard!**

---

## 🎉 You're Done!

Your dashboard is now:
- ✅ **Secure** (credentials in GitHub Secrets, not in code)
- ✅ **Automated** (runs daily, no action needed)
- ✅ **Cloud-hosted** (on GitHub, always online)
- ✅ **Live on a public URL** (shareable with anyone)

Perfect for interview prep too - you've learned Git, GitHub Actions, CI/CD, and security best practices!

Good luck! 