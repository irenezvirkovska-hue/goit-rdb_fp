# Infectious Disease Data Analysis with PostgreSQL

A final project for the NeoVersity relational databases course. This project uses the `infectious_cases` dataset to demonstrate data import, relational normalization, SQL aggregation, date calculations, and user-defined functions.

## Dataset and Tools

- Dataset: [infectious_cases.csv](https://drive.google.com/file/d/1dQf-uVXzYPYYGgXsG1y4Q1qDdhP2miZl/view).
- Database: PostgreSQL.
- Tools: Python, pandas, SQLAlchemy, psycopg2, and pgserver.
- Execution environment: Google Colab.
- Main notebook: `goit_rdb_fp.ipynb`.

The dataset contains infectious disease indicators by entity and year. Entities include individual countries as well as regional and other aggregate groups. The original notebook's saved outputs show 10,521 records and 245 distinct `Entity`–`Code` pairs.

## Project Tasks

1. **Import data.** Create the `pandemic` schema, import the CSV into `infectious_cases`, and inspect the data structure, types, and missing values.
2. **Normalize the data.** Extract distinct `Entity`–`Code` pairs into an `entities` table. Store years and disease indicators in `infectious_cases_normalized`, referencing the entity through `entity_id`. Define primary and foreign keys and check row counts and referential integrity.
3. **Analyze rabies indicators.** Calculate the average, minimum, maximum, and sum of `Number_rabies` for each entity. Sort by average in descending order and return the top 10 results, excluding groups with no observations for this indicator.
4. **Calculate year differences.** Construct January 1 of each observation year, include the current date, and calculate the difference in complete years using `MAKE_DATE`, `CURRENT_DATE`, `AGE`, and `EXTRACT`.
5. **Create SQL functions.** Implement and use `year_difference(input_year)` to calculate year differences and `average_cases(cases_value, divisor)` to divide an annual indicator by a specified number of periods. Demonstrate divisors of 12, 4, and 2. The second function returns `NULL` for a zero divisor or a missing argument.

## Database Structure

All tables are stored in the `pandemic` schema.

| Table | Purpose |
| --- | --- |
| `infectious_cases` | Original data imported from the CSV |
| `entities` | Entity lookup table containing `id`, `entity`, and `code` |
| `infectious_cases_normalized` | Observation year, disease indicators, and the `entity_id` foreign key |

The tables have a one-to-many relationship: `entities.id` → `infectious_cases_normalized.entity_id`.

The normalization query uses `IS NOT DISTINCT FROM` to match entities with missing codes correctly. The saved validation outputs show 10,521 records both before and after normalization, with no orphan records.

## How to Run

1. Download `infectious_cases.csv` using the dataset link above.
2. Open `goit_rdb_fp.ipynb` in Google Colab.
3. Restart the runtime and execute all cells in order, from top to bottom.
4. When prompted, upload `infectious_cases.csv`.
5. Wait for dependency installation, PostgreSQL startup, and query execution to finish.
6. Confirm that all cells complete without errors and save the notebook with its outputs.

The notebook uses `google.colab.files.upload()`, so these instructions target Google Colab. PostgreSQL runs through `pgserver` in the temporary runtime; a separate database server or pgAdmin installation is not required. A full rerun replaces the source table and rebuilds the normalized tables in the `pandemic` schema.

## Interpretation and Limitations

- The top 10 results represent entities, not exclusively countries. Aggregate groups may overlap and should not be summed as independent observations.
- SQL aggregate functions ignore `NULL` values. Missing observations are not replaced with zeros, and averages are calculated over available values.
- Dividing an annual indicator by 12, 4, or 2 produces a period average, not observed monthly, quarterly, or half-year data.
- Fractional indicator values are preserved. Year differences depend on the execution date.

## Validation Status

The row counts reported above come from the notebook's saved outputs. They do not establish that the current version has completed a clean end-to-end run. Before publishing, run all cells in a fresh Colab runtime and save the resulting outputs without errors.
