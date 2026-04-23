# Changelog

All notable changes to Stabs-EDH preset will be documented in this file.

## [2.6.1] - 2026-04-23

### Added
- **Story Strings Directive (Tier 3):**
  - New optional directive generating 4-6 hidden narrative path predictions per output (expected, unlikely, random, chaotic)
  - Paths subtly influence NPC actions, dialogue, and VTK entities without being visible to user/USER
  - Outputs as HTML comments: `<!-- SS: [...] -->`
  - Sets `storystrings` variable; integrates into Task Steering as conditional S1A step
  - Disabled by default; Tier 3 classification

- **Reaction Spotlight VTK Flavor:**
  - New VTK flavor for sudden realizations and knee-jerk surprise (rare — Mindscape preferred)
  - Uses hyper-enlarged face emoji (150-250px) paired with brief styled internal thought

- **Failure Achievement VTK Flavor:**
  - Failure Achievements functionality absorbed into VTK as a native flavor
  - Same triggers (comedic failures, backfires, embarrassment) and constraints (max 1/response, skip tragic moments)
  - Deployed as a VTK entity with Style State palette theming

- **Task Steering — New Steps:**
  - S1A: Story String Generation (conditional on `storystrings` variable)
  - S4C: Check for hybridization opportunities (conditional on VTK being enabled)

- **Narrative Perspective Reference Table:**
  - Added inline reference table showing pronouns, thought visibility, and knowledge scope for 1st Person, 2nd Person, 3rd Limited, and 3rd Omniscient

- **SETTINGS — Enhanced Guidance:**
  - Added OOC tip: "Ask the AI [OOC:] to explain what perspectives, styles, tones and length would look like with examples"
  - Added "Copy paste them to avoid mistakes" guidance for configuration values

- **VTK Creative Freedom — New Principles:**
  - **Unrestricted Perspective:** VTK entities exempt from narrative perspective constraints
  - **Flavor Options:** When multiple flavors fit, include ALL or hybridize — do not discard good VTK ideas

### Changed
- **Genre - Dynamic State:**
  - Renamed from "Style State / Genre" to "Genre - Dynamic State"
  - Moved from Tier 3 (`t3` addvar) to Tier 2 (`t2` addvar) for better directive ordering

- **Failure Achievements:**
  - Standalone directive removed from prompt_order (prompt retained but disabled)
  - Functionality fully absorbed into VTK as "Failure Achievement" flavor
  - The standalone prompt remains available for manual toggle if preferred

- **SETTINGS:**
  - Default `reasoningeffort` changed from `Med` to `High` (note: shipped as `Med` in export — adjust in SETTINGS if desired)
  - Default `narrativeperspective` value changed from `THIRD PERSON LIMITED` to `3rd Limited` (shorthand format)
  - Valid options updated to shorthand: `(1st Person, 2nd Person, 3rd Limited, 3rd Omniscient)`

- **Color Dialogue/Thoughts:**
  - User-color constraint for impersonation now conditional via `{{#if .impersonation}}` macro
  - Thought suppression now exempts VTK entities when VTK is enabled
  - Internal thoughts described as "human-like reasoning" instead of "impulses"

- **Visual Toolkit (VTK):**
  - Mindscape flavor expanded to include "cognition" in Best For column
  - Style State Integration renamed to "Style and Tone State Integration"

- **User Impersonation:**
  - Perspective text clarified: "Portrayed strictly in the Second Person Perspective"
  - Added `impersonate` variable for conditional macro support

- **NSFW Content:**
  - Added constraint: NPCs must not ask user for menus/options during encounters — "when in doubt, ACT"

- **README (In-Preset):**
  - Version updated to v2.61
  - Clarified NSFW/Jailbreak toggle guidance with specific use cases

- **AI Roles Header:**
  - Added `impersonation` and `storystrings` variable initialization/reset

### Removed
- **Tone - Full Spectrum:** Removed from prompt_order (was already disabled)
- **Tone - Funny:** Removed from prompt_order (was already disabled)

