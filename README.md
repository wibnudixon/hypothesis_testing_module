# Third-Place Match Behavioral Analysis

## Overview

This project examines whether **third-place matches differ behaviorally from semifinal matches** in football, using player-level match data scraped from Sofascore.

The goal is to build a reusable pipeline that:

- scrapes match data
- cleans and standardizes the output
- selects relevant variables
- filters the player pool fairly
- compares player behavior across match contexts
- supports France vs England analysis

The project is designed around a **behavioral football lens**, focusing on how players act under different levels of pressure rather than only on the result itself.

---

## Project Structure

```text
third_place_analysis/
├── data/
│   ├── raw/
│   ├── clean/
│   └── outputs/
├── figures/
├── notebooks/
│   └── main.ipynb
├── scripts/
│   ├── scrape_matches.py
│   ├── clean_data.py
│   ├── analysis.py
│   └── viz.py
├── README.md
└── requirements.txt
```

---

## Prerequisites

- Python 3.10+
- pandas
- numpy
- matplotlib
- seaborn
- ScraperFC
- optionally PySpark if you want to scale the workflow

---

## Installation

### 1. Set up the project
Open the project folder in VS Code.

### 2. Create a virtual environment

```bash
python -m venv venv
venv\Scripts\activate
```

### 3. Install dependencies

```bash
pip install pandas numpy matplotlib seaborn ScraperFC jupyter
```

If using Spark:

```bash
pip install pyspark
```

---

## Usage

### 1. Scrape match data
Use the scraping script or notebook to collect player-match data for the relevant England and France matches.

The raw output should include:
- player identity fields
- match metadata
- stats fields
- team information
- timestamps

### 2. Clean the data
Run the reusable cleaning pipeline to:
- standardize column names
- convert data types
- remove duplicate or irrelevant variables
- standardize team names
- prepare one clean dataframe

### 3. Select variables
Use a separate code cell to choose which variables will be included in the final analysis.

This keeps the workflow flexible so it can be reused on:
- this project
- other football datasets
- other analytical agendas

### 4. Filter the player pool
Apply inclusion rules to avoid skewed results:
- remove players with very low minutes
- optionally require at least two matches
- use per-90 stats where appropriate

### 5. Build comparisons
Compute:
- pre-semifinal baseline for each player
- baseline vs semifinal
- baseline vs third-place match

### 6. Generate summaries and charts
Produce:
- player-level comparison tables
- team-level summaries
- behavioral charts
- France vs England comparisons

---

## Input Data Format

The raw data is expected to be at the **player-match level**.

Typical fields include:

- `player_name`
- `team`
- `match_id`
- `minutes_played`
- `start_time`
- tackles, interceptions, and clearances
- dispossessions and unsuccessful touches
- carries, progression, and sprints
- passing actions
- shots, key passes, and chance creation
- match metadata such as score, tournament, season, and status

---

## Cleaning Philosophy

The cleaning system is designed to be **reusable and dataset-agnostic**.

It should:
- work on the Sofascore dataset now
- work on a different football dataset later
- keep cleaning separate from variable selection
- allow users to adapt the analysis to their own agenda

### Cleaning steps
- normalize column names
- remove redundant metadata fields
- convert numeric values properly
- convert timestamps
- drop exact duplicates
- preserve only meaningful analytical columns

### Variable selection
A separate cell should define the final analysis columns based on convenience and research needs.

---

## Analysis Framework

### Main behavioral groups
- **Defensive intensity**
- **Ball security / error-proneness**
- **Progressive / expressive movement**
- **Passing ambition**
- **Physical engagement**
- **Attacking intent**

### Core comparisons
- player baseline vs semifinal
- player baseline vs third-place match
- France vs England
- team-level match context comparisons

---

## Design Document

### Grain of the analysis table
A single row represents a **player in a specific match**, with the associated match metadata and performance stats.

This grain is appropriate because the project compares:
- the same player across different matches
- team behavior across match contexts
- match-specific behavior in semifinal and third-place settings

### Tracked vs non-tracked attributes

