# Pokemon Champions — Team Browser

Browse and compare VGC championship teams from Reg MA and Reg MB.

**Live site:** https://018503.github.io/pct/champions-teams.html

## Features

- Search by Pokemon to find which teams use them
- Side-by-side stat/moveset comparison across builds
- EV spread histograms for popular picks
- Expandable team details with sprites, items, and moves
- Light/dark theme support

## Structure

- `champions-teams.html` — interactive team browser (loads data from `data/`)
- `data/champions-data.json` — team and type data for the HTML browser
- `data/teams.duckdb` — team data in DuckDB format
- `data/megas.duckdb` — mega evolution data in DuckDB format
- `wiki/` — guides and team analyses in markdown
