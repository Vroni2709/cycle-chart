# Cycle Temperature Chart

Live basal body temperature comparison chart — always shows your 3 most recent cycles.
Hosted free on GitHub Pages, data refreshed hourly from Notion via GitHub Actions.

---

## Setup (one time, ~10 minutes)

### Step 1 — Create a Notion integration

1. Go to **notion.so/my-integrations**
2. Click **+ New integration**
3. Give it any name (e.g. "Cycle Chart")
4. Click **Save** → copy the **Internal Integration Secret** (starts with `ntn_` or `secret_`)

### Step 2 — Share your database with the integration

1. Open your **Cycle Tracking** database in Notion
2. Click the `···` menu (top right) → **Connections**
3. Find your integration and click **Connect**

### Step 3 — Create a GitHub repository

1. Go to **github.com** → sign in (or create a free account)
2. Click **+** → **New repository**
3. Name it anything (e.g. `cycle-chart`)
4. Set it to **Public** (required for free GitHub Pages)
5. Click **Create repository**

### Step 4 — Upload these files

Upload the two folders exactly as they are:
```
.github/
  workflows/
    fetch-notion.yml
docs/
  index.html
README.md
```

The easiest way: on your new repo page, drag and drop the files,
or use **Add file → Upload files**.

Keep the folder structure intact — `.github/workflows/` must stay nested.

### Step 5 — Add your Notion token as a secret

1. In your GitHub repo → **Settings** → **Secrets and variables** → **Actions**
2. Click **New repository secret**
3. Name: `NOTION_TOKEN`
4. Value: paste your integration secret from Step 1
5. Click **Add secret**

### Step 6 — Enable GitHub Pages

1. In your repo → **Settings** → **Pages**
2. Source: **Deploy from a branch**
3. Branch: `main`, Folder: `/docs`
4. Click **Save**

### Step 7 — Run the Action for the first time

1. Go to **Actions** tab in your repo
2. Click **Fetch Notion Cycle Data**
3. Click **Run workflow** → **Run workflow**
4. Wait ~30 seconds for it to finish

### Step 8 — View your chart

Your chart is now live at:
```
https://YOUR-GITHUB-USERNAME.github.io/YOUR-REPO-NAME/
```

Copy that URL and paste it into a Notion page using `/embed`.

---

## How it works

- GitHub Actions runs every hour, fetches your Notion database, and saves the top 3 cycles as `docs/cycle-data.json`
- The chart page (`index.html`) reads that JSON file — no API calls from the browser, no CORS issues
- When you start a new cycle and it becomes one of the top 3, the chart updates automatically within an hour

## Adjusting the refresh schedule

Edit `.github/workflows/fetch-notion.yml` and change the cron line:
- Every hour: `0 * * * *`
- Every 6 hours: `0 */6 * * *`
- Once a day at 6am: `0 6 * * *`
