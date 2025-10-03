# Bot Logic Profile: Scenario Definition

To make the State Tester fully portable, test scenarios should be defined directly within the bot's JSON profile. This document outlines the standardized format for doing so.

## 1. The Goal: Self-Contained Profiles

By including scenarios in the main JSON file, a single copy-paste operation can load an entire test environment: the bot's "personality" and the scripted events used to test it. This eliminates the need for separate, hardcoded scenario files.

## 2. The Standardized JSON Format

A new top-level property, `scenarios`, will be added to the root of the JSON profile. This property will be an array of scenario objects.

### Scenario Object Structure

Each scenario object has the following properties:

- **name**: (String) The descriptive name that will appear in the dropdown menu (e.g., "Full Dungeon Run")
- **description**: (String) Brief description of what the scenario tests
- **initialState**: (String) The state the bot should start in for this scenario
- **initialDungeon**: (Boolean) Whether the bot starts inside a dungeon instance
- **initialConditions**: (Object) Key-value pairs of triggers to set before the scenario begins
- **script**: (Array) A list of event objects that will be executed during the simulation

### Script Event Object Structure

Each event object within the `script` array defines an action to be performed at a specific simulation cycle:

- **cycle**: (Number) The simulation cycle on which this event should trigger
- **set**: (Object, optional) Key-value pairs of triggers to modify
- **log**: (String, optional) Message to display in the console

## 3. Example Implementation

Here is how scenarios would be represented in the standardized format within the main JSON profile:

```json
{
  "profileName": "Original GameBot Logic",
  "initialState": "S_IDLE",
  "states": [...],
  "triggers": [...],
  "logic": {...},
  "scenarios": [
    {
      "name": "Full Dungeon Run",
      "description": "Complete dungeon cycle with recovery and selling",
      "initialState": "S_IDLE",
      "initialDungeon": false,
      "initialConditions": {
        "isDungeonReady": true,
        "isAffordable": true,
        "isPotionsAvailable": true
      },
      "script": [
        { "cycle": 1, "log": "SCENARIO START: Full Dungeon Run" },
        { "cycle": 2, "set": { "isDungeonReady": true } },
        { "cycle": 8, "set": { "arrivedAtDungeon": true } },
        { "cycle": 10, "set": { "actionDungeonStartComplete": true } },
        { "cycle": 15, "set": { "isHpMpLow": true } },
        { "cycle": 18, "set": { "actionRecoverComplete": true } },
        { "cycle": 25, "set": { "isBagFull": true, "isSellableExist": true } },
        { "cycle": 30, "set": { "arrivedAtConsignment": true } },
        { "cycle": 32, "set": { "actionSellComplete": true } },
        { "cycle": 35, "set": { "isDungeonComplete": true } },
        { "cycle": 36, "log": "*** SCENARIO COMPLETE: Full dungeon cycle validated ***" }
      ]
    },
    {
      "name": "Potion Purchase Run",
      "description": "Test low potions -> buy potions -> resume activity",
      "initialState": "S_IDLE",
      "initialDungeon": false,
      "initialConditions": {
        "isGrindingReady": true,
        "isPotionsLow": true,
        "isAffordable": true,
        "isPotionsAvailable": true
      },
      "script": [
        { "cycle": 1, "log": "SCENARIO START: Potion Purchase Run" },
        { "cycle": 4, "set": { "arrivedAtConsignment": true } },
        { "cycle": 7, "set": { "actionBuyComplete": true } },
        { "cycle": 10, "set": { "arrivedAtAttack": true } },
        { "cycle": 12, "log": "*** SCENARIO COMPLETE: Potion purchase validated ***" }
      ]
    }
  ]
}
```

## 4. Advanced Scenario Features

### Complex Trigger Manipulation

You can set multiple triggers in a single cycle event:

```json
{
  "cycle": 15,
  "set": {
    "isHpMpLow": true,
    "isMonsterDefeated": false,
    "isBagFull": false
  }
}
```

### Conditional Logic Testing

Create scenarios that test edge cases and failure conditions:

```json
{
  "name": "Market Crisis Recovery",
  "description": "Test bot behavior when broke and unable to buy potions",
  "initialConditions": {
    "isPotionsLow": true,
    "isAffordable": false,
    "isPotionsAvailable": false
  },
  "script": [
    { "cycle": 5, "set": { "arrivedAtConsignment": true } },
    { "cycle": 8, "set": { "isSmallStacksToSell": true } },
    { "cycle": 12, "set": { "isConsignmentGoldToCollect": true } },
    { "cycle": 15, "set": { "isAffordable": true } },
    { "cycle": 18, "log": "*** Crisis resolved through emergency selling ***" }
  ]
}
```

### State Transition Validation

Test specific state transition paths:

```json
{
  "name": "Navigation Priority Test",
  "description": "Verify HP recovery takes priority over navigation",
  "initialConditions": {
    "isDungeonReady": true
  },
  "script": [
    { "cycle": 3, "set": { "isHpMpLow": true } },
    { "cycle": 6, "set": { "actionRecoverComplete": true } },
    { "cycle": 9, "set": { "arrivedAtDungeon": true } },
    { "cycle": 10, "log": "*** Priority handling validated ***" }
  ]
}
```

## 5. Usage Instructions

1. **Design Your Scenario**: Plan what bot behavior you want to test
2. **Define Initial Conditions**: Set the starting trigger states
3. **Script the Events**: Create a timeline of trigger changes
4. **Add to Profile**: Include the scenario in your bot's JSON profile
5. **Test and Iterate**: Load the profile and run your scenario

## 6. Best Practices

- **Start Simple**: Begin with basic scenarios before creating complex ones
- **Use Descriptive Names**: Make scenario purposes clear from their names
- **Add Log Messages**: Include progress indicators and completion messages
- **Test Edge Cases**: Create scenarios for failure conditions and recovery
- **Document Expectations**: Use descriptions to explain what should happen

This structure makes the scenario system fully dynamic and tied to the loaded profile, enabling comprehensive testing of bot logic without external dependencies.