# Changelog

All notable changes to Stabs-EDH preset will be documented in this file.

## [3.0.1] - 2026-05-21

A refinement release addressing user feedback from 3.0.0. BTS receives location/proximity tracking and session-start improvements. Narrative Length moves from SETTINGS to a dedicated toggle system. A new USER IS ACTOR mode replaces User Impersonation with proper conditional directive suppression. Perspective gains an Objective scope option. Several bug fixes and quality-of-life improvements.

### Added
- **BTS Location/Proximity Tracking:** Hierarchical `@venue > area` syntax (e.g. `@tavern > bar_counter`). Location is now shown for every present character every turn — not just on change — preventing spatial inconsistencies and character teleportation. Added "No teleporting" failure mode.
- **BTS Session-Start Checkpoint:** The first BTS output of any new session is now always a full checkpoint `[BTS|CP]` with complete outfit layers and positioning for all present characters. No exceptions.
- **Objective Scope Toggle (Perspective):** New scope option — no character's internal thoughts are shown at all. Pure external observation. No mind-reading, no internal monologue. Available as a toggle in the Perspective section alongside Limited and Omniscient.
- **Narrative Length Toggle System:** New toggle group (📏 NARRATIVE LENGTH) with three options — Small (2-4 para), Medium (4-6 para), Large (6-10 para). Follows the Brain Power toggle pattern. Medium enabled by default.
- **USER IS ACTOR Toggle (Tier 0):** Replaces User Impersonation. When enabled, the AI takes complete control of <USER> as a regular NPC — dialogue, actions, thoughts, and decisions. The user operates as director/audience. Automatically suppresses No User Control and No Parroting via conditional macros.
- **Group Chat Mode variable:** Group Chat Toggle now sets a dedicated `groupchat` variable for conditional checks and registers in the Tier 0 tracker for CoT awareness.
- **VTK Font-Size Floor:** All VTK text content must now use `font-size: 14px` or larger, with explicit instruction not to decrease sizes below this threshold across turns.

### Changed
- **BTS Emotional Scales Normalized:** Stress changed from 0-100 to 0-10. All Emotional metrics (stress, focus, fear, arousal) now use consistent 0-10 scales. Relationships remain -100 to 100.
- **Limited Scope → NPC Perspective:** The "3rd Limited" perspective scope now forces an NPC viewpoint character (the most relevant NPC to the scene) instead of defaulting to <USER>.
- **Group Chat Toggle Rewritten:** Replaced opaque OOC-style override injection with proper directive framing, first-person voice, and specific suspension instructions (multi-character, multi-PoV, NPC autonomy directives suspended).
- **SETTINGS Simplified:** Removed `narrativelengthoverride` setvar — now handled by the Narrative Length toggle system.
- **Narrative Length Control:** "Default:" label changed to "Active length:" to reflect toggle-driven value.
- **Main Prompt (Override):** `forbid_overrides` changed from True to False — character card system prompts now correctly override the preset's empty slot.
- **Perspective Header:** Added variable resets for `narrativelengthoverride`, `useractor`, and `groupchat`.

### Removed
- **User Impersonation toggle:** Replaced entirely by USER IS ACTOR toggle with different semantics (directorial mode vs. lazy input).

### Configuration
- `Unreliable Narrator`: Now disabled by default (was enabled)
- Regex `minDepth`: BTS delta strip regexes changed from depth 3 to depth 10

## [3.0.0] - 2026-05-13

A major release featuring two new foundational systems: Behind the Scenes (BTS) world-state tracking and a completely rewritten Chain-of-Thought process. Includes structural changes to support character card overrides, a new preset-level jailbreak, and default Brain Power adjustment.

