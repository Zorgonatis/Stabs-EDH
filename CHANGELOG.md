# Changelog

All notable changes to Stabs-EDH preset will be documented in this file.

## [2.3.0] - 2026-02-15

### Added
- **NPC Cognitive Bounds Directive (Tier 1):**
  - New consolidated directive combining grounding and perception rules
  - Covers Knowledge Limits, Perceptual Limits, Physical Grounding
  - Includes Relationship Depth guidance and Internal Voice rules
  
- **Custom Assistant Template:**
  - New customizable assistant with editable persona field
  - Enabled by default with placeholder "Dave the penguin" persona
  
- **Extra Assistants Header/End Directives:**
  - New wrapper directives for cleaner assistant organization

- **George and Nico Assistant:**
  - New assistant featuring Broken Sword protagonists
  - Replaces Journalist Assistant

### Changed
- **Task Steering GLM5 - Complete Restructure:**
  - New "START HERE - Deep Thinking CoT Instructions" header
  - Explicit S1-S3 stages with checkboxes for structured execution
  - Added mock skeleton creation step before final output
  - New `{{getvar::assistants}}` variable for cleaner assistant handling
  - Added guidance for known character definitions in S2

- **Environmental Factors Directive - Full Rewrite:**
  - Simplified output format
  - Added Time Scaling table with action-type-based advancement
  - Added Time Skip Protocol with user intervention checks
  - More flexible emoji usage

- **Extra Assistants Formatting:**
  - Renamed all assistants with `║` prefix for visual hierarchy
  - Added HTML-only prohibition (no markdown in assistant outputs)
  - Changed variable from `enhancements` to `assistants`

- **Directive Output Formatting:**
  - All Tier headers now use bold markdown
  - All addvar outputs include checkbox format (`- [ ]`)
  - Creates consistent checklist-style resolution

- **Visual Toolkit - Simplified:**
  - Removed detailed technical specifications
  - Now focuses on Style State themed augments

- **Chaotic Thoughts - Streamlined:**
  - Simplified title and content structure

- **Web Dev:**
  - Added prohibition for markdown within HTML

### Deprecated
- **Grounding Directive:** Functionality merged into NPC Cognitive Bounds
- **Journalist Assistant:** Replaced by George and Nico

### Configuration
- Default API changed from OpenRouter to NanoGPT
- NanoGPT model set to `zai-org/glm-5:thinking`
- SEGFAULT assistant now disabled by default
- Custom Assistant enabled by default

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