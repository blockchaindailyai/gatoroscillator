# BCD Terminal — Gator Oscillator

A Pine Script v6 port of BCD Terminal's **two-sided Gator Oscillator**, displayed in a separate pane. This is not the three-line Williams Alligator price-chart overlay.

## Install in TradingView

1. Open a chart and its **Pine Editor**.
2. Create a new indicator and replace the entire template with [`BCD_Terminal_Gator_Oscillator.pine`](BCD_Terminal_Gator_Oscillator.pine).
3. **Save** and **Add to chart**.
4. Keep lengths **13 / 8 / 5** for the terminal defaults. Match the terminal's symbol, exchange, timeframe, candle type, and any custom lengths.

See [`START_HERE.txt`](START_HERE.txt) for detailed usage and alignment notes.

## Calculation and colors

- Source: `(high + low) / 2` (`HL2`).
- Jaw, teeth, and lips: SMA-seeded SMMA/RMA, with default lengths 13, 8, and 5.
- Recurrence after the seed: `(previous * (length - 1) + source) / length`.
- Upper histogram: `abs(jaw - teeth)`.
- Lower histogram: `-abs(teeth - lips)`.
- Each side is green when its magnitude grows **or stays equal**, and red when it shrinks. The first valid bar after warm-up is red when no previous value exists.

The terminal oscillator uses **unshifted averages** and raw price differences: no Alligator +8/+5/+3 displacements, ATR normalization, or percentage normalization. Positive and negative bars identify the two series, not bullish and bearish direction.

## Live bars and optional alerts

The open candle's values and colors can change with its high and low. There is no future-data request, lookahead, or backward plotting.

The optional “both bars turned green/red” alert conditions require a closed candle and valid prior values. To use one, create an alert in TradingView and select **Once Per Bar Close**. Recreate existing alerts after changing settings. These are spread-color transitions, **not buy/sell signals**.

## Validation status

Numerical port-model checks passed against the terminal TypeScript source: 7 fixtures, 6 length configurations, 25,200 compared bars, and 204,486 assertions, with zero measured absolute error. Coverage included rising, falling, flat, step, oscillating, tiny-price, and missing-value data; warm-up; equal-bar coloring; custom lengths; and prefix invariance.

**The published Pine script compiled and rendered in TradingView on 25 October 2026 using the default lengths 13 / 8 / 5, with no compile or runtime error observed.** This confirms compilation and chart rendering for that check, not full cross-feed or live-data parity. The numerical checks above validate a JavaScript model of the port and static Pine contracts; they are not exhaustive Pine runtime tests. The validation source snapshots and harness are intentionally not included in this standalone repository.

Different market feeds, available history, missing bars, settings, and floating-point arithmetic can still produce cross-platform differences. Color comparisons use the sign of the change to avoid Pine's nine-decimal comparison rounding treating small shrinking spreads as unchanged.

## Provenance

Ported from `blockchaindailyai/BCDTerminal`, commit `75042ebea619f2daee2c640b1e3d12ef09aadad2`, checked 25 October 2026:

- `app/components/chart-indicators.ts` — `rma`, `alligator`, `gator`.
- `app/components/chart-study-config.ts` — defaults, colors, and displacement.

Only this Pine script and its installation notes are included here. No upstream source snapshot, trading ledger, credential, or third-party license grant is included.
