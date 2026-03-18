# Procurement Information System (PIS)
## Senaka Group — Setup & Deployment Guide

---

## 📁 File Structure

```
pis-app/
├── index.html          ← Main PWA app (the form)
├── manifest.json       ← PWA manifest (home screen icon, splash)
├── sw.js               ← Service Worker (offline support)
├── offline.html        ← Shown when user is offline
├── Code.gs             ← Google Apps Script (backend)
├── icons/
│   ├── icon-72.png
│   ├── icon-96.png
│   ├── icon-128.png
│   ├── icon-144.png
│   ├── icon-152.png
│   ├── icon-192.png
│   ├── icon-384.png
│   └── icon-512.png
└── README.md           ← This file
```

---

## 🚀 STEP-BY-STEP DEPLOYMENT

### STEP 1 — Set Up Google Sheet

1. Open Google Sheets → Create a new spreadsheet
2. Name it exactly: **`Senaka Group - Tender Records`**
3. Go to **Extensions → Apps Script**
4. Delete all existing code in `Code.gs`
5. Paste the entire contents of `Code.gs` from this project
6. Click **Save** (💾 icon or Ctrl+S)
7. From the editor, click **Run → Run function → `setupSheet`**
   - First time: grant permissions when asked
8. Return to your Google Sheet — you should see the **📋 PIS Menu** in the menu bar
9. Your sheet is now ready with all columns, dropdowns, and protection

---

### STEP 2 — Deploy Google Apps Script as Web App

1. In the Apps Script editor, click **Deploy → New Deployment**
2. Click the gear icon ⚙ → Select **Web App**
3. Fill in:
   - **Description**: `PIS Web App v1`
   - **Execute as**: `Me`
   - **Who has access**: `Anyone`
4. Click **Deploy**
5. **COPY the Web App URL** — it looks like:
   `https://script.google.com/macros/s/AKfy.../exec`

> ⚠️ Keep this URL safe — it's your form's backend endpoint

---

### STEP 3 — Update index.html with your URL

1. Open `index.html` in a text editor
2. Find this line near the top of the `<script>` section:
   ```javascript
   const GAS_URL = 'YOUR_GOOGLE_APPS_SCRIPT_WEB_APP_URL_HERE';
   ```
3. Replace `YOUR_GOOGLE_APPS_SCRIPT_WEB_APP_URL_HERE` with your actual URL:
   ```javascript
   const GAS_URL = 'https://script.google.com/macros/s/AKfy.../exec';
   ```
4. Save the file

---

### STEP 4 — Host on GitHub Pages (Free)

1. Create a free account at **github.com** if you don't have one
2. Click **New Repository**
   - Name: `pis-senaka` (or any name you like)
   - Visibility: **Public**
   - Click **Create repository**
3. Upload all files:
   - Click **uploading an existing file**
   - Drag and drop: `index.html`, `manifest.json`, `sw.js`, `offline.html`, and the entire `icons/` folder
   - Click **Commit changes**
4. Go to **Settings → Pages**
5. Under **Source**, select **Deploy from a branch** → **main** → **/ (root)**
6. Click **Save**
7. Your app URL will be:
   `https://YOUR-USERNAME.github.io/pis-senaka/`

> ✅ Wait 1-2 minutes for GitHub to publish. Then open the URL in Chrome on your phone.

---

### STEP 5 — Set Up Daily Email Reminders

1. In your Google Sheet, click **📋 PIS Menu → 🕐 Setup Daily Reminders**
2. This creates two automatic triggers:
   - **8:00 AM**: Daily email summary to all 3 users
   - **1:00 AM**: Auto-expire check

To change email addresses or reminder time, edit these lines in `Code.gs`:
```javascript
EMAIL_USERS: ['user1@gmail.com', 'user2@gmail.com', 'user3@gmail.com'],
REMINDER_HOUR: 8,   // 24-hour format
```
After editing, click **Deploy → Manage deployments → New version** to update.

---

### STEP 6 — Install on Phone (Android)

1. Open **Chrome** on your Android phone
2. Go to your GitHub Pages URL
3. Chrome will show a banner: **"Add PIS to Home screen"** → tap **Install**
4. OR tap the ⋮ menu → **Add to Home screen**
5. The app icon appears on your home screen — tap to open fullscreen

### Install on iPhone (iOS)

1. Open **Safari** (must be Safari, not Chrome)
2. Go to your GitHub Pages URL
3. Tap the **Share button** (box with arrow pointing up)
4. Scroll down → tap **Add to Home Screen**
5. Tap **Add** — the PIS icon appears on your home screen

---

## 🔧 CONFIGURATION REFERENCE

### Update Email Recipients
In `Code.gs`, find:
```javascript
EMAIL_USERS: ['user1@gmail.com', 'user2@gmail.com', 'user3@gmail.com'],
```
Replace with actual email addresses.

### Company Names
In `index.html`, find the `<select id="f_company">` section and add/remove `<option>` tags.

### Tender Types
In `index.html`, find `<select id="f_type">` and edit options.

---

## 📋 GOOGLE SHEET COLUMN GUIDE

| Column | Field | Notes |
|--------|-------|-------|
| A | Submitted At | Auto timestamp |
| B | Date | From form |
| C | Tender Type | From form |
| D | Information Source | From form |
| E | Province | From form |
| F | District | From form |
| G | Job Scope | From form |
| H | Client Name | From form |
| I | Bidding Company | From form |
| J | Project Value (LKR) | From form |
| K | Bid Security (LKR) | From form |
| L | Qualification Requirements | From form |
| M | Document Collection Location | From form |
| N | Doc Fee (LKR) | From form |
| O | Pre-Bid Date | From form |
| P | Pre-Bid Location | From form |
| Q | Project Period | From form |
| R | Closing Date | From form |
| S | Remarks | From form |
| T | Advertisement Image | Drive link |
| **U** | **Priority (1-5)** | **Set by staff** |
| **V** | **Action** | **Collect / Do Not Collect / On Hold** |
| **W** | **Status** | **Completed / Pending / Submitted** |
| X | Days Left | Auto-calculated |

> Columns **U, V, W** are the only editable columns for backend staff.
> All other columns are protected from manual editing.

---

## 📋 PIS MENU OPTIONS

| Menu Item | What it does |
|-----------|-------------|
| 🔧 Setup Sheet | Run once to initialise everything |
| 📊 Generate PDF Report | Creates a styled report + emails to 3 users |
| 📧 Send Instant Reminder | Immediately emails all active "Collect" tenders |
| 🗑 Delete Selected Row | Deletes a row (select the row first, then use menu) |
| 🔄 Recalculate Days Left | Manually refresh Days Left column |
| 🕐 Setup Daily Reminders | Creates the daily trigger (run once) |

---

## 🛡️ SECURITY NOTES

- The Google Sheet is **protected** from manual editing
- Only **Priority**, **Action**, and **Status** columns can be edited by users
- Row deletion is only possible via the **PIS Menu** (with confirmation)
- The Web App runs under your Google account credentials
- Images are uploaded to a private Drive folder and shared via link

---

## ❓ TROUBLESHOOTING

**Form submits but data doesn't appear in sheet:**
- Check that you redeployed after editing Code.gs (always create a **New Version**)
- Make sure the GAS_URL in index.html matches exactly

**"Access denied" error:**
- Redeploy the Web App with **"Who has access: Anyone"**

**Images not uploading:**
- Image files over 10MB may fail on slow connections — ask users to reduce photo quality

**PWA not installing on iPhone:**
- Must use Safari browser (not Chrome on iOS)

---

*Senaka Group – Procurement Information System | Version 1.0*
