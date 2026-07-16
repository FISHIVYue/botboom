# Bot or Bomb — Product & Game Specification

## 1. One-line pitch

**Bot or Bomb is a cooperative pixel-art bomb-defusal game in which the human and the physical robot each hold only half of the information. They must communicate under time pressure to disarm three physical modules before the timer reaches zero.**

Chinese pitch:

**《Bot or Bomb》是一款像素风人机合作拆弹游戏：玩家掌握拆弹规则，实体机器人掌握现场信息；双方必须在倒计时内通过扫描、手势确认和行动反馈共同完成任务。**

---

## 2. Why the robot is essential

The robot is not a remote-controlled cursor.

It has four essential roles:

1. **Physical explorer** — it moves between real-world bomb modules.
2. **Sensor** — it scans a QR code, AprilTag, color card, or symbol that the player cannot directly use in the app.
3. **Messenger** — it reports what it detected, including uncertainty.
4. **Actor** — it performs the selected disarm action by approaching a target, flashing an LED, pushing a light button, or signaling completion.

Removing the physical robot should break the core information loop. The app must not reveal module data until a robot scan event is received.

---

## 3. Hackathon-safe physical setup

Use a small tabletop arena with three marked stations:

- Module A
- Module B
- Module C

Each station contains one printed card. A card contains:

- a visible color;
- a visible symbol;
- a QR code or AprilTag containing a module ID;
- optionally one LED or physical push button.

Recommended MVP:

- robot can move to three pre-defined stations;
- robot camera scans QR/AprilTag;
- robot sends the decoded module ID to the web app;
- web app converts module ID into a randomized puzzle state;
- correct disarm action is simulated by robot LED/animation;
- no real cutting, gripping, or complex manipulation is required.

The frontend must include a full **Simulation Mode** so the game can still be demonstrated if the robot connection fails.

---

## 4. Core game loop

A complete mission lasts 90 seconds.

1. The player starts the mission.
2. The app shows only the bomb manual and timer.
3. The robot moves to a module.
4. The robot scans the module and sends a sensor event.
5. The app displays the robot's report:
   - detected color;
   - detected symbol;
   - confidence;
   - optional nearby-light state.
6. The player reads the manual and chooses a high-level command:
   - Scan again
   - Move left
   - Move right
   - Confirm action
   - Abort
7. The command is sent through a gesture recognizer or clickable fallback control.
8. The robot performs the action.
9. Correct action disarms the module.
10. Incorrect action adds one strike and subtracts time.
11. Disarm all three modules to win.
12. Three strikes or zero time causes an explosion.

The gameplay must alternate between:
**Observe → Interpret → Communicate → Act → Feedback.**

---

## 5. MVP puzzle rules

Use three simple puzzle families. Randomize one rule set per mission.

### Module Type 1 — Color + Symbol

Example manual rule:

- Blue triangle → Confirm
- Red circle → Abort, then rescan
- Yellow square → Move right, then confirm
- Any detection below 65% confidence → Scan again

### Module Type 2 — Sequence Memory

Robot scans a sequence of 3 symbols:
Moon → Star → Eye

The player must compare it with a rule card and send the correct order of actions.

For the MVP, the app can show three large command cards and the player clicks or gestures them in sequence.

### Module Type 3 — Trust Decision

Robot reports two possible interpretations:

- Triangle: 58%
- Diamond: 42%

The manual contains different actions for each. The player must decide whether to trust the robot or request another scan, which costs five seconds.

This creates a meaningful risk/reward decision.

---

## 6. Difficulty levels

### Easy
- 120 seconds
- confidence always above 80%
- one-step rules
- no moving obstacles

### Standard
- 90 seconds
- confidence between 55% and 95%
- one re-scan may be required
- two-step rules

### Demo
- 75 seconds
- scripted dramatic events
- guaranteed first module success
- second module produces one uncertain scan
- third module creates a final 10-second climax

Build Demo Mode first because it is optimized for judging.

---

## 7. Input design

Primary interaction:
- gesture recognition from the team's existing model.

Fallback interaction:
- keyboard and on-screen buttons.

Gesture mapping:

