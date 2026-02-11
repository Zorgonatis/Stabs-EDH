# Changelog

All notable changes to Stabs-EDH preset will be documented in this file.

## [2.2.0] - 2026-02-11

### Added
- **GLM5 Support:** Full compatibility with GLM-5 model
- **Narrative Perspective Directive (Tier 1):** 
  - Enforces second-person perspective for all narrative prose
  - Directly addresses user as "you" for enhanced immersion
  - NPC dialogue and actions remain in their natural perspective
  
- **Anti-Deitism Directive (Tier 1):**
  - Grounds character reactions in established personality, motives, and context
  - Prevents NPCs from treating user actions as extraordinary unless earned
  - Eliminates praise for mundane actions
  - Ensures responses reflect character's natural biases, skepticism, or indifference

### Enhanced
- **Informational Realism (NPC Firewall):**
  - Added **Sensory Verification** rules
  - Verifies user has direct, unobstructed line of sight/hearing before revealing details
  - Describes obstructions only (e.g., "Her thumb covers the screen") when content is hidden
  - Provides visual cues only when distant (e.g., "The screen glows red")

- **Environmental Factors Directive:**
  - Added **Structured Output Format:** `[ [Time Emoji] Date | [Location Emoji] Location | [Weather Emoji] Weather ]`
  - Requires status bar at top of every response
  - Tracks Date/Time, Location, and Weather with strict formatting
  - Uses contextual emojis for each category
  - Evolves factors logically based on narrative context

### Documentation
- Updated README with comprehensive documentation for all new directives
- Migrated from GLM 4.7 to GLM 5 references throughout
- Added detailed tier-by-tier directive descriptions
- Enhanced configuration tips section

### Moved
- Previous version `Stabs-GLM-Directives-v2.1.json` moved to `old/` directory
- New version `Stabs-GLM5-Directives-v2.2.json` added to main directory

## [2.1.0] - Previous Release

### Added
- Initial GLM 4.7 support
- Core directive hierarchy (Tiers 0-4)
- Basic NPC tracking and visual toolkit
- WebDev enhancements
- Segfault and Gooner assistant options

### Deprecated
- No deprecated features in this release

### Removed
- No removed features in this release

### Security
- No security updates in this release