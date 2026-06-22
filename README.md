# Math Practice

*Procvičování počítání* — a browser-based arithmetic practice app for children.

It generates sets of addition, subtraction, multiplication and division
equations for a child to solve, then checks the answers and reports a score.

## Features

- Choose which operations to practise: `+`, `−`, `×`, `÷`.
- Configurable number range, result range, number of equations, terms per
  equation, and bracket groups.
- **Focus term** (optional). A second number range that applies to exactly one
  randomly chosen term per equation; all other terms use the normal range.
  Useful for drilling multiplication by a specific number (e.g. `[3, 4]`) or
  division by a fixed divisor (e.g. `[5, 5]`). Activated by filling in both
  Focus term range fields; leaving either blank keeps it off.
- Optional decimal numbers (1 or 2 decimal places). Setting Decimal places to
  `0` uses whole numbers only.
- Negative result ranges supported — set the Result range minimum below zero.
- **Target-driven generator** — the answer is picked first and decomposed into
  an expression tree, guaranteeing in-range results, exact division,
  non-negative subtraction, and standard operator precedence.
- Per-equation marking and an overall percentage score, with progress bars for
  completion and accuracy.
- A stopwatch in the Results panel times each set: it starts when the
  equations are generated and stops once the set is checked and every equation
  is correct, showing the elapsed time as `hh:mm:ss` next to the score.
- **Easter eggs** (optional, on by default). Enable the "Easter eggs" toggle
  in Settings to reveal hidden bonus emoji on correct answers for special
  results. Triggers include: answers of 1, 2, 3 (medals), 4 in Czech (🥔),
  8 (🎱), 9 with expression `3 × 3` or `3 + 3 + 3` in Czech (🎺🐻), 12 in
  Czech (🚶🌾👩😀🍲🍩), 13 (🍀🐞), 42 (🌌), 67 (🎇🌈🤪), 69 (💋), 112 (🆘),
  150/155/156/158 in Czech (🚒/🚑/🚓/🚔), 365 (🗓), −123 (🕝🎶), any result
  whose digits contain three consecutive sixes (😈👹🤘🧛), and decimal results
  that are a left-prefix of π (📐🛞).
- The Settings panel is collapsible: it folds away automatically once a set of
  equations is generated, and re-opens when a checked set is fully answered and
  entirely correct — keeping the focus on the equations while practising.
- Bilingual interface — Czech (default) and English. Czech uses a comma as the
  decimal separator.
- Dark, minimalist design.

## How to use it

The app is a single, self-contained HTML file. There is nothing to install and
no build step.

Just download `Matika-ZaklOp.html` and open it in any web browser. Because it
is one self-contained file, it can be copied to and run from any device —
desktop, laptop, tablet or phone — fully offline.

You can also access the file directly in cloud storage [here](https://filedn.eu/lNSOn6bdI52VD4jLq7uA184/HTMLapps/Matika/Matika-ZaklOp.html).

## Tech notes

- Plain HTML, CSS and JavaScript — no frameworks, no bundler.
- The only external resource is Google Fonts.
- Settings are saved between visits via the browser's `localStorage`.

## Versioning

Current version: **1.2.0**. See [CHANGELOG.md](CHANGELOG.md) for the full
history of changes.

## Known limitation

`localStorage` does not persist inside sandboxed preview environments, so
settings may not be remembered there. This is expected — the app works fully
once the file is downloaded or hosted.