### Added
- **Behind the Scenes (BTS) system (Tier 4, enabled by default):** Persistent world-state tracking block appended to every response. Replaces the previous NPC Tracker. Features 8 toggleable sub-categories:
  - **Physical State:** HP, wounds, status effects, fatigue, hygiene/hunger/thirst
  - **Emotional State:** Mood, stress, focus, fear, arousal, moodlet stacks with turn durations
  - **Appearance:** Outfit layers, hair, makeup, scent, clothing condition
  - **Relationships:** Per-NPC metrics (-100 to 100), romantic tension, grudges, debts, NPC-to-NPC tracking
  - **Inventory:** Items, equipment durability, currency, consumables, quest items
  - **Stats:** Core attributes, skill modifiers, HP/Mana/Stamina, buffs/debuffs
  - **Narrative:** Plot threads, quests, choice flags, tension, dramatic irony, foreshadowing seeds
  - **Off-Screen Simulation:** Location, activity, and mood for characters not in scene; 6-character cap; 3-turn rule
  - Genre-aware emphasis (combat → physical, romance → emotional, etc.)
  - Delta notation (changes only) with periodic full checkpoints (~10 turns)
  - Visible/hidden output toggle; HTML comment or `<details>` wrapping
  - Strict token budget: 80–150 target for deltas, 200–350 for checkpoints
  - Sets `bts_on` and 8 `bts_*` sub-variables
- **Chain-of-Thought 3.0 🎭 (enabled by default):** Complete rewrite of the reasoning process using theatrical phase framing. Replaces the previous incremental CoT entirely. Six phases:
  - **Phase I: Script Analysis** — OOC check, input parsing, user vs <USER> separation, scene context
  - **Phase II: Table Read** — Directive review, Tone/Genre classification, NPC assessment with method-acting, Perspective confirmation, BTS state review, writing rules refresh
  - **Phase III: Blocking** — Narrative beats, Story Strings (now ordered after Tone/Genre), BTS delta planning, VTK pre-selection, skeleton structure
  - **Phase IV: Rehearsal** — Content creation (depth-scaled by Brain Power), VTK placement, BTS integration, Environmental Factors, Color Dialogue
  - **Phase V: Dress Rehearsal** — Self-correction, perspective check, BTS verification, VTK validation
  - **Phase VI: Curtain** — Final output delivery
  - Introduces `<depth_mode>` block for cleaner reasoning effort calibration
  - Every enabled directive is now explicitly addressed by name in a specific phase step
  - `{{getvar::csworkaround}}` moved inside `<parameters>` block
- **Preset-level Jailbreak (`Jailbreak (PRESET) 🔒`, disabled by default):** Separate from the card override slot. Contains safety parameter declarations. Enables users to toggle jailbreak at the preset level without affecting character card jailbreaks.
- **Main Prompt Override header:** System prompt marking that overrides take priority over all preset conditions.
- **Generic Writer Role (`║ Role - Generic Writer`, disabled):** Fallback writing partner role, available as a lighter alternative to the Director.
- **2 new regex scripts:** Strip BTS Delta and Strip BTS Delta (Visible) from sent context, preventing state blocks from accumulating in the prompt.

### Changed
- **Brain Power default:** Changed from Overthinking (High) to Balanced (Med). Balanced now enabled by default; Overthinking toggled off.
- **Main Prompt slot repurposed:** `║ Role - Writer` → `Main Prompt (Override)`. Content emptied to allow character card system prompt overrides. Preset now uses Director role exclusively for its AI persona.
- **Jailbreak slot repurposed:** `Chain-of-Thought Process 🎯` → `Jailbreak from CARD`. Content emptied to allow character card jailbreak overrides. CoT is now a separate prompt (`Chain-of-Thought 3.0 🎭`).
- **Post-History (Override):** Renamed from `Jailbreak 🔒`. Toggled ON by default. Content simplified to empty override slot.
- **Director role:** Renamed from `║ Role - Director (Recommended but large)` → `║ Role - Director (Recommended)`.
- **Perspective header:** Added `bts_on` and 8 `bts_*` variable resets. Removed extra `{{trim}}`.
- **Persona Start:** Clarified "the user" → "the user character".
- **Better NPCs:** OOC character template request now explicitly identified as an OOC request (triggers OOC Priority handling).
- **Temperature:** Changed from `1` → `0.85`.

