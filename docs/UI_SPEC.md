# Bot or Bomb — UI Implementation Specification

## Target

Build a desktop browser game UI at 1440×900 with a minimum usable size of 1280×720.

The interface should look like a polished 16-bit cooperative arcade game, not a SaaS dashboard.

---

## Component inventory

### Layout
- `GameShell`
- `TopStatusBar`
- `MainGameGrid`
- `PixelPanel`
- `PixelModal`
- `CommandDock`
- `ConnectionBanner`

### Menu
- `BootScreen`
- `MainMenu`
- `CalibrationScreen`
- `MissionBriefing`

### Gameplay
- `ArenaCanvas`
- `ModuleStation`
- `RobotAvatar`
- `RobotReportCard`
- `ManualRuleCard`
- `ConfidenceMeter`
- `CommandHistory`
- `GestureFeedback`
- `StrikeDisplay`
- `MissionTimer`
- `RobotQuestionModal`

### Results
- `MissionSuccess`
- `MissionFailure`
- `MissionStats`

### Demo
- `DemoConsole`
- `SimulationBadge`

---

## Main gameplay wireframe

```text
┌──────────────────────────────────────────────────────────────────────────────┐
│ BOT OR BOMB   ● ROBOT LINKED     MISSION 03      01:24       X X _   🔊 Ⅱ  │
├───────────────────────────────────────────────┬──────────────────────────────┤
│                                               │ DEFUSAL MANUAL               │
│                                               │                              │
│              PIXEL ARENA                      │ ROBOT REPORT                 │
│                                               │ Color: BLUE                  │
│       [A]              [B]              [C]   │ Symbol: TRIANGLE             │
│        ✓              scanning           ?    │ Confidence: 58%              │
│                                               │                              │
│                   [ROBOT]                     │ CURRENT RULE                 │
│                                               │ If confidence < 65%,         │
│                                               │ request another scan.        │
│                                               │                              │
│                                               │ HISTORY                      │
│                                               │ > moved to B                 │
│                                               │ > scan started               │
├───────────────────────────────────────────────┴──────────────────────────────┤
│ [SCAN / H]     [LEFT / ←]    [RIGHT / →]    [CONFIRM / OK]    [ABORT / X] │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## Responsive behavior

Below 1280 px:
- reduce outer margin;
- keep game and manual side by side;
- reduce decorative labels;
- never hide timer, strikes, robot report, or command dock.

Below 1024 px:
- show unsupported resolution message.
- Mobile is out of scope.

---

## CSS rules

- Use CSS variables for palette, spacing, borders, and animation speed.
- Use `image-rendering: pixelated` on all sprites.
- Avoid `border-radius` above 4 px.
- Do not use blur or glassmorphism.
- Do not use soft box shadows.
- Use 2 px or 4 px hard borders.
- Use 4 px hard drop shadows for raised controls.
- Use `steps()` timing functions for pixel-style animation where appropriate.
- Use a central `tokens.css`.
- Ensure all component states are visible in Storybook-like dev routes or a component gallery page.

---

## Required routes

- `/` → Main menu
- `/tutorial`
- `/mission/demo`
- `/results/success`
- `/results/failure`
- `/ui-kit` → internal component gallery
- `/debug` → demo console

Use React Router only if it already exists or if it improves implementation. A simple state router is acceptable for the MVP.

---

## Copy tone

The UI voice is concise, slightly playful, and operational.

Good:
- `ROBOT LINK ESTABLISHED`
- `SCAN UNCERTAIN`
- `TRUST THE BOT?`
- `MODULE DISARMED`
- `THAT WAS CLOSE`

Avoid:
- long paragraphs;
- military realism;
- graphic danger language;
- generic AI assistant phrasing.

---

## Pixel asset usage

Preferred external assets:

1. Kenney Pixel UI Pack — CC0
   - panels;
   - buttons;
   - cursors;
   - bars;
   - 9-slice frames.

2. Kenney Interface Sounds — CC0
   - click;
   - alert;
   - confirm;
   - error.

3. Fusion Pixel Font — SIL OFL
   - Chinese and CJK pixel typography.

Asset integration rules:
- copy downloaded assets into `public/assets/vendor/<source>/`;
- preserve original license text;
- create `THIRD_PARTY_NOTICES.md`;
- do not hotlink assets;
- do not fetch assets with unclear licenses;
- do not mix multiple UI packs in the first implementation.

---

## Frontend polish checklist

- [ ] Timer is visible within one second.
- [ ] Current module is obvious.
- [ ] Robot's information is visually separated from the manual.
- [ ] Recognized gesture feedback appears within 100 ms.
- [ ] Destructive actions require confirmation.
- [ ] Robot disconnect does not crash the game.
- [ ] Every screen has one obvious primary action.
- [ ] Pixel sprites are never blurry.
- [ ] UI can be played entirely by keyboard.
- [ ] Demo mode can finish in under three minutes.
