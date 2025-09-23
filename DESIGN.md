---

### 📜 DESIGN.md
```md
# DESIGN.md

## Stack Choice
- **Node.js + Express** chosen for simplicity, lightweight setup, and built-in JSON support.
- Logging handled via **morgan** for visibility.

## API Design
- Single endpoint:
  - GET `/convert?lbs=<number>`
- Returns JSON:
  ```json
  { "lbs": <number>, "kg": <number>, "formula": "kg = lbs * 0.45359237" }

