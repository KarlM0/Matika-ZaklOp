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
- **Shareable settings.** Any configuration can be turned into a link that
  opens the app with those settings already applied — see
  [Sharing a set of settings](#sharing-a-set-of-settings) below.
- **Target-driven generator** — the answer is picked first and decomposed into
  an expression tree, guaranteeing in-range results, exact division,
  non-negative subtraction, and standard operator precedence.
- Per-equation marking and an overall percentage score, with progress bars for
  completion and accuracy.
- A stopwatch in the Results panel times each set: it starts when the
  equations are generated and stops once the set is checked and every equation
  is correct, showing the elapsed time as `hh:mm:ss` next to the score.
- The Settings panel is collapsible: it folds away automatically once a set of
  equations is generated, and re-opens when a checked set is fully answered and
  entirely correct — keeping the focus on the equations while practising.
- Bilingual interface — Czech (default) and English. Czech uses a comma as the
  decimal separator.
- Dark, minimalist design.

### **Easter eggs**
Optional, on by default. Enable the "Easter eggs" toggle in Settings to reveal 
hidden bonus emoji on correct answers for special results. 
Triggers include:
- Answers of 1, 2, 3 (medals), 4 in Czech (🥔),8 (🎱)
- 9 with expression `3 × 3` or `3 + 3 + 3` in Czech (🎺🐻)
- 12 in Czech (🚶🌾👩😀🍲🍩)
- 13 (🍀🐞), 42 (🌌), 67 (🎇🌈🤪), 69 (💋), 112 (🆘)
- 150/155/156/158 in Czech (🚒/🚑/🚓/🚔)
- 365 (🗓), −123 (🕝🎶)
- Any result whose digits contain three consecutive sixes (😈👹🤘🧛)
- Decimal results that are a left-prefix of π (📐🛞).


## How to use it

The app is a single, self-contained HTML file. There is nothing to install and
no build step.

Just download `Matika-ZaklOp.html` and open it in any web browser. Because it
is one self-contained file, it can be copied to and run from any device —
desktop, laptop, tablet or phone — fully offline.

You can also access the file directly in cloud storage [here](https://filedn.eu/lNSOn6bdI52VD4jLq7uA184/HTMLapps/Matika/Matika-ZaklOp.html).

## Sharing a set of settings

Settings can be configured once and handed over as a link — useful for setting
up a specific drill and sending it to whoever is going to work through it.

### From the app

Open **Settings → Share settings** and press **Copy link**. The link is built
from the values currently in the form and the selected language, shown in a
field below the button, and copied to the clipboard.

The **Link starts practice** toggle controls whether the link generates a set
immediately on opening (`go=1`), so the recipient lands straight on the
equations with Settings folded away. Left off, the link opens the app with the
settings applied and waits.

Once the link field is visible it updates automatically as settings are
changed, so what is shown always matches the form.

### Writing a link by hand

Parameters can also be appended manually. Both the hash fragment and the query
string are accepted; the hash is what the app itself emits and is the more
reliable of the two, since it works identically for local files and hosted
copies.

```
Matika-ZaklOp.html#lang=cs&ops=mul&opmax=10&focusmin=3&focusmax=4&go=1
```

| Parameter | Values |
|---|---|
| `lang` | `cs`, `en` |
| `ops` | comma-separated list of `add`, `sub`, `mul`, `div` |
| `opmin`, `opmax` | integers, −10000 to 10000 |
| `resmin`, `resmax` | integers, −10000 to 10000 |
| `focusmin`, `focusmax` | numbers, or empty to switch the focus term off |
| `count` | `5`, `10`, `15`, `20`, `30` |
| `operands` | `2`–`8` |
| `brackets` | `0`–`3` |
| `dplaces` | `0`, `1`, `2` |
| `autobracket`, `showcorrect`, `eggs` | `1` / `0` |
| `go` | `1` generates a set immediately on opening |

Notes:

- Parameter names are matched case-insensitively and unknown keys are ignored,
  so links written by a different version of the app remain usable.
- Booleans also accept `true`/`false` and `yes`/`no`.
- `ops` switches on exactly the operations it lists and switches off all
  others.
- A partial link overrides only the settings it names; everything else keeps
  the value the recipient already had. Links generated by the app always carry
  the full set, so the same link produces the same configuration for everyone.
- Opening a link makes its settings the recipient's new saved default.

## Tech notes

- Plain HTML, CSS and JavaScript — no frameworks, no bundler.
- The only external resource is Google Fonts.
- Settings are saved between visits via the browser's `localStorage`.
- All settings — stored, supplied by a link, or entered in the form — pass
  through one validation step. Number ranges are clamped to ±10000 and
  dropdown values are snapped to their permitted options, so an out-of-range
  value cannot reach the generator. A corrected value is written back into the
  field so the form always shows what will actually be used.

## Versioning

Current version: **1.3.0**. See [CHANGELOG.md](CHANGELOG.md) for the full
history of changes.

## Known limitations

- `localStorage` does not persist inside sandboxed preview environments, so
  settings may not be remembered there. This is expected — the app works fully
  once the file is downloaded or hosted.
- Share links do not work inside sandboxed preview environments either: the
  page address there is a temporary internal one, so a generated link points
  nowhere useful. Generate links from the downloaded or hosted file.
- Clipboard access can be blocked by the browser in some contexts. When that
  happens the app falls back to an older copy method, and failing that shows
  the link in a selected field with a prompt to copy it manually.
- A `file:///…` link only works on the device holding that file. To share a
  set with someone else, generate the link from the hosted copy.
