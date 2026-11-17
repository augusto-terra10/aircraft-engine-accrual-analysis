# DAX measures and calculations

This document records example of measures to the dashboard. Confirm the exact measure names and definitions in the included PBIX before treating these examples as a complete export of the report's DAX.

## Actual accrual

```dax
Actual Accrual =
SUM('Engine Maintenance Data'[Actual_Accrual_PASV_LLP])
```

## Forecast accrual

```dax
Forecast Accrual =
SUM('Engine Maintenance Data'[Forecast_Accrual_PASV_LLP])
```

## Actual block hours

```dax
Actual Block Hours =
SUM('Engine Maintenance Data'[Block_Hours_Actual])
```

## Accrual cost per block hour

```dax
Accrual Cost per Block Hour =
DIVIDE(
    [Actual Accrual],
    [Actual Block Hours]
)
```

## Accrual variance percentage

```dax
Accrual Variance % =
DIVIDE(
    [Actual Accrual] - [Forecast Accrual],
    [Forecast Accrual]
)
```

## Dynamic reporting period

The following pattern displays the first and last selected months and the number of selected months. It assumes a date dimension named `Dim_Date` with a `MonthStart` column.

```dax
Reporting Period =
VAR SelectedMonths =
    CALCULATETABLE(
        VALUES(Dim_Date[MonthStart]),
        ALLSELECTED(Dim_Date)
    )
VAR FirstMonth =
    MINX(SelectedMonths, Dim_Date[MonthStart])
VAR LastMonth =
    MAXX(SelectedMonths, Dim_Date[MonthStart])
VAR MonthCount =
    COUNTROWS(SelectedMonths)
RETURN
    IF(
        MonthCount = 0,
        "No period selected",
        FORMAT(FirstMonth, "MMM yyyy")
            & " - "
            & FORMAT(LastMonth, "MMM yyyy")
            & " | "
            & MonthCount
            & IF(MonthCount = 1, " month selected", " months selected")
    )
```

## Notes

- `DIVIDE` handles zero or blank denominators safely.
- The reporting-period pattern shows the earliest and latest selected months; it does not enumerate non-consecutive selections.
- The engine ranking measure should be validated under aircraft and date slicers. If multiple engines share the same cost value, use a deterministic secondary sort key such as ESN.
- These are representative examples, not a claim that every measure in the PBIX is listed here.
