# Low Desync Framework (LD) - Mod Documentation

## Overview
The Low Desync Framework is a CK3 mod designed to prevent multiplayer desyncs by automatically cleaning up stale relationship data when characters die or relationships become invalid.

## How to Enable Debug Logging

To see what the mod is doing in your game:

1. **Launch CK3 with Debug Mode:**
   - Right-click on Crusader Kings III in Steam
   - Select "Properties" → "General"
   - In "Launch Options" add: `-debug_mode`
   - Launch the game

2. **Find the Game Log:**
   - Game logs are saved in: `Documents\Paradox Interactive\Crusader Kings III\logs\game.log`
   - Open this file with Notepad or any text editor

3. **Search for LD: entries:**
   - Use Ctrl+F to search for `LD:` to find all Low Desync Framework messages
   - Each entry shows when a cleanup event fired and what it did

## Understanding Log Entries

### Event Firing Messages
When an event triggers, you'll see one of these base messages:

```
[HH:MM:SS][I][jomini_script_system.cpp:303]: LD: Death cleanup fired
[HH:MM:SS][I][jomini_script_system.cpp:303]: LD: AI state validation fired
[HH:MM:SS][I][jomini_script_system.cpp:303]: LD: Council cleanup fired
```

The event has loaded and checked for issues. **This is normal and expected.**

### Cleanup Action Messages
If an actual problem is found and fixed, you'll see a follow-up message:

```
[HH:MM:SS][I][jomini_script_system.cpp:303]: LD: Breaking betrothal to dead character
[HH:MM:SS][I][jomini_script_system.cpp:303]: LD: Divorcing dead spouse
```

This means the mod **found and removed** an invalid relationship, preventing a desync.

## Event Descriptions

### Death Events (Trigger: When any character dies)
- **ld.100** - Death cleanup fired
  - Clears betrothals involving the dead character
  - Runs on every death

- **ld.108** - Succession validation fired
  - Validates succession state after death
  - Runs for each AI ruler when someone dies

- **ld.118** - Claim inheritance fired
  - Validates claims haven't become invalid
  - Runs when character dies

- **ld.124** - Modifier cleanup fired
  - Removes stale modifiers from the dead character
  - Runs on death

### Daily Events (Trigger: Once per day for all characters)
- **ld.101** - AI state validation fired
  - Validates AI character relationships remain valid

- **ld.107** - Council cleanup fired
  - Checks council positions are still valid

- **ld.114** - Prisoner dungeon cleanup fired
  - Validates prisoner states

- **ld.115** - Interaction cleanup fired
  - Validates character interactions

### Monthly Events (Trigger: Once per month for all characters)
- **ld.104** - Betrothal cleanup fired
  - **IMPORTANT**: Breaks betrothals to dead characters
  - Most common cleanup action

- **ld.105** - Claim validation fired
  - Validates claims remain valid

- **ld.109** - Hook cleanup fired
  - Validates hooks (blackmail) targets are alive

- **ld.111** - Opinion cleanup fired
  - Validates opinions toward valid targets

- **ld.112** - Religious state fired
  - Validates religious relationships

- **ld.113** - Mercenary validation fired
  - Checks mercenary company states

- **ld.116** - Character state validation fired
  - General character state check

- **ld.117** - Marriage validation fired
  - **IMPORTANT**: Divorces dead spouses
  - Removes invalid marriages preventing desyncs

- **ld.119** - Crusade validation fired
  - Validates crusade participation

- **ld.121** - Realm drift fired
  - Validates realm independence claims

- **ld.122** - Court cleanup fired
  - Validates courtier states

- **ld.123** - Holy site validation fired
  - Validates holy site control

- **ld.125** - Excommunication check fired
  - Validates excommunication status

### Quadrennial Events (Trigger: Every 4 years)
- **ld.110** - Artifact check fired
  - Validates artifact ownership states

### War Events (Trigger: When a war starts)
- **ld.102** - War validation fired
  - Validates war participants are still alive
  - Runs for both attacker and defender

- **ld.120** - Truce safety fired
  - Validates truce states at war start

### Prisoner Events (Trigger: When character is imprisoned)
- **ld.103** - Prisoner check fired
  - Validates prisoner state consistency

### Vassal Events (Trigger: When new vassal contract created)
- **ld.106** - Vassal relationships fired
  - Validates new vassal relationships

## Common Log Patterns

### Healthy Game (No Desyncs Prevented)
```
LD: Death cleanup fired
LD: Betrothal cleanup fired
LD: Marriage validation fired
```
Events are running, but no invalid relationships found. Game state is clean.

### Active Cleanup (Desync Issues Being Prevented)
```
LD: Betrothal cleanup fired
LD: Breaking betrothal to dead character
LD: Divorcing dead spouse
LD: Marriage validation fired
```
The mod is **actively removing invalid relationships**. This is exactly what it's designed to do.

### Problem Indicator
If you see **no LD: entries at all** in the log after multiple days of gameplay, the mod may not be loading. Check:
1. Mod is enabled in CK3 launcher
2. File structure is correct (see below)
3. No syntax errors in error.log

## Mod File Structure

Ensure your mod folder has this structure:
```
low_desync/
├── descriptor.mod          (mod metadata)
├── events/
│   └── 00_ld_cleanup_events.txt
├── common/
│   ├── on_action/
│   │   └── 00_ld_on_actions.txt
│   └── game_rules/
│       └── zz_ld_rule.txt
├── localization/
│   └── english/
│       └── 00_ld_rule_l_english.yml
└── README.md (this file)
```

## Troubleshooting

### Events not firing?
1. Check `error.log` for syntax errors
2. Verify mod is enabled in launcher
3. Confirm file paths exactly match above structure
4. Re-enable debug mode

### Seeing orphaned event warnings?
1. Check on_action folder name is singular: `on_action/` (not `on_actions/`)
2. Verify event IDs in events file match those in on_action file
3. Check for duplicate `on_*` blocks in on_actions

### Game still desyncs?
1. The mod prevents known relationship desyncs (betrothals, marriages, etc.)
2. Other causes (realm changes, titles, etc.) are outside scope
3. Check logs to see which cleanup events are firing
4. Low event frequency = fewer opportunities to catch issues (expected trade-off)

## Performance Impact

The mod is designed to be lightweight:
- **Storage**: ~15KB total file size
- **Runtime**: Events run on standard CK3 pulses (daily/monthly)
- **CPU**: Minimal checks, only cleanup actions trigger side effects
- **Network**: No multiplayer sync cost (checks are local only)

## Settings

In the main menu, you can toggle between two modes:
- **MP Safe** (default): Runs all cleanup events
- **Full**: Same as MP Safe (both options are identical; placeholder for future expansion)

## Support

If you encounter issues:
1. Check `Documents\Paradox Interactive\Crusader Kings III\logs\error.log` for errors
2. Enable debug mode and review `game.log` for LD: entries
3. Verify file structure matches the layout above
4. Ensure the mod file permissions are readable by CK3
