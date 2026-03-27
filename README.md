# ⚡ Clear Mind — Daily Discipline Tracker

> A personal discipline OS. Track your 5 daily habits, visualize streaks, and store everything in your own Google Sheet. Deployable to GitHub Pages in minutes — completely free.

---

## What It Does

| Feature | Details |
|---|---|
| **Daily Scorecard** | Check off 5 habits each day, add a reflection note, save to Sheets |
| **Dashboard** | Streak counter, 13-week heatmap, per-habit completion rates |
| **History** | Full log of every day you've tracked |
| **Plan** | Your Clear Mind blueprint — routine, strategies, goals |
| **Storage** | 100% your own Google Sheet. No third-party servers. |

**The 5 Habits:**
1. **Deep Work** — Completed 90-min focus session before 9 AM?
2. **Output** — Pushed at least one commit to GitHub?
3. **Mastery** — Explained a "Why" instead of just "How"?
4. **Health** — Under 2 hours non-work screen time?
5. **Discipline** — Finished the specific planned task?

---

## Setup (5 minutes)

### Step 1 — Create a Google Sheet

1. Go to [sheets.google.com](https://sheets.google.com)
2. Create a new blank spreadsheet
3. Name it `Clear Mind` (or anything you like)

### Step 2 — Add the Apps Script

1. In your spreadsheet, go to **Extensions → Apps Script**
2. Delete all existing code in `Code.gs`
3. Paste the entire contents of [`Code.gs`](./Code.gs) from this repo
4. Click **Save** (💾 icon)

### Step 3 — Deploy as Web App

1. Click **Deploy → New deployment**
2. Click the gear icon next to "Select type" → choose **Web App**
3. Set these options:
   - **Description:** `Clear Mind API` (anything)
   - **Execute as:** `Me`
   - **Who has access:** `Anyone`
4. Click **Deploy**
5. Click **Authorize access** → choose your Google account → Allow
6. **Copy the Web App URL** (looks like: `https://script.google.com/macros/s/LONG_ID_HERE/exec`)

### Step 4 — Deploy the App to GitHub Pages

1. Fork this repository (or create a new repo and upload the files)
2. Go to **Settings → Pages**
3. Under **Source**, select: Branch `main`, folder `/ (root)`
4. Click **Save**
5. Your app will be live at `https://YOUR_USERNAME.github.io/REPO_NAME`

### Step 5 — Connect

1. Open your GitHub Pages URL
2. Paste your **Apps Script URL** from Step 3
3. Enter your name
4. Click **Connect & Launch →**

Done. 🎉

---

## Re-deploying After Code Changes

If you ever update `Code.gs`, you must create a **new deployment** for the changes to take effect:

1. Apps Script → **Deploy → Manage deployments**
2. Click the pencil icon on your deployment
3. Change **Version** to `New version`
4. Click **Deploy**
5. The URL stays the same — no changes needed in the app.

---

## File Structure

```
clearmind/
├── index.html   ← The entire app (single file, no build step)
├── Code.gs      ← Google Apps Script backend (paste into Apps Script editor)
└── README.md    ← This file
```

---

## How the Data Flows

```
Your Browser (GitHub Pages)
       │
       │ HTTPS GET request with query params
       ▼
Google Apps Script Web App  ← free, serverless, runs as you
       │
       │ reads/writes rows
       ▼
Your Google Sheet           ← your data, your account
```

All API calls are standard `GET` requests — no CORS issues, no auth tokens needed in the browser.

---

## Google Sheet Structure

The `Entries` sheet is auto-created with these columns:

| Date | DeepWork | Output | Mastery | Health | Discipline | Score | Notes | SavedAt |
|---|---|---|---|---|---|---|---|---|
| 2024-01-15 | 1 | 1 | 0 | 1 | 1 | 4 | Great day | 2024-01-15 21:00 |

You can open the sheet anytime to view, edit, or export your data.

---

## Privacy

- **No accounts.** No sign-up. No passwords.
- **Your data lives in your Google Drive.** Only accessible by you.
- **No third-party servers.** The app talks directly to Google's servers via Apps Script.
- The only thing stored in your browser (`localStorage`) is your Apps Script URL and your name.

---

## Customising the Habits

The 5 habits are defined in `index.html` at the top of the `<script>` block:

```javascript
const HABITS = [
  { id:'deepWork',   cat:'Deep Work',  label:'Completed 90-min focus session before 9 AM?' },
  { id:'output',     cat:'Output',     label:'Pushed at least one commit to GitHub?' },
  { id:'mastery',    cat:'Mastery',    label:'Explained a "Why" instead of just "How"?' },
  { id:'health',     cat:'Health',     label:'Under 2 hours non-work screen time?' },
  { id:'discipline', cat:'Discipline', label:'Finished the specific planned task?' },
];
```

Edit the `cat` and `label` fields to match your habits. Keep exactly 5 entries — the scoring system and Sheet columns are tied to 5 habits.

> ⚠️ If you change habit IDs after you've already logged data, the dashboard charts will still work (they use column positions), but old data will show under the new labels.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| "Connection failed" on test | Make sure you set **Who has access: Anyone** when deploying |
| App shows old data after changing `Code.gs` | Deploy a **new version** (see Re-deploying section above) |
| "Script function not found" error | Make sure `doGet` function exists in your `Code.gs` |
| Notes are cut off | Notes are limited to 600 characters (URL length limit) |
| CORS error in console | You're on `file://` locally — deploy to GitHub Pages or run a local server |

---

## Running Locally

```bash
# Python 3
python3 -m http.server 8080

# Node
npx serve .
```

Then open `http://localhost:8080`.

> Opening `index.html` directly as a `file://` URL will cause CORS errors when fetching from Apps Script. Always use a local server.

---

Built for the Clear Mind productivity framework. Stay disciplined. 🎯
