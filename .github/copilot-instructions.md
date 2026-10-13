# Copilot Instructions for Space-Quiz

## Build, Test, and Verification

This is a zero-dependency, self-contained client-side web application. There are no build steps, bundlers, package managers, or server runtimes.

- **Running / Previewing**: Open `index.html` directly in a browser or through the integrated Copilot `browser` canvas (`open_canvas` with `file:///.../index.html`).
- **Smoke Testing**: Run browser-level assertions directly in the browser canvas via `evaluate_javascript`. Exercise DOM states (`question-view` vs `results-view`), keyboard events (`ArrowUp`, `ArrowDown`, `A`–`D`, `1`–`4`, `Enter`, `r`), and score calculations.

## Architecture

The entire application lives inside a single file: `index.html`. It contains CSS styles, semantic HTML markup, questions data, and application logic.

- **State Model**: Managed via vanilla JS state variables:
  - `questions`: Array of question objects containing `question`, `options`, zero-based `answer` index, and `explanation`.
  - `currentQuestionIndex`, `score`, and `answered`: Drive view rendering, option disabling, and feedback banners.
- **View Lifecycle**: Single-page state transitions between `#question-view` and `#results-view` toggled via `display: none` / `block`.
- **Feedback & Animations**:
  - Correct answers trigger green glow highlighting (`.correct` + `pulseGreen`).
  - Incorrect answers trigger red error styling on the chosen option (`.wrong` + `shakeRed`) while revealing the correct answer.
  - The results view ranks performance into 4 tiers with themed emoji reactions (`🚀`, `🧑‍🚀`, `🛸`, `🪐`).

## Key Conventions

- **Zero External Dependencies**: Keep the application 100% self-contained in `index.html`. Do not introduce node modules, build configs, CDN scripts, or external font stylesheets unless requested.
- **Theme Support via CSS Variables**: The UI must adapt to dark and light modes via `prefers-color-scheme`. Colors are parameterized as CSS variables in `:root` and overridden under `@media (prefers-color-scheme: light)`. Always use existing variables (`--bg`, `--card-bg`, `--card-border`, `--text`, `--primary`, `--success`, `--error`) when adding or styling elements.
- **Mission-Control Visual Language**: Maintain the space telemetry / mission-control theme (monospaced callsigns, status indicator lights, uppercase section tags like `MISSION #N`, and aeronautical debrief terminology).
- **Keyboard Navigation & Accessibility**:
  - Options act as a radio group (`role="radiogroup"`, `role="radio"`, `aria-checked`, roving `tabindex`).
  - Options must remain fully navigable via arrow keys, direct alphanumeric shortcuts (`A`–`D`, `1`–`4`), and `Enter` progression.
  - Dynamic status banners use `role="status"` and `aria-live="polite"`. Auto-focus `next-btn` after selection and `restart-btn` on completion.
