# AGENTS.md

## Project goal

Build a polished desktop browser prototype for **Bot or Bomb**, a pixel-art human–robot cooperative bomb-defusal game.

Read these files before changing code:

- `docs/PRODUCT_SPEC.md`
- `docs/UI_SPEC.md`
- `docs/ROBOT_PROTOCOL.md`

## Working agreement

- Start each major task by writing a short implementation plan.
- Do not redesign the product without explaining the tradeoff.
- Prefer small, reviewable changes.
- Run formatting, linting, type checks, unit tests, and Playwright tests after relevant changes.
- Report any command or test that could not run.
- Ask before adding a production dependency unless it is explicitly approved below.
- Do not copy code or assets from repositories with unclear licenses.
- Maintain `THIRD_PARTY_NOTICES.md`.
- Do not generate original pixel art unless explicitly asked.
- Use the selected vendor assets consistently.
- Never make the physical robot a requirement for local development; keep `MockRobotAdapter` working.
- Keep the game deterministic in `demo` mode.
- Preserve keyboard fallbacks for gestures.
- Preserve accessibility and reduced-motion behavior.

## Approved production dependencies

- React
- TypeScript
- Vite
- Phaser
- Zustand
- Howler.js

Additional dependencies require approval.

## Design constraints

- 1440×900 target, 1280×720 minimum.
- Desktop only.
- 8 px spacing grid.
- Pixel-art style with hard edges.
- No glassmorphism.
- No large soft shadows.
- No excessive gradients.
- Use one UI asset pack only.
- Use DOM UI for controls and Phaser only for the arena.
- Do not hide essential information behind hover.
- Timer, strikes, robot report, current rule, and command dock must always be visible during gameplay.

## Code structure

Prefer:

```text
src/
  app/
  components/
  game/
  robot/
  store/
  content/
  styles/
  audio/
```

Keep:
- robot adapters isolated under `src/robot/`;
- puzzle JSON under `src/content/`;
- shared design tokens under `src/styles/tokens.css`;
- Phaser scene code under `src/game/`;
- React UI outside Phaser.

## Testing expectations

At minimum:

- unit test mission timer;
- unit test puzzle evaluation;
- unit test strike/failure logic;
- unit test mock robot event flow;
- Playwright test for a complete scripted win;
- Playwright test for timeout failure;
- Playwright test for robot disconnect fallback.

## Completion standard

A task is not complete until:
- code builds;
- relevant tests pass;
- UI is checked at 1440×900 and 1280×720;
- no blurry pixel sprites are visible;
- the demo flow can be completed without a physical robot.
