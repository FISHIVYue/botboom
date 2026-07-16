# Codex starter prompt — Bot or Bomb

You are working inside a new or existing repository.

First, read:

- `AGENTS.md`
- `docs/PRODUCT_SPEC.md`
- `docs/UI_SPEC.md`
- `docs/ROBOT_PROTOCOL.md`

Your task is to build the frontend MVP for **Bot or Bomb**.

## Important operating rules

1. Do not implement everything in one uncontrolled pass.
2. Inspect the repository first.
3. Write `PLAN.md` with phases, assumptions, risks, and acceptance criteria.
4. Show me the plan before making large architectural changes.
5. Use only the approved dependencies from `AGENTS.md`.
6. Do not download or copy assets with unclear licenses.
7. If the Kenney and Fusion Pixel Font assets are not already present, create clear placeholder files and an `ASSET_SETUP.md` explaining exactly where I should place the downloaded files.
8. Do not generate artwork.
9. Keep mock robot mode fully functional.
10. Use deterministic demo data.

## Implementation phases

### Phase 1 — Scaffold and design system

- Create or validate a React + TypeScript + Vite app.
- Add Phaser, Zustand, and Howler.js if missing.
- Create the folder structure defined in `AGENTS.md`.
- Implement design tokens.
- Implement reusable pixel panels, buttons, status labels, modal, timer, strike display, and command button.
- Build `/ui-kit` so every component and state can be reviewed.
- Use placeholders if licensed external assets are not present.
- Ensure pixel rendering is crisp.

Stop and summarize the result.

### Phase 2 — Static screens

Implement:

- boot screen;
- main menu;
- tutorial/calibration;
- mission briefing;
- main gameplay layout;
- success result;
- failure result;
- hidden demo console.

Use mock data only.

Stop and summarize the result with screenshots if available.

### Phase 3 — Game state and puzzle logic

- Implement the top-level mission state machine.
- Add timer and strikes.
- Add three module states.
- Add JSON puzzle definitions.
- Add a deterministic three-module demo mission.
- Implement low-confidence robot question modal.
- Implement success and failure transitions.
- Add keyboard commands:
  - H = scan
  - Left Arrow = move left
  - Right Arrow = move right
  - Enter = confirm
  - Escape = abort
  - D = demo console

Stop and run tests.

### Phase 4 — Phaser arena

- Add a compact pixel arena scene.
- Render three module stations.
- Render a robot avatar.
- Synchronize robot position with Zustand state.
- Add scan, success, failure, and celebration animations.
- Keep all HUD and controls in React DOM.

Stop and verify no sprites are blurry at 1440×900 and 1280×720.

### Phase 5 — Robot integration

- Implement `RobotAdapter`.
- Implement `MockRobotAdapter`.
- Implement `WebSocketRobotAdapter`.
- Validate incoming JSON.
- Add connection status, reconnect banner, and simulation fallback.
- Use `VITE_ROBOT_WS_URL`.
- Add demo-console event injection.

Stop and test the complete mock flow.

### Phase 6 — Polish and test

- Integrate local vendor assets if present.
- Integrate sound.
- Add reduced-motion behavior.
- Add loading and error states.
- Add Playwright tests for:
  - scripted win;
  - timeout failure;
  - robot disconnect fallback.
- Create `README.md` with setup, controls, asset installation, and robot protocol.
- Create or update `THIRD_PARTY_NOTICES.md`.

## Acceptance criteria

The build is accepted only if:

- `npm run build` succeeds;
- lint and type checks pass;
- tests pass;
- a user can finish the demo mission in under three minutes;
- the game is fully playable with mock robot and keyboard;
- the real robot can connect through the documented WebSocket protocol;
- the UI visibly separates robot knowledge from human manual knowledge;
- a robot uncertainty decision appears during the demo;
- all essential gameplay information remains visible;
- the visual style is consistent and pixel-sharp.

Begin by inspecting the repository and writing `PLAN.md`. Do not start with a full rewrite before reporting the plan.
