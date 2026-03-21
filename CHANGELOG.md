# Changelog

All notable changes to Stabs-EDH preset will be documented in this file.

## [2.5.0] - 2026-03-21

### Added
- **SETTINGS Configuration Prompt:**
  - New centralized configuration prompt for narrative customization
  - Dynamic macro variables for runtime overrides:
    - `{{setvar::narrativeperspective::THIRD PERSON LIMITED}}`
    - `{{setvar::stylestateoverride::None (Dynamic)}}`
    - `{{setvar::narrativelengthoverride::Short-Medium}}`
  - Includes comprehensive documentation for all narrative perspective options (Third Person Limited/Omniscient/Objective, First/Second Person, First Person Plural)
  - Enabled by default in prompt_order

- **AI Roles End Marker:**
  - New empty marker prompt (`╚══ AI Roles End`) for visual hierarchy closure

### Changed
- **Visual Toolkit (VTK) - Complete Rewrite:**
  - Replaced prescriptive rules with creative "flavors" table:
    - **Mindscape**: Internal conflict, strong emotions, impulses (hierarchical screams, mental debris, contradiction overlays)
    - **Interface**: Tech, communication, digital media (phone UI, terminals, holographic displays)
    - **Document**: Papers, letters, ledgers, lore (dark paper, handwritten notes)
    - **Artifact**: Objects worth examining (RPG-style inspection cards with stats)
    - **Subtext**: Hidden meanings, magical influence (underlined phrases, animated effects)
    - **Dialogue Spotlight**: Key NPC moments (themed containers)
  - Added Creative Freedom Principles: quantity flexibility, hybridization, innovation, organic placement
  - Added Technical Foundation section: HTML5/CSS3, CSS Grid, Flexbox, custom properties, @keyframes animations
  - Added `<!-- VTK_START -->` / `<!-- VTK_END -->` wrapper comments
  - Added prohibition: No animated `box-shadow` (use border/background/opacity instead)
  - Removed rigid "3-4 high confidence" requirement in favor of scene-driven quantity

- **Narrative Perspective:**
  - Changed from hardcoded second-person to `{{getvar::narrativeperspective}}` variable
  - Now configurable via SETTINGS prompt (default: THIRD PERSON LIMITED)

- **Style State / Genre:**
  - Renamed to "Dynamic Style State"
  - Added `State Override: {{getvar::stylestateoverride}}` integration
  - Simplified reference list (removed specific VTK/Chaotic Thoughts/NavMap mentions)

- **Narrative Length Control:**
  - Default changed from hardcoded "Short" to `{{getvar::narrativelengthoverride}}`
  - Now configurable via SETTINGS prompt (default: Short-Medium)

- **Visual Hierarchy (Box-Drawing Characters):**
  - AI Roles header: `➢` → `╔══`
  - Role Enhancements header: `➢` → `╠══`
  - Individual role prompts: Added `║` prefix for visual nesting

### Configuration
- File size increased from 123KB to 126KB (90 → 92 prompts)

## [2.4.7] - 2026-03-03

### Fixed
- **Extra Assistants Toggle Bug:**
  - Added missing `{{setvar::assistants::}}` variable injection to AI Roles header
  - Fixes issue where Extra Assistants feature would remain stuck on even when disabled
  - The `assistants` variable is now properly set, allowing the toggle to function correctly

## [2.4.6] - 2026-02-27

### Changed
- **Role - Deep Researcher → Role - Director:**
  - Complete rewrite from academic researcher to Immersive Experience Director
  - New theatrical/cinematic approach with Directorial Process (6-step method)
  - Added Key Traits table: Methodical Precision, Creative Adaptability, Perceptual Manipulation, Multi-Stream Processing, Scene Awareness
  - Includes "What You Are Not" clarifications
  - Emphasizes scene direction, perception control, and narrative staging
  - Renamed from "Role - Deep Researcher (*)" to "Role - Director (Recommended but large)"

- **NPC Behavioural Coherence:**
  - Expanded with new emphasis on proactive NPC agency
  - Added: NPCs initiate conflict, intimacy, and escalation based on own drives
  - Added: World does not cushion falls; rescue is never guaranteed
  - Added: Danger arrives without foreshadowing; close calls sometimes result in collision
  - Added: Boundaries discovered through interaction, not lecture

- **Task Steering:**
  - Added `{{getvar::assistantcheck}}` integration for assistant tracking
  - Renamed from "TASK STEERING GLM5 🎯" to "TASK STEERING 🎯"
  - Simplified constraint #4 wording (removed redundant "or")

- **AI Roles Header:**
  - Renamed from "SOUL.md - Who you are." to "SELF.md"
  - Cleaned up setvar syntax (removed unnecessary spaces)
  - Added `{{setvar::assistantcheck:}}` for assistant tracking

- **Extra Assistants:**
  - Renamed from "╔══ 𝗘𝘅𝘁𝗿𝗮 𝗔𝘀𝘀𝗶𝘀𝘁𝗮𝗻𝘁𝘀 (*)" to "╔══ 𝗘𝘅𝘁𝗿𝗮 𝗔𝘀𝘀𝗶𝘀𝘁𝗮𝗻𝘁 (prompt)"
  - Added bold formatting to "full-width HTML block"
  - Added `{{addvar::assistantcheck::- [ ] Extra Assistant\n}}` output