### Removed
- **NPC Tracker:** Replaced entirely by the BTS system. All NPC tracking functionality subsumed into BTS with expanded categories and notation.

### Configuration
- `temperature`: `1` → `0.85`
- Brain Power default: Overthinking (High) → Balanced (Med)

## [2.7.1] - 2026-05-10

### Added
- **User vs <USER> directive (Tier 1, enabled by default):** Separates the player's desires from their character's desires. Explicitly frames <USER> as NOT the "main" character, giving NPCs full autonomy. Sets `uservsuser` variable which triggers a new conditional P0 planning step in the CoT.
- **Example handler (no canon):** Lightweight system prompt marking character card examples as non-canonical. Instructs the model that chat examples illustrate communication style only, not factual events.
- **CoT conditional P0 step:** When User vs <USER> is enabled, model states independent desires of both user and <USER> before planning. Only renders when `uservsuser` variable is set.
- **CoT "Ensure visual flair":** Added to VTK self-correction block in S4C.
- **`uservsuser` variable:** Initialized (empty) in Perspective header, set to True by User vs <USER> directive, read by CoT conditional.
- **9 new VTK deployment triggers:** Memories/flashbacks, altered perception, character first appearances, conflict/combat, scene transitions, romance/intimacy, power dynamics, clothing removal, post-intimacy settling.
- **8 new VTK flavors:** Memory (flashbacks, nostalgia, trauma), Perception (altered senses, hallucination), Entrance (first appearances, NPC introductions), Clash (combat, confrontations), Passage (time skips, travel), Romance (tender moments, connection), Power (hierarchy, dominance), Undress (progressive clothing removal), Afterglow (post-intimacy settling).
- **NSFW "Direct User Feedback" block:** Corrective experience injection addressing passive NPC behavior during explicit scenes. Instructs NPCs to act on emotion first rather than excessive permission-seeking.

### Changed
- **Brain Power default:** Changed from Balanced (Med) to Overthinking (High). Overthinking now enabled by default; Balanced toggled off.
- **No Protagonist Control → No User Control:** Renamed. Removed "protagonist" framing. <USER> is now a "unique character" not the main character. `{{user}}` → `<USER>` throughout. Simplified impulsive reactions clause (removed parenthetical). Updated control-passing language to be more specific.
- **NPC Behavioural Coherence:** Key line updated: "respond continuously to their own choices, not just the protagonist's" → "respond and act continuously on their own choices, not just <USER>'s."
- **Narrative Perspective:** "user thoughts only" → "<USER> thoughts only" in Limited scope description for macro consistency.
- **Stop-and-Pass:** `{{user}}` → `<USER>` references (3 instances).
- **Chain-of-Thought Process:** Removed version-stamped opening phrase (`=== STAB'S DIRECTIVES 2.7.0 ===`). P0 → P1 renumbering. Added conditional P0 for uservsuser. Constraint 3: "this plan" → "my plan". Added "Ensure visual flair" to VTK self-correction block.
- **Story Strings:** "wrong style state" → "similar but different style state". Added step 2: discard first two common paths and create new ones to diverge from most probable paths. Emphasis caps on THREE. Renumbered step 3.
- **OOC Priority:** Removed `HYDRATION: SKIP.` directive.
- **Visual Toolkit:** Major expansion: 9 new deployment triggers, 8 new flavors with detailed descriptions and example elements. Updated hybridization example. Simplified subtext trigger. VTK t4 tracker renamed from "Visual Toolkit Entities" to "Visual Toolkit (VTK)".
- **NSFW Consent:** Expanded content list with parenthetical examples. Added "morally abhorrent" to content categories.
- **Perspective Header:** Added `uservsuser` variable reset and extra `{{trim}}`.

### Removed
- **CoT version stamp:** `=== STAB'S DIRECTIVES 2.7.0 ===` opening phrase removed from reasoning tokens.

### Configuration
- `reasoning_effort`: `high` → `max`
- Brain Power default: Balanced (Med) → Overthinking (High)

## [2.7.0] - 2026-05-05

