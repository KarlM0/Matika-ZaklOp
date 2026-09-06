# Changelog

All notable changes to **Math Practice** are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/), and the
project aims to follow [Semantic Versioning](https://semver.org/).

> **Note:** formal version numbering and this changelog were introduced in
> v1.1.3. Versions 1.1.0–1.1.2 are reconstructed retroactively from a single
> development session; their dates reflect that session rather than separate
> release dates.

## [Unreleased]

### Change
- Easter egg -123 activates only for Czech language.
- Update the Czech title.

### Add
- Add indication to the settings that are optional.

## [1.3.0] - 2026-09-06

### Added
- **Launch parameters.** The app can be opened with settings supplied in the
  URL, so a set can be prepared once and handed over as a link:

  ```
  Matika-ZaklOp.html#lang=cs&ops=mul&opmax=10&focusmin=3&focusmax=4&go=1
  ```

  Both the hash fragment and the query string are accepted and parsed by the
  same code; the hash is read first. Parameter names are matched
  case-insensitively and unknown keys are ignored, so links written by an
  older or newer version stay usable in both directions.

  Values are merged on top of whatever the visitor already has (defaults,
  then `localStorage`), so a partial link overrides only what it names.
  Opening a link makes its settings the visitor's new local default.

  Recognised parameters: `lang`, `ops`, `opmin`, `opmax`, `resmin`, `resmax`,
  `focusmin`, `focusmax`, `count`, `operands`, `brackets`, `dplaces`,
  `autobracket`, `showcorrect`, `eggs`, `go`. Booleans accept
  `1`/`0`/`true`/`false`/`yes`/`no`. `ops` is a comma-separated list drawn
  from `add`, `sub`, `mul`, `div`; the operations it lists are switched on and
  all others off.
- **Focus term in links.** Because the focus bounds are nullable, they are
  emitted as `focusmin=&focusmax=` when the feature is off, and an empty or
  unparseable value reads back as blank. A link that carries both keys
  therefore sets focus deterministically — including switching it off — while
  a link that omits them leaves the recipient's own focus setting untouched.
- **`go=1` parameter.** When present, the app generates a set as soon as the
  link opens, arriving straight at the equations with Settings folded away.
- **Share-link generator.** A "Share settings" block at the bottom of the
  Settings card builds a link from the values currently in the form and the
  selected language, and copies it to the clipboard. The link is emitted in
  hash form. All values are included, not only those differing from the
  defaults, so the same link produces the same set for every recipient
  regardless of their stored settings.

  The link field stays hidden until the button is pressed; once visible it
  refreshes on every settings change, so a displayed link can never fall out
  of step with the form. Pressing the button also commits any value typed but
  not yet blurred, so the link matches what is on screen.

  Copying falls back in three tiers: the asynchronous clipboard API, then
  `document.execCommand`, then a message asking for a manual copy with the
  link left selected in a visible field.
- **"Link starts practice" toggle.** Sits in the share block and controls
  whether the generated link carries `go=1`. It is a property of the link
  being written rather than of the practice itself, so it is never emitted as
  a parameter of its own.

### Changed
- **Single settings validator.** Stored settings, URL parameters and the
  settings form now all pass through one `sanitizeSettings()` function.
  Previously the clamping lived only in the form-reading path, so no other
  entry point was checked at all.
- **Range fields clamped to ±10000.** This is a safety limit rather than a
  pedagogical one: the generator does work proportional to the number range,
  so an unbounded value — a typo, or a mangled link — could freeze the tab.
  The bound applies to the number range, the result range and the focus term
  range.
- **Select-backed values snapped to their option lists.** `count`,
  `operands`, `brackets` and `dplaces` are moved to the nearest permitted
  option instead of being range-checked. Assigning a value that is not in a
  `<select>` leaves the control displaying its previous value while the state
  holds the new one, so the interface would otherwise misreport what is being
  generated.
- **Corrected values written back into the form.** When a range value is
  clamped, or a maximum below its minimum is raised to match, the input now
  shows the corrected figure. Previously the correction happened in state
  only and the field kept displaying the original number.
- **Generator work caps.** The step budget bounds the number of recursive
  calls but not the work inside a single attempt, so the two enumerating
  branches are now capped independently: the factor search for `×` scans at
  most 4000 trial divisors and collects at most 400 pairs, and the divisor
  search for `÷` tries at most 2000 candidates, sampling the range at random
  rather than enumerating it when it is larger than that. With everyday
  ranges these limits are never reached and generation is unchanged; they
  only take effect on very large ranges, trading a little variety for a
  responsive tab. The engine remains target-driven.

## [1.2.0] - 2026-06-22

### Added
- **Focus term.** A second, independent number range that applies to exactly
  one randomly chosen term per equation. All other terms continue to use the
  normal number range. Activated implicitly when both Focus term range fields
  contain valid numbers; leaving either field blank keeps the feature off.
  Typical use cases: practising multiplication by a specific number (e.g.
  focus range `[3, 4]`), or drilling division by a fixed divisor (e.g. `[5, 5]`).
- **GitHub link.** A "View on GitHub" / "Zobrazit na GitHubu" link appears
  below the version string in the header, styled identically to the version
  label. Links to `https://github.com/KarlM0/Matika-ZaklOp/`.
- **New easter eggs** (all require the Easter eggs toggle to be on):
  - Result **−123** → 🕝🎶
  - Result **8** → 🎱
  - Result **69** → 💋
  - Result **112** → 🆘
  - Result **150**, Czech only → 🚒
  - Result **155**, Czech only → 🚑
  - Result **156**, Czech only → 🚓
  - Result **158**, Czech only → 🚔
  - Result **365** → 🗓
  - Result **9** with expression exactly `3 × 3` or `3 + 3 + 3`, Czech only → 🎺🐻
- **Negative result ranges.** The Result range minimum field now accepts
  negative values, making results such as −123 reachable.

### Changed
- **Easter eggs default on.** The Easter eggs toggle now defaults to enabled
  for new users.
- **Decimal places replaces decimals toggle.** The "Include decimals" toggle
  is removed. Decimal places set to `0` means whole numbers only; `1` or `2`
  activates decimal operands and results. The Decimal places field dims at `0`
  to signal it is inactive. Default changed from `2` to `0`.
- **Focus term replaces focus toggle.** The "Focus term" toggle is removed.
  Focus is inferred directly from the range fields: both fields filled =
  active, either blank = inactive. The focus range field dims when inactive.
  Focus range defaults to blank (off).
- **"(optional)" moved into field labels.** The hint previously appeared as a
  separate line below the label; it now appears inline as a smaller, dimmer
  suffix inside the label element, preserving grid alignment.
- **Settings field order** (left → right, top → bottom): Number range, Focus
  term range, Number of terms, Result range, Bracket groups, Decimal places,
  Number of equations; followed by toggle rows: Auto-bracket, Show correct
  answers, Easter eggs.
- **Pi easter egg updated.** Correct answers that are a left-prefix of π now
  show 📐🛞 instead of 📐 alone (U+1F6DE added).
- **Egg #10 first emoji changed** from 🪨 (rock) to 🎺 (trumpet, U+1F3BA).
- **Duplicate-expression prevention** (carried from v1.1.10): the generator
  makes up to four extra attempts per slot to avoid repeating an expression
  already in the current batch.
- **Commutative operand shuffle** (carried from v1.1.10): addition and
  multiplication nodes randomly swap their left and right subtrees (50/50) for
  greater surface variety.

## [1.1.10] - 2026-05-28

### Added
- **Easter eggs toggle.** Settings now include an "Easter eggs" switch (off by
  default). When enabled, correct answers on special results show bonus emoji
  in place of the normal result face. See the v1.1.7 and v1.1.8 entries for
  the full list of triggers.
- **Duplicate-expression prevention.** The generator now tracks all rendered
  expressions in a batch and makes up to four extra attempts per slot to find a
  distinct one. Batches with a very tight number range may still be shorter than
  requested, handled by the existing partial-set warning.

### Changed
- **Commutative operand shuffle.** For addition and multiplication nodes, the
  generator now randomly swaps left and right subtrees (50/50). This increases
  surface variety within structurally constrained settings (e.g. +/− only,
  3 terms, 1 bracket) without affecting mathematical correctness.

## [1.1.9] - 2026-05-26

### Added
- A stopwatch in the Results panel. It starts when a set of equations is
  generated, and stops when that set is checked with the "Check answers"
  button and every generated equation is answered correctly. The elapsed time
  is shown as `hh:mm:ss` next to the score percentage. Generating a new set
  resets it.

## [1.1.8] - 2026-05-25

### Added
- **Gold medal — result 1.** A correct answer of exactly `1` shows 🥇.
- **Silver medal — result 2.** A correct answer of exactly `2` shows 🥈.
- **Bronze medal — result 3.** A correct answer of exactly `3` shows 🥉.
- **Pi.** A correct answer whose rendered value is a left-prefix of π's decimal
  expansion and has a fractional part (`3.1`, `3.14`, …) shows 📐. A bare
  integer `3` does not qualify and triggers the bronze-medal egg instead.
  With the app's two-decimal-place limit only `3.1` and `3.14` are reachable.
- **Potato — result 4, Czech only.** Shows 🥔 when the interface language is
  Czech.
- **Czech lunchtime — result 12, Czech only.** Shows 🚶🌾👩😀🍲🍩 when the
  interface language is Czech.

### Changed
- Easter egg emoji now **replace** the result face of a correct answer instead
  of being appended after it. When several eggs match the same result their
  emoji are concatenated, and that combined string replaces the star-struck
  face.

## [1.1.7] - 2026-05-25

### Added
- **Triple six ("devilish set").** A correct answer whose digits contain three
  consecutive sixes shows 😈👹🤘🧛. The test runs on the rendered result with
  the sign and decimal separator stripped, so only the digit characters must be
  consecutive (e.g. `666`, `66.6`, `−1256.66`).
- **Lucky thirteen ("lucky charms").** A correct answer of exactly `13` shows
  🍀🐞.
- **Answer forty-two ("milky way").** A correct answer of exactly `42` shows 🌌.
- **Sixty-seven ("fireworks set").** A correct answer of exactly `67` shows
  🎇🌈🤪.

  > _Reconstruction note:_ these four Easter eggs were confirmed present in
  > v1.1.7 and absent in v1.1.5, but the exact iteration that introduced each
  > one was not recorded at the time. They are grouped here as the nearest
  > known version.

### Changed
- Pressing "Check answers" on a set that is fully answered and entirely
  correct now re-opens the Settings card, ready for configuring the next set.

## [1.1.6] - 2026-05-25

### Changed
- A successful "Generate equations" now also collapses the Settings card so
  the equations become the focus. A generation that produces nothing — no
  operation selected, or an impossible settings combination — leaves Settings
  open.

## [1.1.5] - 2026-05-23

### Changed
- Renamed the application file from `math-practice.html` to `Matika-ZaklOp.html`.

## [1.1.4] - 2026-05-23

### Changed
- Swapped the result faces: a wrong answer now shows a thinking face, an
  unanswered one an upside-down face.

### Removed
- Removed the "New set" button. A new set is generated with the main
  "Generate equations" button.

## [1.1.3] - 2026-05-23

### Added
- Version information, shown in the app header and recorded in an HTML comment
  and a `VERSION` constant.
- The Results panel is now a collapsible card, collapsed by default.

### Changed
- The worked solution, including the intermediate calculation step, is now
  shown for every completed answer (correct and incorrect), not only for
  incorrect ones.

## [1.1.2] - 2026-05-23

### Added
- Progress bar above the percentage (green / grey) showing completed vs.
  unanswered equations.
- Progress bar below the percentage (green / red) showing correct vs.
  incorrect answers.

### Changed
- Replaced the custom drawn result faces with standard Unicode emoji.
- Auto-bracket no longer disables the "Bracket groups" selector. It now
  enforces bracketing of `×` and `÷` on top of the chosen bracket-group count,
  instead of overriding it.
- Default language switched to Czech.
- The correct answer is no longer revealed for unanswered equations.

### Removed
- Removed the "Instant feedback" option.

## [1.1.1] - 2026-05-23

### Added
- The Settings section is now collapsible.
- Worked solution with an intermediate calculation step for bracketed
  equations (e.g. `2 + (3 × 3) = 2 + (9) = 9`).
- "Instant feedback" option (grade an answer when leaving its box).

### Changed
- Result marks changed from check marks to faces.
- The score percentage is calculated only from answered equations; the number
  of unanswered equations is shown alongside it.
- Increased the height of the answer box.
- Empty answers are no longer counted as incorrect — they are tracked as a
  separate "unanswered" state.

### Fixed
- The answer input box was not styled with the dark theme (it rendered as a
  default light input); it now matches the rest of the interface.

## [1.1.0] - 2026-05-23

### Added
- Initial version: a single-file, dependency-free HTML app for practising
  addition, subtraction, multiplication and division.
- Settings: operation selection, operand range, result range, equation count
  (5–30), terms per equation (2–8), bracket groups (0–3), decimals with 0–2
  decimal places, auto-bracket, and show-correct-answers.
- Target-driven equation generator: the answer is chosen first and decomposed
  into an expression tree, guaranteeing in-range results, exact integer
  division, non-negative subtraction, and standard operator precedence.
- Generate-and-check workflow with per-equation marking and an overall
  percentage score.
- Bilingual interface (English / Czech) with a Czech comma decimal separator.
- Settings persistence via `localStorage`.
- Dark, minimalist visual design.