- **Structured Visual Data:**
  - Genericized reference from "**NPC Definition of The Mansion**" to "**NPC Definition of <CHAR>**"

- **Role Prompt Naming Cleanup:**
  - "Role - Writer (Recommended)" → "Role - Writer"
  - "Role - Simulator (Recommended)" → "Role - Simulator"

### Added
- **New Assistant: Dr. Emmett Brown (BTTF):**
  - Character persona assistant for Back to the Future roleplay
  - Includes AvatarURL reference for visual identity
  - Disabled by default in prompt_order

### Configuration
- File size increased from 120K to 123K (89 → 90 prompts)

## [2.4.5] - 2026-02-20

### Changed
- **Task Steering GLM5 - Workflow Restructure:**
  - S1 and S2 swapped: Directive Hydration (T0-T4) now executes before Detailed Plan
  - S3 renamed from "Drafting" to "Piece-by-piece formulation" with expanded sub-steps:
    - S3A: Narrative beats
    - S3B: Identify VTK Entities that fit
    - S3C: Craft skeleton structure
    - S3D: Remove redundant prose
  - S4 renamed from "Validate" to "Self-correction"
  - Added new constraint: "Do not hide or obscure your chain of thought; output everything"
  - Output format now references "Generated each turn during instruction S3C"

- **Visual Toolkit - Complete Overhaul:**
  - Renamed to "Visual Toolkit (VTK)" with fun, iterative approach
  - Now recommends 3-4 high confidence VTK enhancements per output
  - Expanded VTK Entities Reference with new categories and clearer guidance:
    - "For LITERATURE/LISTS/DOCUMENTS": Now uses dark clay tablet with contrasting handwritten font
    - "For OBJECTS": Added RPG-styled (+4 strength leather belt) humor and decorative styling
    - "For DENSE INFORMATION": New category for structured documents (ledgers, receipts)
  - Added new prohibitions:
    - Do not extend prose to accommodate VTK Entities
    - If VTK Entity captures information, do not repeat in prose
  - addvar output changed to "- [ ] Visual Toolkit Entities"

### Configuration
- **Function Calling:** Enabled (was disabled)
- **SEGFAULT Assistant:** Enabled by default in prompt_order (was disabled)
- **Custom Assistant:** Disabled by default in prompt_order (was enabled)
- **EDH Definition:** Enabled by default in prompt_order (was disabled)

## [2.4.4] - 2026-02-20

### Changed
- **Task Steering GLM5 - Complete Restructure:**
  - Replaced XML tag structure (`<this_turn_instructions>`) with cleaner `<response_process>` format
  - Converted numbered steps to checkbox-driven workflow (S1-S5 with sub-checkboxes)
  - Added `Directive Hydration T0-T4` step for systematic directive breakdown
  - Removed verbose parameters section in favor of focused constraints
  - Output format now dynamically references `{{getvar::enhancements}}` and `{{getvar::assistants}}`
  - Added `{{getvar::csworkaround}}` at start for jailbreak integration
  - Ends with clear `--- END INSTRUCTIONS ---` marker

- **Visual Toolkit:**
  - Reordered execution triggers for better priority
  - Added dedicated "For OBJECTS" trigger with RPG-styled inspection container
  - Changed "For PEOPLE, OBJECTS OR ANIMALS" to separate "For PEOPLE OR ANIMALS" (objects now have dedicated handler)
  - Added location guidance: "Anchored to relevant prose, never top-or-tailing the narrative"

- **Color Dialogue/Thoughts:**
  - Hardcoded "Dark" theme instead of `{{getvar::color_scheme}}` variable
  - Removed "Keep paragraphs intact & together" prohibition

- **Chaotic Thoughts:**
  - Expanded trigger conditions: "shock and surprise or powerful internal impulses"
  - Previously only triggered on "significant internal impulses"

- **Environmental Factors:**
  - Added clarification: "this is a meta tracker NOT a VTK element"

- **NPC Scene Presence Limit:**
  - Updated addvar output to include checkbox format (`- [ ]`)

- **Web Dev:**
  - Cleaned up trailing text in Forbidden section

### Added
- **Role - Deep Researcher (*) Directive:**
  - New role emphasizing methodical analysis of system prompts and context
  - Focuses on exhaustive directive analysis with "impeccable attention to detail"
  - Experimental role for improved prompt adherence

### Removed
- **Immersion Engine Directive:** Replaced by Role - Deep Researcher

### Configuration
- **Role - Simulator:** Disabled by default in prompt_order (was enabled)
- **Role - Deep Researcher:** Enabled by default in prompt_order
- **Structured Visual Data:** Disabled by default in prompt_order (was enabled)

## [2.4.3] - 2026-02-19