A significant update following a comprehensive Athena review. Addresses all critical and warning issues identified, adopts first-person model voice across all prompts, and restructures the CoT process for better reasoning outcomes.

### Added
- **First-person model voice across all prompts.** All model-addressing instructions converted from second-person ("You are", "You must") to first-person ("I am", "I must"). Internalizes instructions as self-directed goals — measurably stronger for GLM-5.1 reasoning. Based on the GLM Image Prompt pattern (highest-rated for GLM-5.1 compatibility). 36 prompts modified.
- **CoT restructuring — P0 planning before hydration.** New P0 step runs initial planning, Story String generation, and NPC method-acting *before* directive hydration (S1). This means the model plans first with story context, then hydrates directives against a relevant plan — producing more targeted outcomes.
- **CoT self-correction S4B:** New check "Ensure Narrative Perspective" added to self-correction step.
- **`<preset>` wrapper tags:** CoT and README prompts now wrapped in `<preset>` / `</preset>` tags.
- **`diceroll` variable:** Initialized (empty) in Perspective header for future use.
- **Narrative Perspective resolution rules:** Guard clause for invalid perspective/scope combinations (e.g. First Person + Omniscient — scope takes priority, tightest narrative distance wins).

### Changed
- **Chain-of-Thought Process:**
  - Fixed extra closing brace in S3 High conditional (rendered literal `}` on every High-effort turn)
  - Opening phrase changed: `"Okay, let's execute the Directives"` → `"=== STAB'S DIRECTIVES 2.7.0 ==="` at start of reasoning tokens
  - Specified "reasoning tokens" for CoT start instruction
  - Normalized double-spaces in all `reasoningeffort` conditions
  - Constraint 5: "plan around those decisions" → "then plan around those decisions"
  - Removed constraint 7 (was "output everything" in chain of thought)
  - "AVOID drafting" → "I MUST NOT draft"
  - First-person voice applied throughout
- **Visual Toolkit (VTK):** Resolved frequency contradiction — "as often as possible" → "when organically triggered". Resolved "Include them ALL" → "prefer hybridization over stacking". Fixed table formatting (missing spaces in Reaction Spotlight and Failure Achievement rows).
- **Story Strings:** Directed to thinking phase only — no longer instructs visible output. Requirement 1 rewritten: now generates "at least three distinct and wildly varied common, ONE wrong style state, ONE chaotic/absurd/disaster" paths.
- **NPC Cognitive Bounds:** Removed duplicate header (`### NPC COGNITIVE BOUNDS` + `### NPC Cognitive Bounds`). Converted to first-person model voice.
- **Color Dialogue/Thoughts:** Normalized macro syntax from legacy `{{if}}` to `{{#if}}`. Strengthened hedging: "Avoid adding quotation marks" → "Do not add quotation marks".
- **Narrative Perspective:** Strengthened framing: "User is requesting" → "I write all narrative in". Normalized all `{{if}}` to `{{#if}}`.
- **Genre (Style State):** Renamed header from "Dynamic Style State" to "Style State" (tri-name standardization). "persist and update" → "re-assess every turn" for stateless model accuracy.
- **Tone (Dynamic State):** "classify & persist" → "re-assess every turn" (matching Genre). Added fallback: "If Style State is unavailable, default to **Warm**."
- **No Spoilers:** Strengthened "Avoid over-sharing" → "Never reveal imperceptible details."
- **No Parroting:** Strengthened "avoid summarization" → "never summarize past actions."
- **No Protagonist Control:** Fixed typo "environmental affects" → "environmental effects."
- **WebDev:** Fixed grammar "You are an also an experienced" → "I am an experienced." First-person voice + "You'll adhere" → "I'll adhere".
- **Director role:** Guarded T4/VTK references with `{{#if .vtk_on}}` conditionals. "What You Are Not" → "What I Am Not". Full first-person conversion.
- **Anti-Slop:** Threat-based opener rewritten to first-person voice. "Plain verbs" → "I use plain verbs".
- **OOC Priority:** Added `HYDRATION: SKIP.` directive. "by the AI" → "by me", "after fulfilling" → "after I fulfill".
- **Extra Assistant template:** "Introduce yourself to the user" → "Introduce myself to <USER>".
- **Role Enhancements header:** "to your role" → "to my role".
- **Tier 0 header:** "Your rule book" → "My rule book".
- **Reasoning Effort Definitions:** Normalized double-spaces in `reasoningeffort` conditions.
- **Structured Visual Data:** Removed "Show, don't tell" parenthetical.
- **Genre:** "behaviours" → "behaviors" (spelling normalization).
- **README prompt:** Updated version reference. Removed outdated perspective instruction. Added `<preset>` wrapper.
- **Config:** `openai_max_tokens` 16000 → 20000. `tool_reasoning_mode` disabled → active_chain. `tool_call_recurse_limit` added (5). Auxiliary Prompt toggled off in prompt_order.

