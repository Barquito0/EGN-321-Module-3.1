# Assignment 3.1 Test Plan

| Test Name | Category | Family | Temperature | Expected Result | Evidence Source | Why It Matters |
|---|---|---|---:|---|---|---|
| exact_vx100 | Exact lookup | VX-100 | 40 | 0.93 | Engineering Tables | Proves exact row returned unchanged |
| exact_vx200 | Exact lookup | VX-200 | 70 | 1.39 | Engineering Tables | Second exact family case |
| rc_vx100_50 | Interpolation | VX-100 | 50 | 0.96 | Reference Cases RC-02 | Verifies surrounding rows 40/60 |
| rc_vx200_65 | Interpolation | VX-200 | 65 | 1.36 | Reference Cases RC-03 | Verifies non-midpoint interpolation |
| rc_vx300_62_5 | Interpolation | VX-300 | 62.5 | 1.63 | Reference Cases RC-05 | Verifies third family |
| lower_boundary | Lower boundary | VX-100 | 20 | 0.88 | Supported Ranges / RC-01 | Minimum is valid evidence |
| upper_boundary | Upper boundary | VX-300 | 125 | 2.12 | Supported Ranges / RC-06 | Maximum is valid evidence |
| below_range | Below range | VX-100 | 5 | ValueError | Challenge C-04 | Prevents extrapolation |
| above_range | Above range | VX-200 | 95 | ValueError | Challenge C-08 | Prevents extrapolation |
| unknown_family | Unknown family | VX-999 | 50 | ValueError | Challenge C-11 | Rejects unsupported table criterion |

Minimum required evidence is exceeded:
- at least 2 exact lookup tests;
- at least 2 interpolation tests;
- lower and upper boundary tests;
- below- and above-range refusal;
- unsupported-family refusal.
