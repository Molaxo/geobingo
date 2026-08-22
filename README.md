# GeoBingo

Offline, stream-friendly GeoBingo board for online exploration. Create a bingo card with custom tasks, play it together with Google Maps / Street View, and use it as a transparent overlay in OBS for your stream.

Live version: just open `index.html` – no build, no backend, no tracking.

---

### What is GeoBingo?

You get a 3×3 or 5×5 board. Each cell is a task like "Fountain", "Abandoned building", "Dog in clothes", "Gas station from the 90s".

Players have to find the spot on Google Maps / Google Street View and prove it with a link. Clicking a cell marks it as **found** and optionally opens the proof link (Maps / Street View URL).

Perfect for:
- GeoGuessr-style community streams
- Google Maps challenges
- Discord / Twitch community games
- Online exploration with friends

### Features

- **Offline-first:** Everything runs in the browser. State is saved in `localStorage` and in the URL hash.
- **Grid sizes:** 3×3 (9) and 5×5 (25)
- **Adjustable text size & custom title** for streams
- **Themes:** City, Nature, Funny, Road – one-click to fill the board
- **Shuffle:** Randomize all tasks + proof links + found state. Optional auto-shuffle when importing.
- **Found / Ready:** Toggle cells. Found cells turn green.
- **Proof links:** Each cell can store a Google Maps / Street View URL. Clicking a found cell opens the link. A 🔗 icon indicates a link is present.
- **Timer:** Built-in timer for runs
- **URL Sharing:** The full board state is encoded in `window.location.hash` as `#s=...`. Just copy the URL – anyone opening it gets your exact board.
- **Auto-sync:** If you open Control and Overlay in two tabs, they sync via `localStorage` events.

### How to Play (with Google Maps)

1.  Create your board (see Import below)
2.  For each task, players search on Google Maps / Street View
3.  When found, paste the proof link into the cell's second input: `Paste Google Maps / Street View link`
4.  Click the cell to mark it as found. Clicking again opens the proof.
5.  Bingo when you complete a row, column or diagonal!

### OBS Setup – IMPORTANT

Use this as a **Window Capture / Display Capture**, NOT as a Browser Source. If you use it as a Browser Source, the sync between Control and Overlay breaks and the board won't update.

**Why not Browser Source?**

The board state is saved in `localStorage` and in the URL hash (`#s=...`) and synced via the `storage` event between tabs. OBS Browser Sources run in an isolated context – they don't share `localStorage` with your Chrome/Firefox window and don't see the hash updates. Result: your overlay stays empty or frozen.

Window Capture / Display Capture captures the real browser tab, so it always shows the live synced state.

**Step-by-step:**

1.  In GeoBingo, click **Overlay Mode ↗** – a new window opens at `?mode=overlay#s=...`
2.  Keep your main GeoBingo tab (Control) open – this is where you edit tasks, paste Maps links, and mark cells as found.
3.  In OBS:
    - Add Source -> **Window Capture** -> Select the Overlay browser window
    - OR: Add Source -> **Display Capture** and crop to just the overlay window
    - Do NOT use Browser Source.
4.  Place the capture over your Google Maps / Street View capture.
5.  Both windows sync automatically – marking a cell in Control instantly updates the Overlay in OBS.

> Tip: In Window Capture, set "Window Match Priority: Window Title must match" so OBS keeps tracking the overlay tab even after reloads.

### Configuration Import

For quickly creating a board from a list.

**Where:** Left panel -> `📄 Import Configuration`

**Format:** Semicolon `;` separated

```
Brick house; Old church; Yellow car; Park; Fountain; ...
```

- Paste as many as you have. It will fill the board in order.
- If you have a 3×3 board, only the first 9 are used. Extras are ignored.
- Empty entries are skipped.
- `Auto-shuffle when applying config` will randomize after import.

This is stored as `geobingo_last_config` so you don't lose it.

### Full State Import / Export (Term + Source)

For saving your entire game including proof links.

**Where:** Left panel -> `🗂 Full card (term | link)`

**Format:** One line per cell, format `Term | https://...`

```
Brick house | https://maps.google.com/?q=...
Old church | https://www.google.com/maps/@...
Yellow car
Park | https://maps.app.goo.gl/xyz
```

- `|` is the separator. Text before `|` = task, after `|` = proof URL.
- Without `|` it imports as task only.
- **Import full card** reads the textarea and overwrites tasks + links.
- **Export** generates the current board into that format for copy/paste, backup, or sharing on GitHub.

You can also **Copy Link** (Share button) – the whole board (tasks, found state, links, title, grid size, timer) is in the URL. Perfect for viewers.

### Local Development / Hosting

It's a single `index.html` with React 18 via ESM. No build step.

```bash
# just serve it
npx serve .
# or
python3 -m http.server
```

State keys:
- `geobingo_full` / `geobingo_v5` – full board JSON (compressed)
- `geobingo_last_config` – last semicolon list
- `geobingo_auto_shuffle` – auto-shuffle toggle

To reset everything, use **DANGER ZONE -> Reset all** or clear localStorage and remove `#s=` from URL.

### Roadmap Ideas

- Custom colors / streamer branding
- Export as PNG for thumbnail
- Bingo detection + sound
- Team mode

PRs welcome!

### License

MIT – do whatever you want, just keep it fun.
