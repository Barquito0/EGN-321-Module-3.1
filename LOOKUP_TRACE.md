# Lookup Trace — Assignment 3.1

| Step | Spreadsheet Logic | Criterion / Value | Table or Range Used | Python Replacement |
|---|---|---|---|---|
| 1 | Read selected valve family | Valve Family | Selection Input / Engineering Tables | Validate `valve_family` and select `LOOKUP_TABLES[valve_family]` |
| 2 | Read requested operating temperature | Temperature in °C | Selection Input | Use `temperature_c` as second lookup criterion |
| 3 | Approximate row search / nested family branch | Family + temperature | Family-specific rows | Determine supported min/max and refuse values outside them |
| 4 | Exact row or surrounding rows | Requested temperature | Selected family table | Return exact row if present; otherwise locate lower and upper neighboring points |
| 5 | Manual/nested result | Lower/upper row values | Selected rows | Apply `linear_interpolate()` and return a structured result |

## Questions

1. **What is the first lookup criterion?**  
   Valve family.

2. **What is the second lookup criterion?**  
   Operating temperature in degrees Celsius.

3. **Which table applies?**  
   The selected family table from the Engineering Tables dataset.

4. **What happens on an exact row?**  
   Return the listed coefficient directly. Do not interpolate.

5. **What happens between rows?**  
   Find the nearest lower and upper supported rows and use linear interpolation.

6. **What happens outside the supported range?**  
   Raise `ValueError`. Do not extrapolate.

## Why the Spreadsheet Is Hard to Audit

The Legacy Lookup sheet describes a nested `IF/MATCH/INDEX` path whose row
selection depends on the selected family. The upper row is defined as
"lower row + 1", which becomes difficult to reason about at the upper edge,
and the workbook does not clearly separate interpolation from extrapolation.

The Python replacement separates:
- source table data,
- interpolation math,
- family/range validation,
- exact lookup,
- surrounding-row selection.
