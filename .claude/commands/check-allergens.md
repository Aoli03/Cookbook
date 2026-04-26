# Allergen Safety Check

The user is allergic to:
- **All nuts** (including but not limited to: peanuts, almonds, cashews, walnuts, pecans, pistachios, hazelnuts, macadamia, pine nuts, brazil nuts, chestnuts)
- **All shellfish** (including but not limited to: shrimp, prawns, crab, lobster, crayfish, scallops, clams, oysters, mussels)

## Task

Given the argument `$ARGUMENTS`:
- If an ingredient, dish name, or recipe file path was provided, evaluate it against the allergy list above.
- If no argument was provided, scan all `.tex` files in `recipes/` and audit every ingredient line against the allergy list.

For each item checked:
1. Flag any ingredient that **is** or **may contain** a listed allergen — include the specific allergen risk.
2. Flag ingredients that are ambiguous (e.g. "mixed nuts", "seafood", "satay sauce", "pad thai", "pesto" which often contains pine nuts).
3. If nothing is flagged, confirm the item appears safe.

Be conservative: if there is any reasonable chance an ingredient contains a listed allergen, flag it.
