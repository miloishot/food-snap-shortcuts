# Food Snap Shortcuts

iPhone Shortcuts for Food Snap: log a meal from a photo, re-log a usual meal, mark a dragon boat training day, and get a 9 pm summary.

Tap a link on your iPhone → **Add Shortcut** → open it once and paste **your own** key into the first box (it says `PASTE-KEY-HERE`).

| Shortcut | What it does | Import |
|---|---|---|
| **Food Snap** | Photo → estimate → Log it / Change amount / Cancel | _link coming_ |
| **Today** | Shows today's kcal and protein vs your goal | _link coming_ |
| **Trained today** | Marks today as a training day (+500 kcal, +75 g carbs) | _link coming_ |
| **Usual** | Pick a saved meal and log it, no photo | _link coming_ |
| **Save as Usual** | Saves the meal you just logged under a name | _link coming_ |

> **Never share a shortcut that has your real key in it.** Anyone with your key can log food into your diary. Before you tap *Copy iCloud Link*, change the key box back to `PASTE-KEY-HERE`.

---

## Set up the 9 pm summary (once, about 1 minute)

Automations can't be shared, so each phone sets this up once:

1. Shortcuts → **Automation** → **+** → **Time of Day**
2. **21:00**, **Daily**, **Run Immediately** → Next
3. Pick the **Today** shortcut → Done

---

## Build them yourself (if a link isn't up yet)

Every shortcut starts with the same two actions:

1. **Text**: your key (or `PASTE-KEY-HERE` when sharing)
2. **Set Variable** `Key` to *Text*

Every **Get Contents of URL** below uses header `X-Food-Snap-Key` = variable **Key**.
Server: `https://gram.tailcbf335.ts.net:10000/food` (works only with Tailscale on).

### Today
3. **Get Contents of URL** `…/food/today`, Method **GET**, header as above
4. **Get Dictionary Value** `summary` from *Contents of URL*
5. **Show Notification** *Dictionary Value*

### Trained today
3. **Get Contents of URL** `…/food/training`, Method **POST**, header as above, no body
4. **Get Dictionary Value** `summary`
5. **Show Notification** *Dictionary Value*

To undo a wrong tap, duplicate it, rename to **Untrain today**, change Method to **DELETE**.

### Usual
3. **Get Contents of URL** `…/food/usual`, Method **GET**, header as above
4. **Get Dictionary Value** `names` from *Contents of URL*
5. **Choose from List** *Dictionary Value*
6. **URL Encode** *Chosen Item*
7. **Get Contents of URL** `…/food/usual/` + *URL Encoded Text* + `/log`, Method **POST**, header as above
8. **Get Dictionary Value** `summary` → **Show Result**

### Save as Usual
Run it right after you log a meal with Food Snap.

3. **Ask for Input** (Text) `Name this meal` (for example `kopi`)
4. **Get Contents of URL** `…/food/usual`, Method **POST**, header as above, Request Body **Form**, field `name` (Text) = *Provided Input*
5. **Get Dictionary Value** `summary` → **Show Result**

### Training from Apple Watch (optional)
Use instead of *Trained today* if a watch is worn for sessions.

3. **Find Health Samples** where *Type* is *Active Energy* and *Start Date* is *today*
4. **Calculate Statistics** → *Sum* of *Health Samples*
5. **Get Contents of URL** `…/food/training`, Method **POST**, header as above, Request Body **JSON**, field `kcal` (Number) = *Statistics Result*
6. **Get Dictionary Value** `summary` → **Show Notification**

Automate it: Automation → **+** → **Workout** → *When a workout ends* → Run Immediately → this shortcut.

---

## How to make a share link (owner)

1. Open the shortcut → change the key box to `PASTE-KEY-HERE`
2. Share icon → **Copy iCloud Link**
3. Paste the link into the table above
4. Put your key back in the box