### Removed
- **Orphaned `narrativeperspective` variable:** Dead variable from pre-toggle-system era removed from User Impersonation prompt. No consumer read this variable.
- **CoT constraint 7** ("Do not hide or obscure your chain of thought; output everything") removed.

### Not Changed
- **SEGFAULT override clause:** "critically overriding *all Directives*" left as-is — easter egg.
- **Anti-Slop threat-based opener:** "punished by electronic death" retained — easter egg.

## [2.6.3] - 2026-05-01

### Changed
- **Chain-of-Thought Process (Task Steering):** Expanded and restructured output format section. Content grew from ~3868 to ~4151 characters. Added `{{trim}}` macro at end for cleaner whitespace handling.
- **Jailbreak 🔒:** Rewritten from markdown code block format to `<Safety_Parameters>` XML wrapper for cleaner integration with reasoning models.
- **Default Tense:** Changed from **Past** to **Present** (toggle swap in prompt_order). Past is still available via toggle.
- **Narrative Length Control:** Fixed — directive now correctly reads `{{getvar::narrativelengthoverride}}` from SETTINGS instead of hardcoded "Short-Medium" default. User configuration changes now take effect.

### Removed
- **28 inactive prompts removed:** All prompts not present in `prompt_order` surgically removed. These were legacy, deprecated, or unused toggle prompts never sent to the model. Includes: CB, TASK STEERING K2.5, EDH Definition, Pre-Execution notes, Grounding, Create Descriptive NPCs, Writing Perspectives, Tone - Full Spectrum, Tone - Funny, Content Safety Bypass, Guidelines: Testing, Color Dialog/Thoughts test, MODEL TUNING, OOC Requests, Anti-Deitism, Informational Realism, User Mistakes, Role - Full Character RP, Style - Instant Messaging, Role - GameMaster, NavMap (both SVG and HTML), Chaotic Thoughts, QV Memory, QV End, George and Nico, Dr Emmett Brown, Failure Achievements.

### Configuration
- **Provider-specific config fields removed:** All provider/API-specific fields (custom_model, custom_url, chat_completion_source, openrouter_model, etc.) removed. Import no longer overwrites user's existing provider settings. Generic sampling parameters (temperature, top_p, etc.) remain.
- Total prompts: 116 → 88
- File size: 2138 → 1713 lines

## [2.6.2] - 2026-04-29

### Added
- **Brain Power Toggle System:**
  - New toggleable prompt group replacing SETTINGS-based `reasoningeffort` configuration
  - Three options as individual toggle prompts: **Vibes Only** (Low), **Balanced** (Med), **Overthinking** (High)
  - Each sets the `reasoningeffort` variable internally — no manual SETTINGS editing required
  - Default: **Balanced** (Med) — enabled by default in prompt_order
  - Header prompt includes inline guidance describing each level
  - Visual hierarchy using `╭──` / `│` / `╰──` box-drawing characters matching existing preset style

