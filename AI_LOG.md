# AI Usage Log — Module 3 Assignment 3.1

## Interaction 1
- **Tool:** ChatGPT
- **Date:** September 20, 2026
- **Prompt:** Review the Assignment 3.1 instructions, detailed guide, student package, inherited workbook, lookup CSV, and challenge cases. Identify the lookup criteria, supported ranges, reference cases, required behavior, and required tests.
- **AI output:** A workbook trace identifying valve family as criterion 1, operating temperature as criterion 2, the three family ranges, exact-row behavior, interpolation behavior, and required refusal outside the table.
- **What I used:** The lookup trace structure, readable data/logic separation, interpolation plan, range validation plan, and test categories.
- **What I changed:** The final implementation loads the instructor-provided CSV rather than copying table values into nested conditionals.
- **Why I changed it:** Keeping source data separate from lookup logic makes the engineering evidence easier to inspect and maintain.
- **How I verified it:** I compared the loaded values with the workbook Engineering Tables, Supported Ranges, Reference Cases, and challenge CSV.
- **Which test proves the behavior:** The exact, reference-case interpolation, boundary, and refusal tests in `tests/test_selection_tool.py`.

## Interaction 2 — Adversarial Extrapolation Review
- **Tool:** ChatGPT
- **Date:** September 20, 2026
- **Prompt:** Review the lookup implementation as an engineering tester. Identify any path that could return a numeric coefficient for an unsupported family or a temperature outside the documented table range. Produce tests before suggesting code changes.
- **AI output:** Suggested explicit below-range, above-range, unknown-family, and boundary tests.
- **What I used:** The refusal and boundary test cases.
- **What I changed:** Range checks are performed before exact lookup or interpolation, so the interpolation helper is never used to justify an unsupported engineering value.
- **Why I changed it:** The assignment requires interpolation only inside the evidence and refusal outside it.
- **How I verified it:** I tested VX-100 below range, VX-200 above range, VX-300 on both sides, and VX-999.
- **Which test proves the behavior:** `test_below_range_refused`, `test_above_range_refused`, `test_vx300_below_range_refused`, `test_vx300_above_range_refused`, and `test_unknown_family_refused`.

## Required Reflection

**Did the AI attempt to extrapolate outside the table? If so, how did you correct it?**

The final implementation does not allow extrapolation. The interpolation
formula can mathematically produce values outside two known points, so the
selection layer checks the family-specific minimum and maximum temperature
before interpolation. Unsupported temperatures raise `ValueError` instead of
returning a numeric coefficient.
