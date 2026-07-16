# Bot or Bomb — Robot WebSocket Protocol

## Transport

WebSocket JSON messages.

Default endpoint:

```text
ws://localhost:8081/robot
```

Frontend environment variable:

```text
VITE_ROBOT_WS_URL=ws://localhost:8081/robot
```

---

## Frontend → Robot

### Move to station

```json
{
  "type": "MOVE_TO",
  "station": "B",
  "requestId": "req-001"
}
```

### Scan

```json
{
  "type": "SCAN",
  "requestId": "req-002"
}
```

### Confirm physical action

```json
{
  "type": "CONFIRM_ACTION",
  "actionId": "CUT_BLUE",
  "requestId": "req-003"
}
```

### Abort

```json
{
  "type": "ABORT",
  "requestId": "req-004"
}
```

### Celebrate

```json
{
  "type": "CELEBRATE",
  "requestId": "req-005"
}
```

---

## Robot → Frontend

### Connected

```json
{
  "type": "CONNECTED",
  "robotId": "bot-01"
}
```

### Position update

```json
{
  "type": "POSITION",
  "station": "B"
}
```

### Scan result

```json
{
  "type": "SCAN_RESULT",
  "moduleId": "BLUE_TRIANGLE_01",
  "color": "BLUE",
  "symbol": "TRIANGLE",
  "confidence": 0.58,
  "requestId": "req-002"
}
```

### Action started

```json
{
  "type": "ACTION_STARTED",
  "actionId": "CUT_BLUE",
  "requestId": "req-003"
}
```

### Action completed

```json
{
  "type": "ACTION_COMPLETED",
  "actionId": "CUT_BLUE",
  "success": true,
  "requestId": "req-003"
}
```

### Error

```json
{
  "type": "ERROR",
  "code": "CAMERA_TIMEOUT",
  "message": "The module could not be scanned."
}
```

---

## Reliability rules

- Every command has a `requestId`.
- The UI shows a timeout after 5 seconds.
- Duplicate events with the same request ID are ignored.
- Invalid messages are logged but do not crash the app.
- The WebSocket adapter automatically retries with exponential backoff.
- The player can switch to simulation mode after a disconnect.
