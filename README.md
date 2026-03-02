# CK3 Low Desync Framework

A lightweight multiplayer-focused framework designed to reduce desync risk in Crusader Kings III.

This mod adds a multiplayer safe mode that prioritizes deterministic behavior by:
- Cleaning up invalid schemes and interactions (dead, imprisoned, invalid targets)
- Handling edge cases around wars, titles, travel, hostages, marriages, and activities
- Using event-driven, bounded cleanup instead of heavy scans or random logic
- Works better when there are no Great Holy Wars! 
- **There are some desyncs that will appear due to engine level issues we cant resolve.**

## Issues
If you encounter a desync, please report what was happening immediately before it occurred (event, interaction, war, scheme, etc.) so additional safeguards can be added. 
