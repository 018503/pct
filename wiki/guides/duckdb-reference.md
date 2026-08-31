# DuckDB Team Database Reference

The teams database at `pokemon/data/teams.duckdb` contains every team from the wiki in a queryable format. Use it for metagame analysis, spread comparisons, and teammate correlations.

## Setup

```bash
pip install duckdb
python3 -c "import duckdb; con = duckdb.connect('data/teams.duckdb', read_only=True); print(con.execute('SELECT COUNT(*) FROM teams').fetchone())"
```

Or use the DuckDB CLI directly:

```bash
duckdb pokemon/data/teams.duckdb
```

## Schema

### teams

| Column | Type | Description |
|---|---|---|
| filename | VARCHAR | Primary key — wiki filename stem, e.g. `reg-mb-s2-rank1-mega-garchomp-floette` |
| title | VARCHAR | H1 title from the wiki page |
| regulation | VARCHAR | `MA`, `MB`, `MB Season 2`, etc. |
| rental_code | VARCHAR | In-game rental code (if known) |
| placement | VARCHAR | `1st Place`, `Top 8`, or empty |
| pokepaste | VARCHAR | Full `pokepast.es` URL |
| creator | VARCHAR | Creator handle |
| source_title | VARCHAR | Video title |
| source_url | VARCHAR | Video URL |
| team_size | INTEGER | Number of Pokemon (usually 6) |

### pokemon

| Column | Type | Description |
|---|---|---|
| team_filename | VARCHAR | FK to `teams.filename` |
| regulation | VARCHAR | Regulation (denormalized for convenience) |
| pokepaste | VARCHAR | Pokepaste URL (denormalized) |
| slot | INTEGER | 1-6 position on the team |
| pokemon | VARCHAR | Species name, e.g. `Garchomp`, `Floette-Eternal`, `Arcanine-Hisui` |
| item | VARCHAR | Held item |
| mega_form | VARCHAR | Mega evolution name, or empty |
| ability | VARCHAR | Ability |
| nature | VARCHAR | Nature with modifiers, e.g. `Adamant (+Atk, -SpA)` |
| hp_base, hp_points, hp_final | INTEGER | HP base stat, invested points, calculated final |
| atk_base ... spe_final | INTEGER | Same triple for Atk, Def, SpA, SpD, Spe |
| total_points | INTEGER | Sum of all 6 point values (should be 66) |
| move_1 .. move_4 | VARCHAR | Moveset |

Primary key: `(team_filename, slot)`.

### Stat points system

Champions uses points (0-32 per stat, 66 total) instead of traditional EVs. The formulas:

```
HP    = floor((2 * Base + 31 + Points * 2) / 2) + 60
Other = floor((floor((2 * Base + 31 + Points * 2) / 2) + 5) * NatureMod)
```

## Common Queries

### Find all teams containing a specific Pokemon

```sql
SELECT t.title, t.regulation, t.placement, t.creator,
       p.nature, p.item, p.ability,
       p.hp_points||'/'||p.atk_points||'/'||p.def_points||'/'||p.spa_points||'/'||p.spd_points||'/'||p.spe_points AS spread,
       p.move_1 || ' · ' || p.move_2 || ' · ' || p.move_3 || ' · ' || p.move_4 AS moves
FROM pokemon p
JOIN teams t ON p.team_filename = t.filename
WHERE p.pokemon = 'Garchomp'
ORDER BY t.regulation, t.title;
```

### Most used Pokemon overall

```sql
SELECT pokemon, COUNT(*) AS usage, 
       ROUND(COUNT(*) * 100.0 / (SELECT COUNT(DISTINCT team_filename) FROM pokemon), 1) AS pct
FROM pokemon
GROUP BY pokemon
ORDER BY usage DESC
LIMIT 15;
```

### Most common teammates for a Pokemon

```sql
SELECT p2.pokemon, COUNT(*) AS cnt
FROM pokemon p1
JOIN pokemon p2 ON p1.team_filename = p2.team_filename AND p1.slot != p2.slot
WHERE p1.pokemon = 'Incineroar'
GROUP BY p2.pokemon
ORDER BY cnt DESC
LIMIT 10;
```

### Compare spreads for a Pokemon across teams

```sql
SELECT t.title, p.nature, p.item,
       p.hp_points AS hp, p.atk_points AS atk, p.def_points AS def,
       p.spa_points AS spa, p.spd_points AS spd, p.spe_points AS spe,
       p.hp_final||'/'||p.atk_final||'/'||p.def_final||'/'||p.spa_final||'/'||p.spd_final||'/'||p.spe_final AS finals
FROM pokemon p
JOIN teams t ON p.team_filename = t.filename
WHERE p.pokemon = 'Kingambit'
ORDER BY p.nature, p.spe_points;
```

### Average spread by nature for a Pokemon

```sql
SELECT nature, COUNT(*) AS n,
       ROUND(AVG(hp_points),1) AS hp, ROUND(AVG(atk_points),1) AS atk,
       ROUND(AVG(def_points),1) AS def, ROUND(AVG(spa_points),1) AS spa,
       ROUND(AVG(spd_points),1) AS spd, ROUND(AVG(spe_points),1) AS spe
FROM pokemon
WHERE pokemon = 'Sneasler'
GROUP BY nature
ORDER BY n DESC;
```

### Most common items for a Pokemon

```sql
SELECT item, COUNT(*) AS cnt
FROM pokemon
WHERE pokemon = 'Garchomp'
GROUP BY item
ORDER BY cnt DESC;
```

### Speed tier analysis — who outspeeds what

```sql
SELECT pokemon, nature, spe_base, spe_points, spe_final, COUNT(*) AS n
FROM pokemon
WHERE spe_final >= 150
GROUP BY pokemon, nature, spe_base, spe_points, spe_final
ORDER BY spe_final DESC;
```

### Move usage for a Pokemon

```sql
SELECT move, COUNT(*) AS cnt
FROM (
    SELECT move_1 AS move FROM pokemon WHERE pokemon = 'Charizard'
    UNION ALL SELECT move_2 FROM pokemon WHERE pokemon = 'Charizard'
    UNION ALL SELECT move_3 FROM pokemon WHERE pokemon = 'Charizard'
    UNION ALL SELECT move_4 FROM pokemon WHERE pokemon = 'Charizard'
)
GROUP BY move
ORDER BY cnt DESC;
```

### Pokemon that always appear together (core pairs)

```sql
SELECT p1.pokemon AS mon1, p2.pokemon AS mon2, COUNT(*) AS pair_count
FROM pokemon p1
JOIN pokemon p2 ON p1.team_filename = p2.team_filename AND p1.pokemon < p2.pokemon
GROUP BY p1.pokemon, p2.pokemon
HAVING pair_count >= 5
ORDER BY pair_count DESC
LIMIT 15;
```

### Teams by regulation

```sql
SELECT regulation, COUNT(*) AS teams,
       SUM(team_size) AS total_pokemon
FROM teams
GROUP BY regulation
ORDER BY teams DESC;
```

### Find teams with a specific two-Pokemon core

```sql
SELECT t.title, t.regulation, t.creator
FROM teams t
WHERE EXISTS (SELECT 1 FROM pokemon p WHERE p.team_filename = t.filename AND p.pokemon = 'Charizard')
  AND EXISTS (SELECT 1 FROM pokemon p WHERE p.team_filename = t.filename AND p.pokemon = 'Aerodactyl')
ORDER BY t.regulation;
```

### Data quality check — flag bad stat totals

```sql
SELECT team_filename, pokemon, total_points, pokepaste
FROM pokemon
WHERE total_points < 50
ORDER BY total_points;
```
