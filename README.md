# Bot or Bomb — Codex Collaboration Pack

This package contains a complete product and frontend brief for a hackathon-ready human–robot cooperative game.

Files:

- `AGENTS.md` — durable instructions for Codex.
- `CODEX_START_PROMPT.md` — paste this into Codex to start the implementation.
- `docs/PRODUCT_SPEC.md` — game mechanics, screens, states, and architecture.
- `docs/UI_SPEC.md` — detailed UI implementation guidance.
- `docs/ROBOT_PROTOCOL.md` — WebSocket contract between frontend and robot.
- `docs/ASSET_MANIFEST.md` — approved open-source assets and licenses.

Recommended workflow:

1. Create a Git repository.
2. Copy this package into the repository root.
3. Download the approved assets manually.
4. Put them into the documented `public/assets/vendor/` folders.
5. Open the repository in Codex.
6. Paste `CODEX_START_PROMPT.md`.
7. Review `PLAN.md` before letting Codex implement all phases.
8. Work phase by phase, checking the browser after each phase.

Do not ask Codex to “find and copy most of a game from GitHub.” Instead:
- use maintained npm packages for infrastructure;
- use explicitly licensed asset packs;
- let Codex connect and adapt the pieces;
- keep your team responsible for the game rules, visual selection, and final design judgment.
