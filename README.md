# dsc-livestock-movement

dsc-livestock-movement

## What it does

| Step | What it does |
|---|---|
| **Connect to EarthRanger** |  |
| **Set Time Range** | Choose the period of time to analyze. |
| **Get Events** | Get events. |
| **Process Events Details** | Extracts and processes event details for all events in the DataFrame, resolving enum values from the schema for each event type and mapping keys to titles. |
| **Normalize Json Column** |  |
| **Drop Column Prefix** | Remove a prefix from column names in a DataFrame with duplicate handling. |
| **Decompose Datetime** | Decompose a datetime column into its constituent temporal components. |
| **Map Columns** | Maps and transforms the columns of a DataFrame based on the provided parameters. The order of the operations is as follows: drop columns, retain/reorder columns, and rename columns. |
| **Validate Block Movements** |  |
| **Map Columns** | Maps and transforms the columns of a DataFrame based on the provided parameters. The order of the operations is as follows: drop columns, retain/reorder columns, and rename columns. |
| **Persist Df** | Persist dataframe to a file or cloud storage object. |
| **Draw Table** | Creates an HTML table of the provided dataframe. |
| **Persist Text** | Persist text to a file or cloud storage object. |
| **Create Table Widget Single View** | Create a table widget with a single view. |
| **Merge Widget Views** | Merge widgets with the same `title` and `widget_type`. |
| **Set Workflow Details** | Add information that will help to differentiate this workflow from another. |
| **Set Groupers** |  |
| **Gather Dashboard** |  |

## Requirements

- Access to a **EarthRanger** data source, set up as a connection

---

## 1. Load the Workflow

In the workflow runner, add a new template using this repository's URL. Once added, select
**dsc-livestock-movement** from the available templates list to load it.

---

## 2. Configure the Workflow

You'll be prompted to fill in the following parameters before running.

### Connect to EarthRanger

| Field | Description | Default |
|---|---|---|
| Data source *(required)* | Select one of your configured data sources. | — |

### Set Time Range

| Field | Description | Default |
|---|---|---|
| Since *(required)* | The start time | — |
| Until *(required)* | The end time | — |
| Timezone |  | — |

### Set Workflow Details

| Field | Description | Default |
|---|---|---|
| Workflow Name *(required)* |  | — |
| Workflow Description |  | `` |

---

## Developing

```bash
# recompile spec.yaml (first time: --install; after that: --update, which bumps VERSION.yaml)
./dev/recompile.sh --update

# run the test cases in test-cases.yaml
pixi run --manifest-path pixi.toml -e default bash dev/pytest-cli.sh dsc_livestock_movement --all
```

Documentation: [docs/index.html](docs/index.html).
