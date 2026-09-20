# Interpolation Planning Worksheet

Worked case: **VX-200 at 65°C**

- Requested x: `65`
- Lower x1: `50`
- Lower y1: `1.27`
- Upper x2: `70`
- Upper y2: `1.39`

Formula:

`y = y1 + ((x - x1) / (x2 - x1)) * (y2 - y1)`

## Show Your Work

Fraction between rows:

`(65 - 50) / (70 - 50) = 15 / 20 = 0.75`

Change in y:

`1.39 - 1.27 = 0.12`

Interpolated change:

`0.75 * 0.12 = 0.09`

Interpolated y:

`1.27 + 0.09 = 1.36`

- Is x inside the supported range? **Yes**
- Supported VX-200 range: **10°C to 90°C**
- Result: **1.36**

If x were outside 10°C–90°C, the selection tool should raise `ValueError`
instead of applying the interpolation line beyond the available evidence.