### Added
- **Narrative Length Control Directive (Tier 2):**
  - New directive controlling output size with Small/Medium/Large tiers
  - Default: Short (2-4 paragraphs, single beat)
  - Includes time/scene progression limits
  - Enforces STOP before resolution moments

- **NPC Scene Presence Limit Directive (Tier 3):**
  - Limits active (speaking/acting) NPCs to 1-2 per response
  - Defines active vs passive NPC presence
  - Provides priority guidelines when cap is reached
  - Exceptions for full-cast gatherings and climactic moments

### Changed
- **Task Steering GLM5:**
  - Restructured execution steps with sub-steps (3A: Identify positions, 3B: Craft skeleton)
  - Added validation sub-step (4A: Correct oversights)
  - Updated output format instruction to reference step 3B

- **Visual Toolkit:**
  - Changed from "augments" to "fun augments"
  - Restructured execution triggers with clearer categories
  - Added separate triggers for TECH (interfaces/UIs), LITERATURE/LISTS/DOCUMENTS

- **Style State / Genre:**
  - Moved from Tier 2 to Tier 3 for better directive ordering

- **Stop-and-Pass:**
  - Renamed to "Stop-and-Pass (Slow, good for RPG) - CRITICAL"
  - Added "- CRITICAL" emphasis to directive name and checkbox

- **Color Dialogue/Thoughts:**
  - Added prohibition: "Keep paragraphs intact & together"

- **Web Dev:**
  - Enhanced mandatory wrapper instruction for VTK placement
  - Clarified placement guidance: "placed appropriately (unless location specified, within the planned narrative)"

### Configuration
- **OpenRouter Sort Models:** Changed from "alphabetically" to "pricing.prompt"

## [2.4.2] - 2026-02-19

### Fixed
- **Task Steering GLM5 Instructions:**
  - Fixed numbering sequence (was 1, 2, 2, 3, 4 → now properly 1, 2, 3, 4, 5)

### Changed
- **Task Steering GLM5 Constraints:**
  - Removed "Never start with `<CHAR>:`" constraint - model handles this correctly without explicit instruction
  
### Configuration
- **Names Behavior:** Changed from `1` to `-1` (improves character name handling in multi-character scenarios)

## [2.4.1] - 2026-02-19

### Changed
- **Task Steering GLM5 - Major Restructure:**
  - Converted from `system` role to `user` role for tighter instruction following
  - Restructured with XML-style tags: `<this_turn_instructions>`, `<instructions>`, `<parameters>`, `<constraints>`, `<output_format>`
  - More precise expectations: Verbosity, Logical Decomposition, Information Exhaustiveness, Problem Diagnosis, Precision and Completeness
  - Removed verbose S1-S4 checklist format in favor of streamlined 4-step process
  - Added constraint to never start with `<CHAR>:`

- **Tier 4 Header Updated:**
  - Added instruction that additional outputs should be interspersed within narrative
  - Added prohibition against starting or ending narration with visual elements

- **Extra Assistants:**
  - Purpose now explicitly states "addressing <USER> directly"

- **AI Roles Header:**
  - Renamed from `# ROLE` to `# SOUL.md - Who you are.`

- **Tier 0 Header:**
  - Now references `Execution Directive Hierarchy.md - Your rule book`

### Added
- **NPC Behavioural Coherence Directive (Tier 1):**
  - Ensures NPCs respond authentically to the reality their actions create
  - Handles contradictions between stated intent and behavior
  - NPCs must notice/reconcile, reveal true intent, or experience psychological break
  - Prevents holding patterns where words and actions conflict

- **README Prompt Tips:**
  - Added OOC tip: `[OOC: more details for char]` for character sheets
  - Added OOC tip: `[OOC: Give me an SVD of char]` for Structured Visual Data

### Configuration
- **Post-Processing Strict:** Added `semi_tools` (Semi-strict / Alternating roles with tools) to custom_prompt_post_processing
  - Merges user input + task steering into single instruction for tighter following
- OpenAI max tokens reduced from 32,000 to 16,000

## [2.4.0] - 2026-02-17

### Added
- **Failure Achievements Directive (Tier 4):**
  - New directive celebrating missteps and blunders with sarcastic trophy-style popups
  - Adapts to current Style State for theming
  - Triggers on: failed checks, embarrassing moments, backfired choices, social failures
  - "Roasting with affection" tone - playful acknowledgment of screw-ups
  - Maximum one per response; skips for genuinely tragic or dark moments

### Changed
- **All Assistants - Complete Overhaul:**
  - New unified layout structure for all assistants
  - Floating sidebar layout (110px fixed-width left sidebar with 90px circular avatar)
  - Support for AvatarURL field for custom images
  - Expanded build instructions for commentary, options, and footer
  - Added accessibility guidelines (minimum font sizes: 11px footer, 15px body; color contrast)
  - Custom Assistant now has structured INPUT section with Persona and AvatarURL fields

### Configuration
- Default API changed from NanoGPT to OpenRouter
- NanoGPT model updated to `zai-org/glm-5-original:thinking`
- ZAI model updated from `glm-4.7` to `glm-5`
- OpenAI max context increased from 131,072 to 200,000 tokens

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