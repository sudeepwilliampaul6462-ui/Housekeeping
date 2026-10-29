# Hotel Housekeeping Board

A simple web app for a 20-room hotel. Housekeepers and reception can see every room's cleaning status, assign rooms, add notes and export a daily report. It's a single file (`index.html`), hosted free on GitHub Pages.

**Features**
- 20 rooms (101–110, 201–210), colour-coded: Dirty / In progress / Clean / Inspected / Out of order
- Occupancy: Vacant, Stayover, Check-out today, Arrival today
- Assign housekeepers, mark priority rooms (⭐), add notes
- Clickable counters + filters by floor, housekeeper, occupancy, room number
- Activity log with times
- "Start new day" button resets occupied rooms to Dirty
- Export CSV (opens in Excel)
- Settings: change hotel name, staff list, room list

> ⚠️ Data is saved **in the browser of each device**. Two phones won't see each other's changes yet — see "Next step: shared data" at the bottom.

---

## Part 1 — Put the app online (first time, ~15 minutes)

### Step 1: Create a GitHub account
1. Go to **github.com** → **Sign up**.
2. Pick a username (it becomes part of your web address, e.g. `sudeep-hotel`).
3. Verify your email.

### Step 2: Create a repository (a project folder on GitHub)
1. Click the **+** (top right) → **New repository**.
2. Repository name: `housekeeping`
3. Set it to **Public** (free GitHub Pages needs public, unless you pay for Pro).
4. Tick **Add a README file**.
5. Click **Create repository**.

### Step 3: Upload the app
1. In your new repository click **Add file → Upload files**.
2. Drag in `index.html` (and this `README.md` if you like — it'll replace the default one).
3. At the bottom, in "Commit changes", type a message like `First version`.
4. Click **Commit changes**.

> A **commit** = a saved snapshot of your files. GitHub keeps every one, so you can always go back.

### Step 4: Turn on GitHub Pages (makes it a live website)
1. In the repository, click **Settings** (top menu).
2. Left sidebar → **Pages**.
3. Under **Build and deployment → Source**, choose **Deploy from a branch**.
4. Branch: **main**, folder: **/ (root)** → **Save**.
5. Wait 1–2 minutes, refresh the page. You'll see:
   **"Your site is live at `https://YOUR-USERNAME.github.io/housekeeping/`"**

That's your app. Open it on your phone or the reception PC.

### Step 5: Add it to phones like an app
- **iPhone (Safari):** Share button → **Add to Home Screen**
- **Android (Chrome):** ⋮ menu → **Add to Home screen**

---

## Part 2 — Using the app

| Action | How |
|---|---|
| Update a room | Tap the room card → pick status, occupancy, housekeeper, notes → **Save** |
| See only dirty rooms | Tap the **Dirty** counter at the top |
| See one housekeeper's rooms | Use the "All housekeepers" dropdown |
| Start of shift | Tap **↻ Start new day** |
| End-of-day report | Tap **⬇ Export CSV** → opens in Excel |
| Change hotel name / staff / rooms | **⚙ Settings** (room list format: `101, Double` one per line) |

---

## Part 3 — Editing the website

### Option A: Edit directly on GitHub (easiest, no software)
1. Open your repository → click **index.html**.
2. Click the **✏️ pencil icon** (top right of the file).
3. Make your change (see examples below).
4. Click **Commit changes…** → write a short message → **Commit changes**.
5. Wait ~1 minute and refresh your live site (on phone, pull down to refresh).

### Option B: Edit on your computer (better for bigger changes)
1. Install **VS Code** (code.visualstudio.com).
2. Download `index.html`, open it in VS Code.
3. Double-click the file to open it in your browser — refresh to see changes as you go.
4. When happy, upload it again on GitHub (**Add file → Upload files**, same name replaces it) → commit.

> Later you can install **GitHub Desktop** to push changes with one click instead of re-uploading.

### Common edits (search for these in `index.html` with Ctrl+F)

**Change the colours** — near the top, in `:root`:
```css
--dirty: #ef4444;      /* red */
--clean: #10b981;      /* green */
--accent: #2563eb;     /* buttons */
```
Pick colours at htmlcolorcodes.com.

**Change the default housekeepers** — find `DEFAULT_STAFF`:
```js
const DEFAULT_STAFF = ["Anna", "Maria", "Priya", "John"];
```
(Or just use ⚙ Settings in the app — no code needed.)

**Change room numbers/types** — find `function defaultRooms()`. Floors are `[1, 2]`, rooms per floor `i <= 10`. Room types cycle through:
```js
const types = ["Single", "Double", "Twin", "Double", "Family"];
```

**Add a new cleaning status** (e.g. "Pickup") — two places:
1. In `STATUSES` add a line: `pickup: "Pickup",`
2. In the CSS add: `.s-pickup { border-top-color: #a855f7; } .b-pickup { background: #a855f7; }`

**Add a new occupancy type** — add a line to `OCCUPANCY`, e.g. `late: "Late check-out",`

> ⚠️ After changing room/staff defaults in code, existing devices keep their saved data. Use **⚙ Settings → Reset all data** on that device to load the new defaults.

### Made a mistake? Roll back
1. Repository → click **index.html** → **History** (clock icon).
2. Click the last good version → **⋯ → View file** → copy everything.
3. Edit `index.html`, paste over it, commit.

---

## How the code is organised

`index.html` has three parts:
1. **`<style>`** — the look (colours, sizes, layout)
2. **HTML body** — the header, counters, toolbar, room grid, pop-up windows
3. **`<script>`** — the logic, split into numbered sections:
   1. Settings you can edit (statuses, occupancy, staff, rooms)
   2. Saving & loading (browser storage)
   3. Drawing the screen
   4. Editing a room
   5. Daily tools (new day, CSV)
   6. Settings window
   7. Start the app

---

## Next step: shared data across devices
Right now each phone/PC stores its own copy. For housekeepers and reception to see the same live board, connect a free cloud database — **Supabase** or **Firebase**. The plan:
1. Create a free Supabase project, add a `rooms` table.
2. Replace the `load()` and `save()` functions with calls to Supabase.
3. Add a simple login so only staff can edit.