- Open palm / Hello → Scan
- Point left → Move left
- Point right → Move right
- OK → Confirm
- Cross arms / Away → Abort

Every gesture action must display:
- recognized command;
- confidence;
- a 600 ms confirmation window;
- visual cancel option.

Do not let accidental gestures immediately trigger destructive actions.

---

## 8. Frontend screens

### Screen 0 — Boot Sequence

Purpose:
- establish the aesthetic;
- preload assets;
- test audio;
- check robot connection.

Elements:
- pixel-art logo;
- animated terminal text;
- progress bar;
- ROBOT LINK status;
- “Enter Training” button after loading.

Duration:
- skip automatically after 2–3 seconds;
- click to skip.

---

### Screen 1 — Main Menu

Primary elements:
- logo: BOT OR BOMB;
- subtitle: HUMAN + ROBOT DEFUSAL UNIT;
- Start Mission;
- Tutorial;
- Settings;
- robot connection indicator;
- simulation mode toggle.

Background:
- dark pixel-art operations room;
- robot silhouette;
- slow blinking warning light.

Do not include more than four primary actions.

---

### Screen 2 — Gesture Calibration / Tutorial

Purpose:
- teach interaction in under 30 seconds.

Layout:
- left: animated pixel hand icon or simple silhouette;
- center: current gesture name;
- right: live recognition confidence;
- bottom: five slots showing completed gestures.

Flow:
1. Show gesture.
2. Ask player to perform it.
3. Fill the slot when recognized.
4. Continue automatically.
5. Allow “Use keyboard fallback”.

Completion message:
“COMMUNICATION LINK ESTABLISHED.”

---

### Screen 3 — Mission Briefing

Elements:
- mission code;
- difficulty;
- 3 module silhouettes;
- rule summary;
- timer preview;
- one large “Deploy Robot” button.

Animation:
- modules appear one by one;
- timer flips from `--:--` to `01:30`;
- robot status changes from STANDBY to READY.

Keep this screen under 8 seconds during the live demo.

---

### Screen 4 — Main Gameplay Screen

Target viewport:
- desktop 1440×900;
- minimum 1280×720;
- kiosk-style full screen.

#### Top Status Bar — 72 px

Left:
- mission ID;
- robot connection dot;
- current phase.

Center:
- large countdown timer.

Right:
- strike indicators ×3;
- sound button;
- pause button.

The timer must be the most visually dominant number.

#### Left Main Area — 62%

Contains:
- pixel-art tabletop map;
- three module stations;
- robot avatar position;
- movement path;
- module states: UNKNOWN / SCANNING / ARMED / DISARMED;
- small particle effects for scanning.

The physical robot position should be represented by a matching pixel avatar.

#### Right Manual Panel — 38%

Contains:
- title: DEFUSAL MANUAL;
- current robot report;
- current rule card;
- robot confidence meter;
- recommended next actions;
- command history.

The manual must show only information needed for the current module. Do not make the player read a wall of text.

#### Bottom Command Dock — 112 px

Contains five large pixel buttons:

1. SCAN
2. LEFT
3. RIGHT
4. CONFIRM
5. ABORT

Each button shows:
- icon;
- command label;
- gesture hint;
- keyboard shortcut.

When a gesture is recognized:
- matching button rises by 4 px;
- border flashes;
- command text appears above the dock;
- a short countdown confirms activation.

---

### Screen 5 — Robot Question Overlay

This is a modal overlay used when confidence is low.

Example:

> “I MAY BE LOOKING AT A TRIANGLE.”
> CONFIDENCE: 58%
> RESCAN COST: -5 SEC

Actions:
- Trust Robot
- Scan Again

Visual treatment:
- background freezes and darkens;
- robot portrait shows uncertainty;
- timer continues running;
- warning sound plays once.

This overlay is the signature HRI moment of the project.

---

### Screen 6 — Module Disarmed Moment

Duration:
- 1.0–1.5 seconds;
- do not navigate away from gameplay.

Effects:
- station flashes cyan/green;
- small pixel explosion becomes a sparkle;
- robot celebrates;
- “MODULE A DISARMED” appears;
- timer gains optional +5 seconds;
- next station highlights.

---

### Screen 7 — Failure