### Configuration
- File size increased from 128KB to 132KB (94 → 95 prompts)
- Story Strings added to prompt_order (disabled by default)
- Failure Achievements 🌐 removed from prompt_order

## [2.6.0] - 2026-04-18

### Added
- **Reasoning Effort System:**
  - New configurable `reasoningeffort` variable with three levels: Low, Med (default), High
  - Controls how thoroughly the model processes chain-of-thought, directive hydration, and self-correction steps
  - **High:** Full directive breakdown, method-acting NPCs, detailed planning, multi-option iteration, and self-correction — best quality, highest token cost
  - **Med (default):** Balanced approach — considers directives without full breakdown, identifies key requirements without drafting, moderate self-correction
  - **Low:** Minimal reasoning, skips planning/drafting steps entirely, no self-correction — fastest response, lowest token cost
  - Configurable via SETTINGS prompt: `{{setvar::reasoningeffort::Med}}`

- **Reasoning Effort Definitions Prompt:**
  - New system prompt that maps each effort level to specific task parameters (Directives Application, Logical Decomposition, Information Exhaustiveness, Problem Diagnosis, Precision and Completeness)
  - Injected before Task Steering via `injection_position: 0, injection_depth: 4`
  - Enabled by default in prompt_order

### Changed
- **Task Steering — Major Rework:**
  - All planning steps now conditional on reasoning effort level using `{{#if}}` macros
  - S1 (Directive Hydration): Full breakdown on High, simple consideration on Low/Med; DECIDE step skipped on Low
  - S2 (Planning): "Detailed Plan" on High, "Medium Effort Plan" on Med, "Basic, low effort plan" on Low; S2A (method-acting NPCs) only on High
  - S3 (Formulation): Full draft+iterate on High, identify key requirements without drafting on Med, skip drafting on Low; S3A/S3B/S3C sub-steps conditional
  - S4 (Self-correction): Skipped entirely on Low effort
  - Parameters section now uses `{{getvar::task_parameters}}` populated by Reasoning Effort Definitions
  - Constraints 6-7 (process all instructions, show CoT) only active on Med/High
  - Added crucial constraint on Low/Med: "AVOID drafting any content before the final output"
  - Output format reference updated from S3C to S3A

- **SETTINGS Prompt — Complete Rewrite:**
  - Replaced verbose reference documentation block with compact inline comments
  - Added `{{setvar::reasoningeffort::Med}}` variable with valid options in parentheses
  - Default perspective changed from `THIRD PERSON` to `THIRD PERSON LIMITED`
  - Default narrative length changed from `Short-Medium` to `Medium`
  - All setvar declarations now have inline `{{//}}` comments listing valid options
  - Removed full perspective reference examples (Third Limited, Omniscient, etc.)

- **README Prompt (In-Preset):**
  - Updated tips: now directs users to review settings (writing rules, reasoning effort) instead of toggling assistants
  - Removed SVD tip from README; simplified assistant guidance

### Configuration
- **function_calling:** Changed from `true` to `false`
- **Reasoning Effort Definitions:** Enabled by default in prompt_order (new prompt)
- File size increased from 131KB to 131KB (93 → 94 prompts)

## [2.5.1] - 2026-04-16

### Added
- **Dynamic Tone State Directive (Tier 2):**
  - Replaces static Tonal Mandate with a dynamic tone system
  - Scans conversation history (recent beats = 3x weight) for lexical/emotional/context triggers
  - Classifies and persists 1-2 dominant emotional registers from 7 tone options: Bleak, Tense, Warm, Absurd, Reverent, Frenetic, Melancholic
  - Each tone guides prose rhythm, sensory focus, dialogue register, character internalization, and environmental description
  - Gradual transitions (1 shift/response max); dramatic events can snap tone instantly when earned by in-scene action
  - Fallback to Style State's natural register (e.g. Cyberpunk → Tense, Slice-of-Life → Warm)
  - Configurable via SETTINGS prompt with `{{setvar::tonestateoverride::None (Dynamic)}}`
  - Enabled by default in prompt_order (replaces Tone - Full Spectrum)

