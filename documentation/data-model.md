# Data model and workbook structure

## Workbook contents

The supplied Excel workbook contains four worksheets:

| Worksheet | Rows (excluding header) | Columns | Purpose |
|---|---:|---:|---|
| `Engine Maintenance Data` | 9,075 | 30 | Monthly engine-level source data |
| `Dim_Aircraft` | 10 | 7 | Aircraft dimension |
| `Dim_Engine` | 121 | 5 | Engine dimension |
| `Dim_Date` | 75 | 11 | Date and financial-period dimension |

## Key fields

### Engine Maintenance Data

The source data includes fields for:
- `MonthStart`, `DateKeyColumns`, `ActualMonthKey`, `FinancialYear`
- `ESN`, `Aircraft`
- Forecast accrual and forecast utilisation fields
- Actual accrual, actual flight hours, actual flight cycles and actual block hours
- Supporting assumptions and account/cost-centre attributes

### Dim_Aircraft

Includes `AircraftKey`, `Aircraft`, `AircraftModel`, `FleetSequence`, `AccountID`, `Cost_Centre_Key` and `Analysis_code`.

### Dim_Engine

Includes `EngineKey`, `ESN`, `AircraftKey`, `Aircraft` and `EngineSequence`.

### Dim_Date

Includes `DateKey`, `MonthStart`, `MonthEnd`, calendar attributes, `YearMonth`, `FinancialYear`, `FinancialPeriod` and `ActualMonthKey`.

## Modelling notes

The workbook contains a monthly engine-level source table plus aircraft, engine and date dimension worksheets. The exact relationships and any Power Query transformations should be checked in the included PBIX file before describing them as implemented details.

Potential matching fields visible in the workbook include:
- Engine: `ESN`
- Aircraft: `Aircraft`
- Month: `MonthStart` or the corresponding date key fields

If implementing or reviewing the model, validate relationship cardinality, uniqueness on dimension keys, filter direction and whether the fact table has been transformed to use surrogate keys.

## Synthetic-data disclaimer

All aircraft, engine, operational and financial records are synthetic. The fleet composition and engine counts are modelling assumptions and must not be interpreted as real-world aircraft configurations.
