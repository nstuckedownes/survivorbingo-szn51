# 🔥 Survivor 51 Bingo 🔥

*Outwit. Outplay. Outmark.*

A self-contained, mobile-friendly bingo site for watching Survivor 51 with friends. Every player gets a randomized 5x5 card of show-specific prompts, plus personal Winner, Jury, and Loser picks. Players click along in real time while the host confirms what actually happened each episode.

Built with R Markdown. Renders to two static HTML pages: no server required.

## Features

- **Randomized cards** for each player: 21 prompt squares, a Winner pick, a Jury pick, a Loser pick, and a FREE space in the center
- **Two kinds of marks**
  - 🟡 **Claimed (yellow dauber):** click any square on your own card. Stored in your browser's `localStorage` only, so it is private and unofficial.
  - 🟢 **Confirmed (green dauber):** set by the host in `data/marks.csv` and baked in at render time. Only confirmed marks count toward BINGO.
- **Rule Book page** with definitions, scoring, and grey-area rulings
- **Table of contents** that jumps to each player's card
- **Responsive and print-friendly:** works on phones, and cards print in black and white

## Project Structure

```
.
├── index.Rmd          # Main page: renders every player's card
├── rules.Rmd          # Rule Book page
├── card_setup.Rmd     # One-time script that generates data/cards.rds
├── data/
│   ├── prompts.csv    # Prompt list
│   ├── picks.csv      # Each player's Winner / Jury / Loser picks
│   ├── marks.csv      # Host-confirmed hits (update after each episode)
│   └── cards.rds      # Generated cards (output of card_setup.Rmd)
├── images/
│   └── survivor.png   # Header logo
└── font/
    └── survivant.woff2  # Custom font
```

## Requirements

- [R](https://www.r-project.org/)
- [Pandoc](https://pandoc.org/) (bundled with RStudio)
- R packages: `rmarkdown`, `htmltools`

## Data Files

### `data/prompts.csv`

| Column | Description |
|---|---|
| `prompt_id` | Unique numeric ID |
| `prompt_text` | Text shown in the square |

### `data/picks.csv`

| Column | Description |
|---|---|
| `player` | Player name; must match the names in `card_setup.Rmd` |
| `winner_text` | Who they think wins the season |
| `jury_text` | Who they think makes the jury |
| `loser_text` | Who they think gets voted out |

### `data/marks.csv`

| Column | Description |
|---|---|
| `id` | For `prompt` rows, the `prompt_id`. For `winner`, `jury`, and `loser` rows, the **player name**. |
| `type` | One of `prompt`, `winner`, `jury`, `loser` |
| `hit` | `1` if confirmed, `0` otherwise |

## Usage

### 1. Generate the cards (once to kick the season off)

Knit `card_setup.Rmd`, or run:

```r
rmarkdown::render("card_setup.Rmd")
```

This writes `data/cards.rds`. The script uses `set.seed(314)`, so it is reproducible, but **changing the prompt list or the player list will change every card.** Only regenerate before the season starts.

### 2. Build the site

```r
rmarkdown::render("index.Rmd")
rmarkdown::render("rules.Rmd")
```

### 3. Confirm marks after each episode

1. Open `data/marks.csv` and set `hit` to `1` for everything that happened on the aired broadcast.
2. Re-render `index.Rmd`.
3. Publish the updated `index.html`.

Cards are rebuilt from the data on every render, so nothing else needs to change.

## Troubleshooting

`index.Rmd` runs a few sanity checks and will stop with an error if:

- A data file is missing or in the wrong location (the error message prints the working directory)
- A player in `cards.rds` has no matching row in `picks.csv`
- A prompt ID in `marks.csv` does not exist in `prompts.csv`

Also note that `data/marks.csv` IDs are read as text, so player names for the Winner, Jury, and Loser rows must match the card names.

## Hosting

The output is plain static HTML, so it can be served from anywhere, such as GitHub Pages, Netlify, or a shared drive. Include the `images/` and `font/` folders alongside the HTML files so the logo and custom font load.

## A Note on Fair Play

Claimed marks are tracked per browser, not per player, so nothing technically stops you from clicking someone else's card. Please don't. Your tribe is watching.

## Credits

Nicole Stucke-Downes, PhD
Ryan Downes, punkass
Emma (Gregory) Casey, MA, RD
Dylan Casey, dad
