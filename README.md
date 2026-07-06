# 📊 Trading Dashboard

A **cloud-hosted**, auto-updating trading dashboard that runs on **GitHub Actions** + **GitHub Pages**.

## ✨ Features

- 📈 **Live charts** — Win rate, P/L curves, weekly breakdown
- 🔄 **Auto-updating** — Runs daily at 12 PM UTC via GitHub Actions
- 🌐 **Public URL** — Dashboard always accessible at `https://your-username.github.io/trading-dashboard`
- 💰 **100% free** — No servers, no costs
- 📱 **Responsive** — Works on desktop, tablet, mobile
- 🔗 **Connected to Notion** — Pulls live trade data from your Notion database

## 🚀 Quick Start

### 1. Clone This Repository

```bash
git clone https://github.com/YOUR-USERNAME/trading-dashboard.git
cd trading-dashboard
```

### 2. Configure Your Notion Token

Edit `generate_dashboard.py`:
- Line 10: Replace `NOTION_TOKEN` with your Notion API token
- Line 11: Replace `DATABASE_ID` with your Notion database ID

**Get your Notion token:**
1. Go to https://www.notion.so/my-integrations
2. Create a new integration
3. Copy the "Internal Integration Token"

**Get your database ID:**
1. Open your Notion database → Share → Copy the database ID from the URL
   - URL format: `https://notion.so/workspace/DATABASE_ID?v=...`

### 3. Push to GitHub

```bash
git add .
git commit -m "Initial commit: Trading dashboard"
git push -u origin main
```

### 4. Enable GitHub Pages

1. Go to your GitHub repo → **Settings** → **Pages**
2. Under "Source", select: **Deploy from a branch**
3. Branch: **main** → **/ (root)**
4. Click **Save**

### 5. Your Dashboard is Live! 🎉

Access at: `https://YOUR-USERNAME.github.io/trading-dashboard`

---

## ⚙️ Configuration

### Change Update Schedule

Edit `.github/workflows/update-dashboard.yml`, line 7:

```yaml
schedule:
  - cron: '0 12 * * *'  # Daily at 12 PM UTC
```

**Cron Examples:**
- `0 9 * * 1` → Every Monday at 9 AM UTC
- `*/30 * * * *` → Every 30 minutes
- `0 6 * * *` → Daily at 6 AM UTC

### Change Timezone Display

Edit `generate_dashboard.py`, search for `timeZone:`. Change `'Asia/Kolkata'` to your timezone:

```javascript
{timeZone:'America/New_York'}  // Eastern Time
{timeZone:'Europe/London'}     // London
{timeZone:'Asia/Tokyo'}        // Tokyo
```

---

## 📋 File Structure

```
trading-dashboard/
├── generate_dashboard.py          # Python script to fetch & generate dashboard
├── .github/
│   └── workflows/
│       └── update-dashboard.yml   # GitHub Actions automation
├── index.html                     # Generated dashboard (auto-updated)
└── README.md                      # This file
```

---

## 🔧 How It Works

1. **You push code** → GitHub detects the update
2. **GitHub Actions runs** → Executes `generate_dashboard.py` on schedule
3. **Script fetches data** → Pulls all trades from your Notion database
4. **Generates HTML** → Creates a static `index.html` file
5. **Commits & pushes** → Automatically updates the repo
6. **GitHub Pages serves** → Your URL is instantly updated

---

## 🎛️ Manual Updates

Want to trigger an update manually?

1. Go to your GitHub repo
2. Click **Actions** tab
3. Select **Update Trading Dashboard** workflow
4. Click **Run workflow** → **Run workflow**
5. Done! Dashboard updates in ~30 seconds

---

## ❓ Troubleshooting

### Dashboard shows "No data"
- ✅ Check Notion token is correct in `generate_dashboard.py`
- ✅ Check database ID is correct
- ✅ Make sure your Notion integration has read access to the database

### GitHub Actions fails
- Go to **Actions** tab → **Update Trading Dashboard**
- Click the failed run → View logs
- Check for errors in the output

### Changes not showing
- GitHub Pages takes ~1-2 minutes to deploy
- Try hard refresh: `Ctrl+Shift+R` (Windows) or `Cmd+Shift+R` (Mac)

---

## 🔐 Security Note

Never commit your Notion token to GitHub. Instead, use GitHub Secrets:

1. Go to **Settings** → **Secrets and variables** → **Actions**
2. Create new secret: `NOTION_TOKEN`
3. In `update-dashboard.yml`, use: `${{ secrets.NOTION_TOKEN }}`
4. In `generate_dashboard.py`, read: `NOTION_TOKEN = os.getenv('NOTION_TOKEN')`

---

## 📄 License

MIT License — Feel free to fork and customize!

---

**Questions?** Check the [GitHub Docs](https://docs.github.com/en/pages) on Pages & Actions.
