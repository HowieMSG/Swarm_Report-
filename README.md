# Highland Elite Swarm 12/13 — Match Analytics Site

A standalone, filterable stats dashboard for the team. It's a static site (plain HTML/CSS/JS,
no build step, no server, no database) that reads everything from `data/season.json`. Select
one game or several at the top and every chart, table, and the Game Breakdown section updates
to match.

## What's in this folder

```
index.html          the whole site (one file — HTML, CSS and JS all inline)
data/season.json     all match, player, and "key moments" data — this is what you filter over
assets/logo.png      team crest, used in the header and as the browser tab icon
```

## Put it online with GitHub Pages (free, ~5 minutes, no coding)

1. Go to **github.com**, sign in (or create a free account), and click the **+** in the top
   right → **New repository**. Name it something like `swarm-analytics`, leave it **Public**
   (GitHub Pages on a free account needs a public repo), and click **Create repository**.
2. On the new repo's page, click **uploading an existing file** (or drag files directly onto
   the page).
3. Drag in all three items from this folder: `index.html`, the `data` folder, and the `assets`
   folder (GitHub keeps the folder structure automatically). Scroll down and click
   **Commit changes**.
4. Go to the repo's **Settings** tab → **Pages** (left sidebar) → under **Build and
   deployment**, set **Source** to **Deploy from a branch**, **Branch** to `main` / `(root)`,
   and click **Save**.
5. Wait about a minute, then refresh that Pages settings page — it'll show your live URL,
   something like `https://<your-username>.github.io/swarm-analytics/`. That's the link to
   share with coaches, players, or parents.

Any time you want to update the live site later, go back to the repo, open the file you want
to change, click the pencil (✎) icon to edit it right in the browser, and **Commit changes** —
the live site updates automatically within a minute or two. No git command line needed at any
point.

## Editing data yourself (no need to come back to Claude for this)

Open `data/season.json` — it's a plain text/JSON file, editable in GitHub's web editor (pencil
icon) or any text editor. Safe things to hand-edit:

- **`"read"`** — the one-paragraph coaching note under each match card. Rewrite it any time.
- **`"note"`** inside each item under `moments.own` / `moments.opp` — the description text for
  each key moment in the Game Breakdown section. Fix a typo, add context, or swap in a
  different note.
- Team name/colors under `"team"` at the top.

Just keep the quotes and commas intact — if you're not sure, copy the whole file somewhere safe
before editing, and a broken JSON file will just show a "could not load data" message on the
site (nothing will crash beyond that).

**Don't hand-edit the numeric stats** (`goals`, `passes`, `distanceMi`, etc.) or the
`moments.timeline` / `moments.fullLog` minute-by-minute data — those come straight off Veo and
are easy to get subtly wrong by hand. For a new match, or to correct a pulled number, come back
to this Claude conversation (or project) and ask to refresh the data — the workflow for pulling
it from Veo is written up in the project doc (`swarm-analytics-overview.md`), so a future
session can do it the same way.

## Adding a new match later

The cleanest way is to ask Claude to do it — it'll pull the match's team stats, player stats,
and shot/goal events from Veo the same way this data was built, and hand you either an updated
`season.json` to paste in, or do the GitHub commit for you if this session is ever linked to
your GitHub. To do it by hand: copy one of the two objects inside `"matches": [ ... ]` in
`season.json`, paste it as a third entry, and fill in the new match's numbers in the same shape.

## If the site ever shows "could not load data"

`index.html` now carries a backup copy of `data/season.json` baked right into the page. If the
live `data/season.json` file can't be reached for any reason, the site automatically falls back
to that backup and shows a small amber notice at the top instead of a blank error — so a broken
upload no longer takes the whole site down.

That said, the live file is still what you should fix if you see the notice, since the backup
copy won't include any edits made after it was built. The most common cause: when uploading
through GitHub's drag-and-drop screen, dropping the *contents* of the `data` and `assets`
folders (rather than the folders themselves) leaves `season.json` and `logo.png` sitting at the
repo root instead of inside `data/` and `assets/` — which is exactly what the site's relative
`fetch('data/season.json')` won't find. To check: open your repo on github.com and confirm you
see two folders named `data` and `assets` (not loose files named `season.json` / `logo.png` at
the top level). If they're in the wrong place, delete them and re-upload by dragging the actual
`data` and `assets` folder icons onto the upload page — most browsers preserve the folder
structure when you drag a folder directly. It's also worth waiting a minute or two after any
commit — GitHub Pages takes a short moment to rebuild.

## Notes on the data

- Only matches with Veo's **Analytics 2** tier and a finalized lineup show full player-level
  stats — currently that's both games in this file. Matches lineups aren't finalized for
  yet won't have individual player rows.
- "Matches", Starts, Captaincies, and Player-of-the-Match all read 0 for every player because
  Veo's lineups haven't been finalized for either game — assign the roster in Veo (by player
  name, not just jersey number) to unlock those and get names instead of jersey numbers
  throughout the site.
- The Game Breakdown section deliberately doesn't invent a pass-by-pass replay — it cites the
  real AI-tagged minute and jersey number for each moment so a coach can cue the actual Veo
  footage to that point and review it with the team.