Trigger:
- timer reaches zero;
- or third strike.

Sequence:
1. screen freezes for 200 ms;
2. red flash;
3. pixelated explosion;
4. UI fragments shake;
5. text: SIGNAL LOST / MISSION FAILED;
6. show retry and mission summary.

Avoid realistic violence. Keep it arcade-like.

---

### Screen 8 — Success / Results

Elements:
- robot victory animation;
- total time;
- modules disarmed;
- trust decisions;
- rescans;
- strikes;
- teamwork score;
- replay button.

Optional final insight:
“You trusted the robot 2 times. One was correct.”

This makes the HRI concept visible to judges.

---

### Screen 9 — Hidden Demo Console

Open with `D`.

Purpose:
- guarantee a stable hackathon demo.

Controls:
- connect/disconnect robot;
- inject scan event;
- set robot position;
- force confidence value;
- disarm current module;
- add strike;
- freeze timer;
- restart mission;
- select scripted demo scenario.

The console should not be visible in the normal UI.

---

## 9. Visual system

### Direction

**16-bit emergency operations room + playful arcade tension.**

Avoid:
- generic futuristic blue dashboard;
- photorealistic bomb imagery;
- excessive gradients;
- dozens of tiny panels;
- mixing multiple pixel-art styles.

### Pixel grid

Use an 8 px layout grid.

Recommended spacing values:
- 8, 16, 24, 32, 48, 64 px.

Use integer scaling for pixel sprites:
- 1×, 2×, 3×, 4× only.

CSS:
- `image-rendering: pixelated`;
- avoid fractional transforms on sprites;
- use stepped animations instead of smooth floating motion.

### Palette

- Ink: `#11131F`
- Panel: `#1B2035`
- Raised panel: `#29314D`
- Paper/cream: `#F3E7C5`
- Cyan signal: `#5CD6C0`
- Yellow warning: `#F3C95B`
- Red danger: `#E55364`
- Muted text: `#93A0B8`

Use red only for:
- time under 15 seconds;
- strikes;
- destructive actions;
- failure.

### Typography

Chinese and CJK:
- Fusion Pixel Font, 12 px or 16 px variants, local file.
- License: SIL Open Font License.

English labels:
- use the same family when possible;
- all caps for short control labels.

Readability rule:
- pixel font for headings, labels, and short status text;
- use a clean system sans-serif for long instructions if the pixel font becomes tiring.

### Buttons

Every button has four states:
- idle;
- hover/focus;
- pressed;
- disabled.

Pixel button behavior:
- 4 px hard shadow;
- pressed state moves down 4 px and removes shadow;
- no soft shadow;
- strong focus outline for keyboard access.

### Panels

Use 9-slice pixel panels from one asset pack only.

Recommended:
- Kenney Pixel UI Pack, CC0.

Do not mix Kenney panels with another UI pack unless they are manually normalized.

---

## 10. Motion and feedback

Use short, stepped animation.

- hover: 80–120 ms;
- button press: 80 ms;
- panel reveal: 160–240 ms;
- scan pulse: 600–900 ms;
- module success: 1000–1500 ms;
- failure sequence: under 2500 ms.

Animation principles:
- gameplay information first;
- no decorative animation that obscures the timer;
- respect `prefers-reduced-motion`.

Sound:
- one click sound;
- one scan sound;
- one warning sound;
- one success jingle;
- one failure sound.

Recommended:
- Kenney Interface Sounds, CC0;
- Howler.js for playback, MIT.

---

## 11. Frontend architecture

Recommended stack:

- React
- TypeScript
- Vite
- Phaser 3 for the pixel map and sprite layer
- Zustand for game state
- Howler.js for sound
- Vitest for unit tests
- Playwright for end-to-end browser tests

Use React for:
- menus;
- panels;
- manual;
- controls;
- modals;
- HUD.

Use Phaser only for:
- map;
- robot sprite;
- module stations;
- particles;
- short game-world animations.

Do not build the entire interface in Canvas. DOM-based UI is faster to polish and more accessible.

---

## 12. State machine

Top-level states:

- `boot`
- `menu`
- `calibration`
- `briefing`
- `mission.running`
- `mission.awaitingRobot`
- `mission.awaitingHuman`
- `mission.executing`
- `mission.moduleSuccess`
- `mission.success`
- `mission.failure`

Important events:

- `ROBOT_CONNECTED`
- `ROBOT_DISCONNECTED`
- `MISSION_STARTED`
- `ROBOT_POSITION_CHANGED`
- `SCAN_STARTED`
- `SCAN_RESULT_RECEIVED`
- `GESTURE_DETECTED`
- `COMMAND_CONFIRMED`
- `ROBOT_ACTION_STARTED`
- `ROBOT_ACTION_COMPLETED`
- `MODULE_DISARMED`
- `STRIKE_ADDED`
- `TIMER_EXPIRED`

The UI must be driven from state, not from arbitrary component-local flags.

---

## 13. Robot adapter

Create a single interface so mock and real robot modes are interchangeable.

```ts
export type RobotCommand =
  | { type: "MOVE_TO"; station: "A" | "B" | "C" }
  | { type: "SCAN" }
  | { type: "CONFIRM_ACTION"; actionId: string }
  | { type: "ABORT" }
  | { type: "CELEBRATE" };

export type RobotEvent =
  | { type: "CONNECTED" }
  | { type: "DISCONNECTED"; reason?: string }
  | { type: "POSITION"; station: "A" | "B" | "C" | "BASE" }
  | {
      type: "SCAN_RESULT";
      moduleId: string;
      color: "RED" | "BLUE" | "YELLOW";
      symbol: "TRIANGLE" | "CIRCLE" | "SQUARE" | "DIAMOND";
      confidence: number;
    }
  | { type: "ACTION_STARTED"; actionId: string }
  | { type: "ACTION_COMPLETED"; actionId: string; success: boolean }
  | { type: "ERROR"; message: string };
```

Adapters:

```ts
interface RobotAdapter {
  connect(): Promise<void>;
  disconnect(): Promise<void>;
  send(command: RobotCommand): void;
  subscribe(listener: (event: RobotEvent) => void): () => void;
}
```

Implement:
- `MockRobotAdapter`
- `WebSocketRobotAdapter`

Default WebSocket endpoint:
- `ws://localhost:8081/robot`

Use environment variable:
- `VITE_ROBOT_WS_URL`

---

## 14. Data model

Puzzle data must be stored in JSON, not hard-coded inside components.

Example:

```json
{
  "moduleId": "BLUE_TRIANGLE_01",
  "type": "COLOR_SYMBOL",
  "report": {
    "color": "BLUE",
    "symbol": "TRIANGLE"
  },
  "rules": [
    {
      "condition": {
        "color": "BLUE",
        "symbol": "TRIANGLE",
        "minimumConfidence": 0.65
      },
      "correctAction": "CONFIRM"
    }
  ]
}
```

Create at least:
- 6 module definitions;
- 3 scripted demo missions;
- 1 random mission generator.

---

## 15. Accessibility and demo resilience

- keyboard controls for all actions;
- visible focus states;
- mute control;
- reduced-motion mode;
- readable high contrast;
- no essential information conveyed only by color;
- reconnect banner;
- full simulation mode;
- hidden demo controls;
- deterministic demo seed.

The demo must remain playable without:
- a camera;
- gesture recognition;
- physical robot;
- network connection.

These fallbacks are for reliability, not the primary presentation.

---

## 16. Definition of done

The MVP is complete when:

1. A user can complete a full three-module mission.
2. The timer, strikes, and result score work.
3. Gesture input can be simulated from keyboard.
4. Mock robot mode works end to end.
5. Real robot events can enter through WebSocket.
6. The UI clearly distinguishes human information from robot information.
7. A low-confidence robot question appears at least once.
8. Success and failure sequences work.
9. The design uses one coherent pixel-art system.
10. Playwright can complete a scripted winning mission.

---

## 17. Scope cuts

Do not build during the hackathon:

- account system;
- database;
- online multiplayer;
- level editor;
- true machine learning inside the frontend;
- procedural pixel-art generation;
- complex robot arm manipulation;
- mobile responsive version;
- realistic bomb graphics;
- more than three puzzle families.

Focus on one polished 3-minute demo.