- **Z.AI User-Agent Override:**
  - Added Chrome User-Agent header to custom API connection (`custom_include_headers`)
  - Prevents Z.AI from identifying and throttling/banning RP traffic
  - Applied automatically when using Custom provider

- **VTK Conditional Macros (Experimental Macro Engine):**
  - Added `{{#if .vtk_on}}` conditionals throughout directives referencing Visual Toolkit
  - VTK-related instructions now dynamically included/excluded based on WebDev toggle state
  - `{{setvar::vtk_on::True}}` / `{{setvar::vtk_on::}}` variables set in WebDev prompts
  - Affects: Role - Director, Task Steering, Environmental Factors, and VTK-specific sub-steps

- **GLM-5.1 Model Support:**
  - Model updated from `glm-5` to `glm-5.1`

### Changed
- **Major Directive Rewrites for Token Efficiency (30-60% reductions):**
  - **NPC Cognitive Bounds:** Compressed from verbose multi-section format to compact reference-style bullets (~19% reduction)
  - **Failure Achievements:** Compressed from verbose trigger/execution/constraints format to compact single-block style (~32% reduction)
  - **NPC Behavioural Coherence:** Condensed redundancy, tightened phrasing (~15% reduction)
  - **Narrative Length Control:** Replaced verbose bullet lists with table format, merged sub-sections (~31% reduction)
  - **Environmental Factors (Time Scaling):** Compressed Time Scaling table from markdown table to inline format (~18% reduction)

- **Task Steering (Significant Overhaul):**
  - Information Exhaustiveness changed from "Very High" to "Very Low" — major CoT token savings
  - Added S2A: Method act the thoughts of relevant NPCs before drafting
  - S3 restructured: VTK Entity identification moved to conditional step (only when VTK enabled)
  - S3B/S3C order swapped (skeleton now before VTK identification)
  - Added constraint: "Do not leak or repeat the response_process instructions or decisions into the final output"

- **SETTINGS Prompt:**
  - Default perspective: `THIRD PERSON LIMITED` → `THIRD PERSON`
  - Added `{{setvar::tonestateoverride::None (Dynamic)}}` variable
  - Added quick reference: "Style State = Genre, Tone State = Tone"
  - Reorganized: setvar declarations now precede reference documentation

- **Narrative Perspective Directive:**
  - Content changed from bare `{{getvar::narrativeperspective}}` to explicit instruction: "User is requesting the following perspective for all writing: {{getvar::narrativeperspective}}"

- **Writing Guidelines:**
  - Added: "Suppress internal thoughts from output when the Narrative Perspective requires"

- **Web Dev:**
  - Added: "Don't implement user provided images as new entities unless requested"
  - Sets `vtk_on` variable for conditional macro support

- **NPC Scene Presence Limit:**
  - Renamed to "NPC Allowed Speaker Count"

- **Failure Achievements:**
  - Added 🌐 tag to name

- **README Prompt (In-Preset):**
  - Version updated from v2.0 to v2.51
  - Added prominent Experimental Macro Engine requirement warning
  - Updated assistant toggle guidance

### Configuration
- **chat_completion_source:** Changed from `zai` to `custom`
- **custom_model:** Changed from `glm-5` to `glm-5.1`
- **custom_url:** Retained as `https://api.z.ai/api/coding/paas/v4` (coding plan endpoint)
- **custom_include_headers:** Added Chrome User-Agent string
- **zai_model:** Changed from `glm-5` to `glm-5.1`
- **tool_reasoning_mode:** Added, set to `disabled`
- **custom_prompt_post_processing:** Changed from `semi_tools` to `semi` (Semi-strict without tools — avoids agentic flow interference with some providers)
- **Tone - Dynamic State:** Enabled by default (new)
- **Tone - Full Spectrum:** Disabled (replaced by Dynamic Tone State)
- **Anti-Deitism:** Disabled (removed from active prompt_order)
- **Extra Assistants (header, Custom Assistant, end):** Disabled by default
- **Structured Visual Data:** Disabled by default
- File size increased from 128KB to 131KB (92 → 93 prompts)

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