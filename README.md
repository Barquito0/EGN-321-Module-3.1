# EGN 321 — Module 3 Assignment 3.1
# Valve Lookup and Interpolation Tool

## Purpose

This project replaces the inherited spreadsheet lookup logic in
`VALVE_SELECTION_rev3.xlsx` with a readable and testable Python tool.

The tool uses two criteria:

1. valve family;
2. operating temperature in °C.

It returns a listed coefficient for an exact table row, uses linear
interpolation for temperatures between supported rows, and refuses requests
outside the documented engineering-data range.

**Engineering rule: Interpolate inside the evidence. Refuse outside it.**

## Source Data

The authoritative assignment data comes from the `Engineering Tables` sheet
in `VALVE_SELECTION_rev3.xlsx`.

The same data is provided in machine-readable form as:

```text
data/valve_lookup_table.csv
```

The Python table layer loads that CSV and groups the rows by valve family.

## Supported Valve Families

| Family | Minimum Temperature | Maximum Temperature |
|---|---:|---:|
| VX-100 | 20°C | 100°C |
| VX-200 | 10°C | 90°C |
| VX-300 | 25°C | 125°C |

The minimum and maximum values themselves are valid table rows.

## Lookup Behavior

### 1. Select the Family

`select_coefficient()` first checks whether the requested valve family exists.

An unsupported family is refused:

```text
Unsupported valve_family: VX-999
```

### 2. Enforce the Supported Range

The selected family's first and last temperature rows define the supported
temperature range.

A request below or above that range raises `ValueError`.

### 3. Exact Lookup

If the requested temperature exactly matches a table row, the listed
coefficient is returned directly with:

```text
method = "exact"
```

No interpolation is performed.

### 4. Surrounding-Row Search

For an in-range temperature that is not an exact row, the program finds the
neighboring lower and upper table points.

Example for VX-200 at 65°C:

```text
lower_point = (50, 1.27)
upper_point = (70, 1.39)
```

### 5. Linear Interpolation

The helper uses:

```text
y = y1 + ((x - x1) / (x2 - x1)) * (y2 - y1)
```

where:

- `x` = requested temperature;
- `x1` = lower known temperature;
- `x2` = upper known temperature;
- `y1` = coefficient at the lower temperature;
- `y2` = coefficient at the upper temperature.

For VX-200 at 65°C:

```text
1.27 + ((65 - 50) / (70 - 50)) * (1.39 - 1.27)
= 1.36
```

## Refusal Behavior

### Below Range

```python
select_coefficient("VX-100", 5)
```

raises a `ValueError` stating that the request is below the supported minimum.

### Above Range

```python
select_coefficient("VX-200", 95)
```

raises a `ValueError` stating that the request exceeds the supported maximum.

### Unsupported Family

```python
select_coefficient("VX-999", 50)
```

raises:

```text
Unsupported valve_family: VX-999
```

The tool never silently substitutes another family and never extrapolates
outside the engineering evidence.

## Structured Results

A successful lookup returns information explaining how the coefficient was
obtained.

## Independent Verification

The workbook's Reference Cases are used as independent expected values.

Examples:

| Case | Family | Temperature | Expected |
|---|---|---:|---:|
| RC-01 | VX-100 | 20°C | 0.88 |
| RC-02 | VX-100 | 50°C | 0.96 |
| RC-03 | VX-200 | 65°C | 1.36 |
| RC-04 | VX-200 | 90°C | 1.54 |
| RC-05 | VX-300 | 62.5°C | 1.63 |
| RC-06 | VX-300 | 125°C | 2.12 |

The function under test is not used to generate these expected values.

## Testing

The suite contains tests for exact lookup, interpolation, surrounding rows,
both boundaries, range refusal, unsupported-family refusal, and the
interpolation helper independently.

Run:

```bash
pytest
```

## Project Structure

```text
.
├── README.md
├── AI_LOG.md
├── LOOKUP_TRACE.md
├── INTERPOLATION_WORKSHEET.md
├── TEST_PLAN.md
├── VALVE_SELECTION_rev3.xlsx
├── requirements.txt
├── pytest.ini
├── data/
│   ├── valve_lookup_table.csv
│   └── lookup_challenge_cases.csv
├── src/
│   ├── __init__.py
│   ├── lookup_tables.py
│   ├── interpolation.py
│   └── selection_tool.py
└── tests/
    ├── test_interpolation.py
    └── test_selection_tool.py
```

## Assumptions

- Valve family names must match the assigned table names exactly.
- Operating temperature is supplied in degrees Celsius.
- Lookup data is sorted by temperature after loading.
- Linear interpolation is appropriate only between adjacent supported rows.
- Table boundaries are valid exact lookup values.

## Known Limitations

- Only VX-100, VX-200, and VX-300 are supported by the assigned dataset.
- No extrapolation is allowed.
- The tool does not infer or convert temperature units.
- The tool does not invent coefficients for unsupported families.
- The interpolation helper performs mathematics only; engineering range
  validation is handled by the selection layer.

## AI Use

Generative AI was used to help analyze the workbook, organize the Python
implementation, create adversarial refusal tests, and review documentation.
Details are recorded in `AI_LOG.md`.
