# dsc-livestock-movement

Checks livestock block movements recorded in EarthRanger. For each Community/Zone, every movement's **FROM** blocks
should be the blocks the previous movement went **TO**. The workflow lists every movement with a status, so missed,
empty or duplicate entries can be found and fixed in EarthRanger after each survey.

## What it does

| Step | What it does |
|---|---|
| **Set Workflow Details** | Name and description shown on the dashboard. |
| **Set Groupers** | No grouping: one table for all communities. |
| **Set Time Range** | The period whose movements are checked. |
| **Connect to EarthRanger** | The EarthRanger site to read from. |
| **Get Events** | Reads the `livestock_block_movement` events in the time range. |
| **Process Events Details** | Turns the event fields into columns named by their titles (Community/Zone, FROM, TO). |
| **Normalize Json Column** | Splits the event details into one column per field. |
| **Drop Column Prefix** | Removes the `event_details__` prefix from those columns. |
| **Decompose Datetime** | Adds the event's date. |
| **Map Columns** | Renames `time_date` to `date`. |
| **Validate Block Movements** | Compares each movement with the one before it in the same Community/Zone and gives it a status. |
| **Map Columns** | Keeps and renames the columns for the report. |
| **Persist Dataframe** | Saves the report as `livestock_movement_validation.csv`. |
| **Draw Table** | Draws the report as a table. |
| **Persist Text** | Saves the table for the dashboard. |
| **Create Table Widget Single View** | The *Livestock Movement Validation* table widget. |
| **Merge Widget Views** | Combines the widget's views. |
| **Gather Dashboard** | Builds the dashboard. |

### Statuses

Movements are sorted by time within each Community/Zone. A movement can have more than one status, joined with `;`.

| Status | Meaning | An issue? |
|---|---|---|
| Valid movement | FROM matches the previous movement's TO | No |
| First record | The first movement for that Community/Zone in the time range, with nothing to compare with | No |
| Missed movement: expected FROM [...] (#serial, date) | FROM doesn't match the previous movement's TO, which is shown with its serial number and date | Yes |
| No movement | FROM and TO are the same blocks | Yes |
| Empty FROM or TO | One side has no blocks | Yes |
| Duplicate entry | Same Community/Zone, date, FROM and TO as another movement | Yes |

The report's columns: Serial Number, Previous Serial Number, Date, Previous Date, From, To, Community/Zone, Status
and `is_issue` (filter on it to see only the problems).

## Requirements

- Access to an **EarthRanger** data source, set up as a connection, with a `livestock_block_movement` event type
  whose fields are titled **Community/Zone**, **FROM** and **TO**

---

## 1. Load the Workflow

In the workflow runner, add a new template using this repository's URL. Once added, select
**dsc-livestock-movement** from the available templates list to load it.

---

## 2. Configure the Workflow

The run form asks for these, in this order.

### Set Workflow Details

| Field | Description | Default |
|---|---|---|
| Workflow Name *(required)* | Shown on the dashboard. | — |
| Workflow Description | Shown on the dashboard. | `` |

### Set Time Range

| Field | Description | Default |
|---|---|---|
| Since *(required)* | The start time | — |
| Until *(required)* | The end time | — |
| Timezone | | — |

### Connect to EarthRanger

| Field | Description | Default |
|---|---|---|
| Data source *(required)* | Select one of your configured data sources. | — |

---

## 3. Fix what it finds

Open each flagged movement in EarthRanger by its serial number. For a *Missed movement*, also look at the movement
named in the status: often one of the two has a wrong date (a year typo moves it out of order), or a movement
between them was never entered.

---

## Developing

```bash
# recompile spec.yaml (first time: --install; after that: --update, which bumps VERSION.yaml)
./dev/recompile.sh --update

# run the test cases in test-cases.yaml
pixi run --manifest-path pixi.toml -e default bash dev/pytest-cli.sh dsc_livestock_movement --all
```

The validation task is `validate_block_movements` in `ecoscope-workflows-ext-distance-sample-counts` (1.1.5, from
`https://repo.prefix.dev/ecoscope-workflows-custom/`).

Documentation: [docs/index.html](docs/index.html).
