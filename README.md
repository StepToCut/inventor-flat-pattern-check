# Inventor flat pattern check

An iLogic rule that tells you when Inventor's unfold skipped a bend.

On a STEP part converted to sheet metal, `Unfold` and `Unfold2` can leave a bend folded and still return without an error. If every bend unfolded, the flat body is exactly one thickness high. A bend left folded sticks out of it. This rule measures that.

## Use

1. Add `FlatPatternCheck.iLogicVb` as a rule (or external rule).
2. Run it on a sheet metal part after the unfold.
3. "OK" or "The flat is N mm higher than the thickness".

To stop an export instead of showing a message, call `FlatExtraHeightMm` from your own rule and skip the export when it returns more than 0.01 or NaN.

## Tested

Inventor 2025.2, October 2026, on a U-shaped test plate (2 mm, two bends R2) exported to STEP and re-imported:

| Test part | Result |
|---|---|
| Clean, thickness set to 2.000 | OK |
| Thickness set to 1.981 on a 1.9812 part | 38.02 mm too high |
| One outer radius 0.001 mm too large | 38.00 mm too high |
| One bend face stored as a B-spline | 38.00 mm too high |
| Both bend faces stored as B-splines | 38.00 mm too high |
| Holes in the flanges | OK |
| Hole in the bend | OK |
| Thickness set to 1.9812 (exact) on the 1.9812 part | OK |

Full output of the last run: `test_results.txt`.

## Limits

- It does not check the flat length: a wrong K-factor or unfold rule passes.
- Formed features (louvers, embosses, extruded holes) stay in the flat and trigger it. Not tested.
- First solid body only.

Why bends get skipped, with the measurements: [Inventor skips a bend when unfolding a STEP part](https://steptocut.com/blog/inventor-unfold-skips-bend-step/).

Written with Claude (AI), tested in Inventor by [StepToCut](https://steptocut.com).

## License

MIT