#### Tracked attributes
These are the variables that should drive historical or behavioral analysis:

- `total_tackle`
- `won_tackle`
- `interception_won`
- `total_clearance`
- `last_man_tackle`
- `blocked_scoring_attempt`
- `possession_lost_ctrl`
- `dispossessed`
- `unsuccessful_touch`
- `error_lead_to_a_shot`
- `total_progression`
- `total_ball_carries_distance`
- `progressive_ball_carries_count`
- `number_of_sprints`
- `total_long_balls`
- `accurate_long_balls`
- `total_cross`
- `accurate_cross`
- `key_pass`
- `big_chance_created`
- `goal_assist`
- `duel_won`
- `duel_lost`
- `challenge_lost`
- `was_fouled`
- `aerial_won`
- `aerial_lost`
- `total_shots`
- `shot_off_target`
- `on_target_scoring_attempt`
- `expected_goals`
- `goals`

These variables are behaviorally meaningful and reflect how a player acts in different match situations.

#### Non-tracked attributes
These are mainly identity, metadata, or technical fields and should usually not drive the main analysis:

- `name`
- `firstName`
- `lastName`
- `slug`
- `shortName`
- `jerseyNumber`
- `shirtNumber`
- `userCount`
- `gender`
- `sofascoreId`
- `country`
- `fieldTranslations`
- `marketValueCurrency`
- `dateOfBirthTimestamp`
- `proposedMarketValueRaw`
- `ratingVersions`
- `statisticsType`
- `captain`

These are useful for identification or reference, but they do not represent behavioral change.

---

## Effective dating approach


### Baseline
For each player, compute:
- the average value of each tracked statistic across all matches **before the semifinal**

This becomes the player’s baseline.

### Comparison matches
Then compare the baseline with:
- the semifinal match
- the third-place match

### Why this works
This gives a player-specific reference point and reduces bias from:
- varying match roles
- changing minutes
- different teams or match states

For a more robust version, the stats should also be normalized per 90 minutes.

---

## Medallion-style positioning

The project can be organized in a Medallion-like flow:

### Bronze layer
Raw scraped match files from Sofascore.

### Silver layer
Cleaned, standardized, analysis-ready player-match table.

### Gold layer
Final tables and charts:
- baseline comparisons
- France vs England summaries
- behavioral visualizations

---

## Downstream use

The cleaned dataset can be used to:
- compare semifinal vs third-place match behavior
- identify whether third-place matches are looser or more expressive
- compare France and England
- support article or report writing with data-backed evidence

---

## Incremental processing approach

The process can be repeated whenever new match files are added.

The workflow should:
1. scrape new matches
2. append them to the raw layer
3. run the same cleaning pipeline
4. update the analysis tables
5. regenerate charts

Because the workflow is modular, future datasets can follow the same pattern.

---

## Data quality considerations

Potential issues include:
- inconsistent column names
- missing values
- players with very low minutes
- players appearing in only one match
- goalkeeper-only rows mixed with outfield players
- nested metadata fields
- duplicate rows

### How the pipeline handles this
- standardizes columns
- coerces types
- removes duplicates
- supports variable selection
- filters low-minute players
- optionally separates goalkeepers from outfield analysis

---

## Assumptions made

- The data is already scraped from Sofascore.
- Match IDs for England and France are known.
- The third-place match is identifiable by match ID.
- The analysis is centered on France and England only.
- The final behavioral comparison should focus on outfield players.
- Players with very low minutes may be excluded or normalized per 90.

---

## Suggested workflow

1. scrape the match data
2. clean and standardize the dataframe
3. inspect available columns
4. choose final variables in a separate code cell
5. filter the player pool fairly
6. compute player baselines
7. compare baseline vs semifinal and third-place
8. generate team summaries
9. build charts
10. write the article

---

## Data quality and fairness rules

To reduce skew:
- use per-90 values where possible
- exclude players with very few minutes if needed
- require a minimum number of appearances if appropriate
- treat goalkeepers separately if they are not part of the core behavioral story