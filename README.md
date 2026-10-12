🔥 **Survivor 51 Bingo** 🔥
Outwit. Outplay. Outmark.

A self-contained, mobile-friendly bingo site for watching Survivor 51 with friends. Every player gets a randomized 5x5 card of show-specific prompts, plus personal Winner, Jury, and Loser picks. Players click along in real time while the host confirms what actually happened each episode.

Built with R Markdown. Renders to two static HTML pages.

**Features**
Randomized cards for each player: 21 prompt squares, a Winner pick, a Jury pick, a Loser pick, and a FREE space in the center
Two kinds of marks
🟡 Claimed (yellow dauber): click any square on your own card. Stored in your browser's localStorage only, so it is private and "unofficial."
🟢 Confirmed (green dauber): set by the host in data/marks.csv and baked in at render time. Only confirmed marks count toward BINGO.

**Project Structure**
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
    └── survivant.woff2 # Custom font

**Requirements**
R
Pandoc (bundled with RStudio)
R packages: rmarkdown, htmltools
Data Files
 - data/prompts.csv
 - data/picks.csv
 - data/marks.csv

**Usage**
1. Generate the cards (once)
2. Build the site
3. Confirm marks after each episode

**Troubleshooting**
index.Rmd runs a few sanity checks and will stop with an error if:
 - A data file is missing or in the wrong location (the error message prints the working directory)
 - A player in cards.rds has no matching row in picks.csv
 - A prompt ID in marks.csv does not exist in prompts.csv

Also note that data/marks.csv IDs are read as text, so player names for the Winner, Jury, and Loser rows must match the card names.

**Hosting**
The output is plain static HTML, so it can be served from anywhere, such as GitHub Pages, Netlify, or a shared drive. Include the images/ and font/ folders alongside the HTML files so the logo and custom font load.

Created by Nicole Stucke-Downes, PhD
