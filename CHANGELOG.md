# Changelog

All notable changes to **Math Practice** are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/), and the
project aims to follow [Semantic Versioning](https://semver.org/).

> **Note:** formal version numbering and this changelog were introduced in
> v1.1.3. Versions 1.1.0–1.1.2 are reconstructed retroactively from a single
> development session; their dates reflect that session rather than separate
> release dates.

## [1.1.9] - 2026-05-26

### Added
- A stopwatch in the Results panel. It starts when a set of equations is
  generated, and stops when that set is checked with the "Check answers"
  button and every generated equation is answered correctly. The elapsed time
  is shown as `hh:mm:ss` next to the score percentage. Generating a new set
  resets it.

## [1.1.8] - 2026-05-25

### Changed
- Minor UI changes

## [1.1.7] - 2026-05-25

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
