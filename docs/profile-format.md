# Bot Logic Profile: Standardized Definition

This document outlines the "pseudo-code" standard for defining a custom state machine for the simulator. The format is a single JSON object that describes all the states, triggers, and transition logic for a given bot.

## 1. Deconstruction of the Current GameBot

To create a generic standard, we first identify the unique components of our existing bot:

### States
The distinct phases of operation:
- IDLE, BAGFULL, Maps, ATTACK, RECOVER, SELL_CONSIGNMENT, BUY_POTIONS, START_DUNGEON, MARKET_WAIT, EXIT

### Triggers (Conditions)
The boolean flags that the bot checks to make decisions:
- `isHpMpLow`, `isBagFull`, `isPotionsLow`, `arrivedAtConsignment`, `isDungeonReady`, `actionSellComplete`, etc. (19 total)

### Logic (Rules)
The "if-this-then-that" decision-making process. These are the core of the bot's "personality."

**Example Rules:**
- If the current state is IDLE and the trigger `isHpMpLow` is true, then transition to the RECOVER state
- If the current state is Maps and the trigger `arrivedAtDungeon` is true, then transition to the START_DUNGEON state

### Actions
The specific tasks the bot performs within a state, which result in log messages:
- `move_to()`, `attack_monster()`, `buy_potions()`, `collect_sold_gold()`, etc.

## 2. The Standardized JSON Format

A "Bot Logic Profile" is a single JSON object with three root properties: `states`, `triggers`, and `logic`.

### states
An array of objects, where each object defines a state:

```json
"states": [
  { "key": "S_IDLE", "label": "Idle" },
  { "key": "S_NAVIGATE", "label": "Navigate" },
  { "key": "S_ATTACK", "label": "Attacking" }
]
```

### triggers
An array of objects defining the conditions. The `color` property dictates the color of the indicator light in the UI:

```json
"triggers": [
  { "key": "isHpMpLow", "label": "HP/MP Low", "color": "red" },
  { "key": "isBagFull", "label": "Bag Full", "color": "orange" },
  { "key": "arrivedAtDest", "label": "Arrived", "color": "cyan" }
]
```

### logic
An object where each key is a state key (e.g., "S_IDLE"). The value is an array of rules that are evaluated in order. The first rule that evaluates to true is executed.

Each rule has three parts:
- **if**: A string containing a logical condition using trigger keys. The engine will safely evaluate this expression
- **actions**: An array of strings representing functions to call. These will print to the console (e.g., `["log:Starting combat rotation"]`)
- **transitionTo**: The key of the state to move to next

```json
"logic": {
  "S_IDLE": [
    {
      "if": "isHpMpLow",
      "actions": ["log:HP or MP is low, beginning recovery."],
      "transitionTo": "S_RECOVER"
    },
    {
      "if": "isGrindingReady && !arrivedAtGrindSpot",
      "actions": ["log:Moving to grinding location."],
      "transitionTo": "S_NAVIGATE"
    }
  ],
  "S_NAVIGATE": [
    {
      "if": "arrivedAtGrindSpot",
      "actions": ["log:Arrived at destination."],
      "transitionTo": "S_ATTACK"
    }
  ]
}
```

## 3. Complete Example Profile

Here's a minimal but complete bot profile:

```json
{
  "profileName": "Simple Combat Bot",
  "initialState": "S_IDLE",
  "states": [
    { "key": "S_IDLE", "label": "Idle" },
    { "key": "S_NAVIGATE", "label": "Navigating" },
    { "key": "S_ATTACK", "label": "Attacking" },
    { "key": "S_RECOVER", "label": "Recovering" }
  ],
  "triggers": [
    { "key": "isHpLow", "label": "HP Low", "color": "red" },
    { "key": "isEnemyNear", "label": "Enemy Near", "color": "orange" },
    { "key": "arrivedAtTarget", "label": "Arrived", "color": "cyan" },
    { "key": "recoveryComplete", "label": "Recovered", "color": "green" }
  ],
  "logic": {
    "S_IDLE": [
      { "if": "isHpLow", "transitionTo": "S_RECOVER" },
      { "if": "isEnemyNear", "transitionTo": "S_ATTACK" },
      { "if": "true", "actions": ["log:Waiting..."], "transitionTo": "S_IDLE" }
    ],
    "S_ATTACK": [
      { "if": "isHpLow", "transitionTo": "S_RECOVER" },
      { "if": "!isEnemyNear", "transitionTo": "S_IDLE" },
      { "if": "true", "actions": ["log:Fighting enemy!"], "transitionTo": "S_ATTACK" }
    ],
    "S_RECOVER": [
      { "if": "recoveryComplete", "transitionTo": "S_IDLE" },
      { "if": "true", "actions": ["log:Healing..."], "transitionTo": "S_RECOVER" }
    ]
  }
}
```

## 4. Usage Instructions

1. **Copy the Profile**: Copy your complete JSON profile to the clipboard
2. **Open Settings**: Click the ⚙️ gear icon in the State Machine Logic Tester
3. **Navigate to Profile Tab**: Click on "Logic Profile" tab
4. **Paste and Save**: Paste your JSON and click "Save and Reload"
5. **Test Your Logic**: Use the trigger toggles and scenarios to validate your state machine

This standardized format completely separates the bot's unique personality from the simulator itself, making it easy to test different bot behaviors and share configurations.