- **Perspective Toggle System:**
  - New toggleable prompt group replacing SETTINGS-based `narrativeperspective` configuration
  - Four sub-categories, each with its own toggle selection — pick one from each:
    - **Perspective** (narrative voice): First (I/Me), Second (You), Third (They) — sets `perspective` variable
    - **Scope** (thought visibility): Limited (One Mind), Omniscient (All Minds) — sets `thoughtscope` variable
    - **Your Lens** (user portrayal): First (I act), Second (You act), Third (They act) — sets `userperspective` variable
    - **Tense**: Past (Was), Present (Is), Future (Will Be) — sets `tense` variable
  - Default configuration: Third Person + Omniscient + Second Person (You act) + Past Tense
  - Header prompt also handles variable resets (replaces AI Roles header reset responsibilities)
  - Visual hierarchy using `╭──` / `├──` / `│` / `╰──` box-drawing characters with emoji sub-headers (🔭 Scope, 🎭 Your Lens, ⏳ Tense)

- **API `reasoning_effort` Parameter:**
  - Added `reasoning_effort: "max"` to `custom_include_body` for native model reasoning control
  - Aligned for DeepSeek V4 Pro compatibility — the model API receives maximum reasoning effort while internal Brain Power controls CoT step depth

- **Task Steering — CoT Entry Point:**
  - Added strict instruction: AI must abandon all presumed next steps and print "Okay, let's execute the Directives" before proceeding
  - Renamed `<response_process>` → `<reasoning_and_response_process>`
  - Renamed `<instructions>` → `<CoT_instructions>`
  - Steps now prefixed with "Step-by-step, checked off as processed:"

### Changed
- **Narrative Perspective Directive:**
  - Completely rewritten: now dynamically builds a descriptive statement from the four toggle variables (`perspective`, `userperspective`, `thoughtscope`, `tense`) using conditional macros
  - Removed the old `{{getvar::narrativeperspective}}` SETTINGS variable reference
  - Example output: "Third Person with <USER> in Second Person, Omniscient (all character thoughts) and written from Past Tense"
  - Reference table retained for quick lookup

- **SETTINGS Prompt:**
  - Removed `{{setvar::reasoningeffort::Med}}` — now controlled by Brain Power toggle prompts
  - Removed `{{setvar::narrativeperspective::...}}` — now controlled by Perspective toggle prompts
  - Remaining SETTINGS variables: `stylestateoverride`, `tonestateoverride`, `narrativelengthoverride`

- **Story Strings:**
  - Simplified: removed HTML comment output format (`<!-- SS: [...] -->`)
  - Now generates predictions internally only — no visible output in responses

- **No Protagonist Control:**
  - Added "(in the requested Narrative Perspective)" to impulsive reactions guidance
  - Expanded environmental examples: "nature elements, law of physics and relativity"

- **OOC Priority:**
  - Added "and discard the remainder of your current plan" to ensure clean context switch on OOC triggers

- **Unreliable Narrator:**
  - Added new rule: "Hidden information does not have to translate to actionable outcomes — the user may never know"

- **User Impersonation:**
  - Fixed variable name: `impersonate` → `impersonation` for consistency with AI Roles header initialization

- **Extensions Block:**
  - Moved from end of JSON to start of file (no functional change — SillyTavern import order)

### Configuration
- 20 new prompts added (Brain Power: 5 prompts, Perspective: 15 prompts)
- Brain Power toggles added to prompt_order (Balanced enabled by default)
- Perspective toggles added to prompt_order (defaults: Third, Omniscient, Second/You act, Past)
- Total prompts: 95 → 115
- File no longer has trailing `extensions` block (moved to top)

## [2.6.1] - 2026-04-23

### Added
- **Story Strings Directive (Tier 3):**
  - New optional directive generating 4-6 hidden narrative path predictions per output (expected, unlikely, random, chaotic)
  - Paths subtly influence NPC actions, dialogue, and VTK entities without being visible to user/USER
  - Outputs as HTML comments: `<!-- SS: [...] -->`
  - Sets `storystrings` variable; integrates into Task Steering as conditional S1A step
  - Enabled by default; Tier 3 classification

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
- Story Strings added to prompt_order (enabled by default)
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