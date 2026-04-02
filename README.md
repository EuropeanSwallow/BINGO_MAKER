# Bingo Card Generator

A standalone web app for generating personalised bingo cards as a PDF. No installation, no server, no internet connection required after the first open. Just a single HTML file you double-click in your browser.

---

## Quick Start

1. Download `bingo_card_generator.html`
2. Double-click it to open in Chrome or Firefox
3. Your players and prompts load automatically (pre-loaded with the ESL Birmingham 2026 set)
4. Click **Generate PDF** in the top-right corner
5. A file called `bingo_cards.pdf` downloads to your Downloads folder — one page per player

---

## Interface Overview

The app is split into two main areas: a **sidebar** on the left for configuration, and a **main panel** on the right for previewing and exporting cards.

### Sidebar

#### Players
Displays all current players as tags. Click the **×** on any tag to remove a player. Type a name in the input field and press Enter or click **+ Add** to add a new player.

#### Card Theme
Six colour theme options displayed as coloured circles. Click any swatch to apply it to all cards. The last swatch (multi-coloured) assigns a different colour to each player's card automatically.

#### Options
- **Free space in centre** — toggles the FREE square in the middle cell of the grid on or off
- **Exclude player's own name from card** — when enabled, any prompt that mentions a player's name (including possessives like "Angus's") is removed from that player's pool before selection. This is on by default.
- **Show event title on cards** — adds a subtitle line beneath each player's name on both the preview and the exported PDF. Reveals a text input where you can type the title (e.g. "ESL Birmingham 2026").

#### Stats
Shows a live count of players, total prompts, and how many prompts each card will contain. A status message confirms whether there are enough valid prompts to generate all cards, or warns you if more are needed.

The **Reshuffle cards** button regenerates all cards with a new random selection of prompts, without changing your players or prompts list.

---

### Main Panel

#### Cards Preview tab
Displays a live preview of all generated bingo cards. Use the player name pills at the top to filter down to a single player's card, or select **All** to see every card at once.

Each card preview shows:
- The player's name in the theme colour
- The optional event title if enabled
- BINGO column headers (B / I / N / G / O) with the G column highlighted
- A 5×5 grid of prompts with the FREE square in the centre

#### Prompt Manager tab
A searchable list of all prompts currently loaded. Use the search bar to filter by keyword. Click the **×** on any prompt to delete it. Add individual prompts using the input at the bottom. The **Sort A–Z** button sorts all prompts alphabetically in place.

---

## Importing and Exporting CSV Files

The toolbar at the top provides CSV import and export buttons for both prompts and players. CSV files can be opened and edited directly in Notepad, Excel, or Google Sheets.

### File format

Each file contains one entry per line with no headers:

**bingo_prompts.csv**
```
Someone says 'Align'
Huskar 1st picked
Meepo on mainstage
Drunk political argument
```

**bingo_players.csv**
```
Angus
Robbie
Fraser
```

If a prompt contains a comma, it is automatically wrapped in double quotes on export and correctly unwrapped on import. You do not need to do this manually.

### Exporting

Click **Export CSV → Prompts** or **Export CSV → Players** in the top bar. The file downloads immediately. This is the recommended way to back up your data or share the prompt list with others.

### Importing

Click **Import CSV → Prompts** or **Import CSV → Players**, then select your file. The app reads each line as a separate entry and adds any that are not already in the list. Duplicates are skipped. A confirmation message shows how many entries were added.

---

## Generating the PDF

Click **Generate PDF** in the top-right corner. The button is disabled if any player does not have enough valid prompts to fill a card.

The PDF is saved as `bingo_cards.pdf` in your browser's default Downloads folder. It contains one A4 page per player with:

- A dark background
- The player's name in large text, coloured to match the chosen theme
- The optional event title if enabled
- A BINGO header row above the grid
- A 5×5 grid of rounded cells containing the player's 24 randomly selected prompts
- The FREE square centred in the grid (if enabled)
- A thin coloured rule beneath the player name

---

## Data Persistence

Player names, prompts, and the chosen theme are automatically saved to your browser's local storage every time you make a change. The storage key is `bingo_gen_v2`.

This means:
- Closing and reopening the HTML file restores your last session automatically
- Data is stored only on your own machine and is never sent anywhere
- Clearing your browser's site data will erase the saved state. Use Export CSV to back up your prompts before doing this.

If you share the HTML file with someone else, it will load with the default ESL Birmingham 2026 prompts and players on their machine, since local storage is per-browser.

---

## How the Card Generation Works

This section describes the logic that runs when cards are generated or reshuffled.

### Prompt filtering

For each player, the full prompt list is filtered to exclude any prompt that mentions that player's name. The match uses a regular expression that catches:
- Exact name matches (e.g. "Angus")
- Possessives (e.g. "Angus's" or "Angus'")
- Case-insensitive matches

This filtering only applies when the **Exclude player's own name from card** option is enabled.

### Random selection

From each player's filtered prompt pool, 24 prompts are selected at random without replacement (or 25 if the FREE space is disabled). The selection is reshuffled every time you click **Reshuffle cards** or make a change that affects the prompt pool.

### Grid layout

Selected prompts are placed into a 5×5 grid, left-to-right, top-to-bottom. If the FREE space option is enabled, the centre cell (row 3, column 3) is always the FREE square and is excluded from the 24 prompt slots.

### Error handling

If a player has fewer valid prompts than required after name filtering, their card displays an error rather than generating. The **Generate PDF** button is disabled until all players have enough prompts.

---

## Adding and Editing Prompts

The simplest way to manage prompts year to year is:

1. Click **Export CSV → Prompts** to save the current list
2. Open the CSV in Notepad or Excel
3. Add, edit, or delete lines
4. Save the file
5. Click **Import CSV → Prompts** to load the updated list

Alternatively, use the **Prompt Manager** tab to add or delete prompts individually.

---

## External Dependencies

The app loads two resources from the internet the first time it is opened:

| Resource | Source | Purpose |
|---|---|---|
| Bebas Neue, DM Sans, DM Mono fonts | fonts.googleapis.com | Typography |
| jsPDF 2.5.1 | cdnjs.cloudflare.com | PDF generation |

Both are standard public CDNs. No data is sent to either — they only serve files. Once loaded, the fonts and library are cached by your browser and the app will work offline on subsequent uses.

No API keys, passwords, credentials, or personal information are embedded in the file.

---

## Troubleshooting

**The PDF button is greyed out**
A player does not have enough valid prompts. Check the stats panel — the alert will say which condition is not met. Either add more prompts or disable the name-exclusion option.

**My data disappeared**
Browser local storage was cleared (e.g. by clearing browsing data). Use Export CSV regularly to keep a backup.

**Fonts look wrong or plain**
The Google Fonts request failed, probably because the file was opened without an internet connection before the fonts cached. Connect to the internet and reload the file once to cache them.

**The PDF looks different from the preview**
The preview uses your screen's rendering. The PDF is generated by jsPDF which uses its own layout engine — minor differences in text wrapping are normal, especially for longer prompts.

**Importing a CSV added nothing**
All entries in the file were duplicates of what was already loaded. Try exporting first to see what is currently in the list.
