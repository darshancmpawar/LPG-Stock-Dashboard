# LPG Stock Tacker Dashboard

A Python Dash dashboard for tracking LPG stock risk with a professional dark UI.

## Features

* Selected dashboard date driven logic
* Weekend exclusion from stock decay
* Filters out vendors where `gail/png = yes`
* Unique vendor KPIs
* Client KPI using worst vendor risk
* Region cards
* Executive city view
* Donut risk split
* Click-to-expand pivot table by risk category

## Project structure

```text
project/
├─ app.py
├─ data_loader.py
├─ aggregations.py
├─ components.py
├─ requirements.txt
├─ README.md
├─ data/
│  └─ lpg_stock_data.xlsx
└─ assets/
   └─ styles.css
```

## Install

Create a virtual environment and install dependencies.

```bash
pip install -r requirements.txt
```

## Dataset expectations

The loader tries to map common column name variations automatically.

### Required logical fields

* vendor
* client
* region
* pax
* days_of_stock
* last_updated
* gail_png

### Optional logical field

* continuity

## Important business rules

### 1. Gail/PNG filter

Rows where `gail/png = yes` are excluded during load.

### 2. Live LPG stock days

Live LPG days are calculated as:

`Live Days = Days of Stock - Working Days Between(Last Updated, Selected Date)`

### 3. Weekend exclusion

Saturday and Sunday are not counted in stock decay.

### 4. Risk category

* Out of Stock = 0
* Critical = 1 to 2
* Moderate = 3 to 4
* Safe = 5+

### 5. KPI logic

* Vendor KPI uses unique vendors and worst risk per vendor
* Client KPI uses unique clients and worst vendor risk per client

## Default dataset path

The app currently expects the file here:

```text
data/lpg_stock_data.xlsx
```

If your file name is different, update this in `data_loader.py`:

```python
DEFAULT_DATA_PATH = Path("data/lpg_stock_data.xlsx")
```

## Run the dashboard

```bash
python app.py
```

Then open the local URL shown in the terminal.

## File responsibilities

### `app.py`

Main Dash app, layout, and callbacks.

### `data_loader.py`

Loads raw data, standardizes columns, cleans data, and filters `gail/png = yes`.

### `aggregations.py`

Creates enriched rows and all summaries for KPIs, regions, city executive view, and pivot groups.

### `components.py`

Contains reusable UI blocks and table builders.

### `assets/styles.css`

Controls the professional dark theme and layout styling.

## Next recommended improvement

Once your real dataset is tested, the next file to add should be:

* `stock_logic.py` to isolate date/risk logic even further
* or `config.py` for dataset paths and app constants
