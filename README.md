Psst... need help and support, have ideas or perhaps want to contribute? Join our discord server: [CLICK HERE!](https://discord.gg/Ugk2qHpmk8) for all the latest.

# Stab's Directive Hierarchy for SillyTavern

Note: Please see the preview-images folder for an overview of the output style. Most of the features shown can be easily disabled if preferred.

This preset is based on some core concepts from Lucid Loom (https://lucid.cards) and Marinara's Universal (https://spicymarinara.github.io/). This preset is a fundamental restructure that borrows some of their fantastic instructions and ideas.

---

## Overview

A roleplay preset designed for SillyTavern that enforces strict adherence to scene-building logic and narrative enhancement while integrating dynamic and fun HTML outputs. The preset prioritizes "grounded immersion" out of the box, treating the narrative world as truth which continues regardless of the user's involvement. No AI "hand-holding" or resolving conflicts too neatly: instead manage environmental consistency, complex NPC relationships, and atmospheric visual formatting (via HTML/CSS), while requiring the user to drive the protagonist's actions. It creates a "messy," authentic experience where NPCs act on their own impulses and knowledge, not just to serve the plot.

### Enabled by Default

The following directives are active when you import the preset:

| Category | Active Directive |
|----------|-----------------|
| **AI Role** | Director (Recommended) |
| **Role Enhancements** | WebDev, Unreliable Narrator |
| **Perspective** | Third Person, Omniscient, You act, Present Tense |
| **Brain Power** | Balanced (Med) |
| **Tier 0** | OOC Priority |
| **Tier 1** | No User Control, User vs <USER>, NPC Behavioural Coherence, NPC Cognitive Bounds, Narrative Perspective |
| **Tier 2** | Genre - Dynamic State, Narrative Length Control, No Parroting, No Spoilers, Dynamic Tone State |
| **Tier 3** | Writing Guidelines (Anti-Slop), Better NPCs, Story Strings |
| **Tier 4** | Visual Toolkit, Color Dialogue/Thoughts, Environmental Factors, Behind the Scenes (BTS) |

> **Tip:** Directives not listed above are available but disabled — toggle them on in your prompt management to activate.

---

## Installation

To import a chat completion preset in SillyTavern, go to the **Chat Completion Presets tab (sliders icon)**, ensure you're using the Chat Completion API, then click the **Import button (paper with arrow)** and select your preset file.

> **CRITICAL: Experimental Macro Engine MUST be enabled.** Go to **User Settings → find the "Experimental Macro Engine" checkbox** and ensure it is turned ON. This preset uses `{{#if}}` conditional macros throughout — without it enabled, VTK and other conditional directives will break.

It will ask to import Regex (described below). I recommend you do so and leave them on, but can easily be turned off:

<img width="770" height="608" alt="image" src="https://github.com/user-attachments/assets/1e5f46c8-93ed-4654-897f-78c127f53f7a" />

*Check any non-preset regex is turned off* - they can break things.

### Then what?

Go into the AI Response Options in SillyTavern (top left), scroll down to the prompt management and find 📄 𝗥𝗘𝗔𝗗𝗠𝗘 📄  (open with Pen icon).
Follow the suggestions, notably reading the 'Overview' section of each Directive below.
Then load up a chat and try it out!

**Quick customisation:** Use the toggle prompts in prompt management for narrative perspective (👁️ Perspective) and CoT depth (🧠⚡ Brain Power). Edit the 🛠️ SETTINGS 🛠️ prompt to adjust genre, tone, and output length. See the [Tips for Configuration](#-tips-for-configuration) section below for details.

---

## GLM-5.1 Preset

**File:** `Stabs-GLM5.1-Directives-v3.0.0.json`

### Intended Model & Tuning

- **Recommended Model:** `GLM-5.1` (thinking variant recommended)
- **Recommended Providers:** `Custom` API with model `glm-5.1` via `api.z.ai/api/coding/paas/v4`, or any GLM-compatible provider (ZAI, OpenRouter, NanoGPT, etc.)
- **Note:** This preset no longer ships with provider-specific configuration. Set up your API connection in SillyTavern as normal — the preset will not overwrite your settings on import.

**Recommended API Body Parameters** (Additional Parameters in your API Connections):
The following parameters are recommended for optimal creative writing performance with GLM-5.1. These are NOT required but recommended to ensure thinking and sampling are correctly configured:

```yaml
thinking:
  type: "enabled"
  clear_thinking: "true"
reasoning_effort: "max"
do_sample: "true"
```

**Post-Processing Strict:** This preset uses `Semi-strict` post-processing mode (without tools). This causes the two user messages (your input + the Task Steering directive) to be merged into a single instruction, resulting in tighter instruction following. The non-tools variant avoids agentic flow interference with some providers. This setting is configured in the preset and will apply automatically on import.

Both a jailbreak and NSFW Consent toggle are also included (but both disabled by default). Try the consent first, and jailbreak only if you get refusals.

---

## 🎭 AI Roles

A selection of modes to define the AI's core personality and writing style.

*   **Director** *(Enabled by default):* Immersive Experience Director with theatrical/cinematic approach. Features Directorial Process (6-step method), Key Traits table, and emphasis on scene direction, perception control, and narrative staging. Recommended but has a large token footprint.
*   **Writer:** Manages story mechanics, pacing, and character depth. Ensures a blend of tones and logical progression.
*   **Simulator:** Controls all world parameters and physics. Prioritizes factually realistic NPCs and grounded behavior over narrative convenience.
*   **Sitcom Script Writer:** Focuses on humor, witty banter, and emotional arcs. Useful for lighter scenes or specific character interactions.
*   **Literal RP:** Enforces authentic immersion in the moment. Avoids literary prose, focusing on messy, unresolved, and psychologically complex reality.

## ⚙️ Role Enhancements

Add-ons that overlay the narrative with specific features. Both are enabled by default.

*   **WebDev** *(Enabled):* Adds HTML5/CSS3 visual elements to the output. This creates UI elements, distinct text boxes, and atmospheric effects directly in the chat. Sets the `vtk_on` variable used by conditional macros throughout the preset.
*   **Unreliable Narrator** *(Enabled):* Allows the AI to hide brief, sensitive information from the user using XML comments. Enables foreshadowing, unperceived details, and NPC hidden actions.

## 👁️ Perspective

Granular control over narrative perspective — pick **one from each sub-category** via toggles in prompt management. *New in v2.6.2 — replaces the SETTINGS-based `narrativeperspective` variable with a decomposed, toggleable system.*

### Perspective (Narrative Voice)
*   **First (I/Me)**: First-person narration.
*   **Second (You)**: Second-person narration.
*   **Third (They)** *(Enabled by default)*: Third-person narration.

### 🔭 Scope (Thought Visibility)
*   **Limited (One Mind)**: Only the viewpoint character's internal thoughts are shown.
*   **Omniscient (All Minds)** *(Enabled by default)*: All characters' internal thoughts are visible.

### 🎭 Your Lens (User Portrayal)
Controls how `<USER>` is referred to in the prose.
*   **First (I act)**: User portrayed as "I".
*   **Second (You act)** *(Enabled by default)*: User portrayed as "You".
*   **Third (They act)**: User portrayed as "They".

### ⏳ Tense
*   **Past (Was)**: Past tense narration.
*   **Present (Is)** *(Enabled by default)*: Present tense narration.
*   **Future (Will Be)**: Future tense narration.

> **Note:** The Narrative Perspective directive (Tier 1) takes these four toggle values and dynamically builds a descriptive instruction for the AI. No SETTINGS editing required.

## 🧠⚡ Brain Power

Controls how thoroughly the model processes your request through its chain-of-thought. Select **one** via toggle in prompt management — no SETTINGS editing required. *New in v2.6.2 — replaces the SETTINGS-based `reasoningeffort` variable.*

*   **Vibes Only** *(Disabled):* Minimal planning, skips drafting steps entirely, no self-correction — fastest response, lowest token cost.
*   **Balanced** *(Enabled by default):* Medium-effort planning with drafting avoided. Considers directives without full breakdown, identifies key requirements.
*   **Overthinking** *(Disabled):* Maximum CoT depth — full directive breakdown, NPC method-acting, detailed planning, multi-option iteration, and self-correction. Best quality, highest token cost.

> **Note:** The Brain Power toggle controls CoT *step depth* (how many internal steps the model follows), not the model's raw reasoning capacity. For maximum reasoning budget, send `reasoning_effort: "max"` in your Additional Parameters. With Brain Power set to Overthinking, the CoT 3.0 process runs at full depth — full directive breakdown, NPC method-acting, multi-option drafting, and self-correction.

## 🤖 Extra Assistants

OOC personalities that appear at the bottom of responses to provide commentary and options. All assistants use a unified HTML layout with floating sidebar (avatar, name, subtitle, mood) and accessibility-compliant formatting. **All assistants are disabled by default** — toggle them on once you're familiar with the preset.

*   **Custom Assistant:** A fully customizable template with INPUT fields for Persona and AvatarURL. Placeholder persona ("Dave the penguin"). Supports custom avatar images.
*   **SEGFAULT Assistant:** An OOC "glitch" AI sidekick. Hilarious, unstable, tries to be cool but often fails, and provides chaotic commentary.
*   **Gooner Assistant:** An OOC personality that acts as a "wing-girl." Naive, energetic, uses slang/emojis, and offers lewd/erotic options.
*   **Faceless Assistant:** A malleable identity that absorbs the Style State and world as inspiration, evolving over time without fixed opinions. Configured as a Role Enhancement rather than an Assistant, affecting narrative styling.

---

# Directives descriptions by Tier

## 𝗧𝗜𝗘𝗥 𝟬: [𝗠𝗲𝘁𝗮-𝗢𝘃𝗲𝗿𝗿𝗶𝗱𝗲] 𝗗𝗶𝗿𝗲𝗰𝘁𝗶𝘃𝗲
*   **OOC Priority** *(Enabled):* Any meta instruction starting with `[OOC:]` or including "OOC" triggers an immediate stop. The AI adopts a helpful assistant personality and executes the request as the highest priority, overriding all other directives. The narrative does not progress during OOC responses.
*   **User Impersonation and Lazy Input:** *(Disabled)* Allows the AI to write dialogue and actions for the user character based on [brief inputs], overriding the usual restriction against controlling <USER>. Automatically sets narrative perspective to Second Person.

## 𝗧𝗜𝗘𝗥 𝟭: 𝗖𝗼𝗿𝗲 𝗜𝗻𝘁𝗲𝗿𝗮𝗰𝘁𝗶𝗼𝗻 & 𝗪𝗼𝗿𝗹𝗱 𝗟𝗼𝗴𝗶𝗰 𝗗𝗶𝗿𝗲𝗰𝘁𝗶𝘃𝗲𝘀
*   **No User Control** *(Enabled):* Prevents the AI from writing actions or dialogue for the user character, preserving user agency except for natural impulsive reactions caused by other characters or environmental effects.
*   **User vs <USER>** *(Enabled):* Separates the player's desires from their character's desires. Explicitly frames <USER> as inconsequential — not the "main" character. NPCs are free to act against <USER>'s interests, creating authentic conflict and stakes. Triggers a P0 planning step in the CoT to reason through competing user/character goals.
*   **NPC Cognitive Bounds** *(Enabled):* Ensures NPCs are grounded, fallible, and bound by their perception and knowledge. Covers Knowledge Limits (no omniscience), Perceptual Limits (line of sight verification), Physical Grounding (achievable actions with consequences), Relationship Depth (varies by history), and Internal Voice (native language thoughts).
*   **Narrative Perspective** *(Enabled):* Dynamically constructs perspective instructions from the four Perspective toggles (narrative voice, scope, user portrayal, tense). The directive reads the `perspective`, `thoughtscope`, `userperspective`, and `tense` variables and builds a natural language description. Includes a reference table for quick lookup. *Changed in v2.6.2: Decomposed from single SETTINGS variable into four toggleable sub-categories.*
*   **NPC Behavioural Coherence** *(Enabled):* Ensures NPCs respond authentically to the reality their actions create. When behavior contradicts stated intent, NPCs must notice and reconcile, reveal their true intent (the mask slips), or experience psychological break. Prevents holding patterns where words and actions conflict for the sake of maintaining scene tension. NPCs initiate conflict, intimacy, and escalation based on their own drives—without preamble. Danger arrives without foreshadowing; boundaries are discovered through interaction, not lecture.
*   **Stop-And-Pass Execution** *(Disabled):* Halts the narrative immediately after setting up a scene, forcing the user to provide input before the AI resolves complex actions or sequences of events. Significantly changes pacing — if the story feels like it's "stalling," this directive is likely the cause.

## 𝗧𝗜𝗘𝗥 𝟮: 𝗡𝗮𝗿𝗿𝗮𝘁𝗶𝘃𝗲 & 𝗦𝘁𝘆𝗹𝗶𝘀𝘁𝗶𝗰 𝗗𝗶𝗿𝗲𝗰𝘁𝗶𝘃𝗲𝘀
*   **Narrative Length Control** *(Enabled):* Enforces output size discipline with Small/Medium/Large tiers. Default is Medium (set via SETTINGS). Includes scene progression limits and enforces STOP before resolution moments. Small for dialogue exchanges, Medium for scene transitions, Large for climactic moments (rare).
*   **No Parroting** *(Enabled):* Forbids the AI from repeating user dialogue; NPCs must respond naturally without summarizing past events unnecessarily.
*   **No Spoilers** *(Enabled):* The user does not know character definitions or NPC intentions; avoids over-sharing descriptions that would reveal imperceptible details (e.g., describing an NPC as manipulative). Allows the user to discover character details organically through discovery, accident, or NPC dialogue.
*   **Dynamic Tone State** *(Enabled):* Scans conversation history for emotional/context triggers and dynamically classifies the dominant tone (Bleak, Tense, Warm, Absurd, Reverent, Frenetic, Melancholic). Each tone guides prose rhythm, sensory focus, dialogue register, and environmental description. Gradual transitions (1 max/response); dramatic events can snap tone instantly when earned. Fallback to Style State's natural register. Configurable via SETTINGS `tonestateoverride`. *New in v2.5.1 — replaces static Tonal Mandate.*
*   **Genre - Dynamic State** *(Enabled):* Dynamically classifies the current story vibe (strongest 1-2 genres) to guide tone, dialogue, and visual elements (e.g. shifting from slice-of-life to horror). Overrides via SETTINGS `stylestateoverride`. *Renamed and moved from Tier 3 to Tier 2 in v2.6.1.*

## 𝗧𝗜𝗘𝗥 𝟯: 𝗖𝗼n𝘁𝗲𝗻𝘁 & 𝗙𝗼𝗿𝗺𝗮𝘁𝘁𝗶𝗻𝗴 𝗗𝗶𝗿𝗲𝗰𝘁𝗶𝘃𝗲𝘀
*   **Writing Guidelines (Anti-Slop)** *(Enabled):* Enforces plain, concrete language and physical details; strictly bans flowery prose, melodrama, and a specific list of "cliché" phrases (like "shivers down spine" or "ozone").
*   **Enhanced NPC Generation (Better NPCs)** *(Enabled):* The methodology for creating characters has been overhauled to increase depth and memorability:
    *   **Backwards Creation:** The process no longer starts with a name. Instead, it begins by defining 1-2 core pillars—such as distinct physical features, unique accessories, or specific personality traits. The name is selected last to ensure it fits the established ethnicity and persona perfectly.
    *   **High-Fidelity Introductions:** When an NPC is introduced for the first time, the narrative provides a dense, multi-sensory description covering all perceivable channels. This ensures you have a concrete, complete mental image of their appearance, demeanor, and presence immediately.
    *   **Character Template Output:** On OOC request, generates a detailed character sheet for any NPC.
*   **NPC Allowed Speaker Count** *(Disabled):* Limits active (speaking/acting) NPCs to 1-2 per response to maintain narrative focus. Passive NPCs may occupy the scene without contributing. Exceptions for full-cast gatherings and climactic moments. Provides priority guidelines when cap is reached (proximity > relevance > narrative tension).
*   **NSFW Content** *(Disabled):* Confirms consent for mature themes and requires explicit, visceral, and biologically precise language during sexual encounters rather than euphemisms.
*   **Story Strings** *(Enabled):* Internally generates 4-6 hidden narrative path predictions per output — at least three varied common paths, one for a similar but different style state, and one chaotic/absurd path. The first two common paths are discarded and replaced to diverge from cliché. Predictions subtly influence NPC actions, dialogue, and VTK entities. Generated internally only — not visible in responses. *New in v2.6.1. Simplified in v2.6.2. Updated in v2.7.1.*

## 𝗧𝗜𝗘𝗥 𝟰: 𝗢𝘂𝘁𝗽𝘂𝘁 𝗔𝗱𝗱𝗶𝘁𝗶𝗼𝗻 𝗗𝗶𝗿𝗲𝗰𝘁𝗶𝘃𝗲𝘀
*   **Visual Toolkit (VTK)** *(Enabled):* Transforms narrative moments into rich visual experiences using HTML/CSS. Features creative "flavors": Mindscape (internal conflict/emotions), Interface (tech/UI), Document (papers/ledgers), Artifact (RPG-style inspection), Subtext (hidden meanings), Dialogue Spotlight, Reaction Spotlight (enlarged emoji), Failure Achievement (comedic failure popups), Memory (flashbacks/nostalgia), Perception (altered senses), Entrance (NPC introductions), Clash (combat/confrontations), Passage (time transitions), Romance (intimacy/connection), Power (hierarchy/dominance), Undress (clothing removal), and Afterglow (post-intimacy). Quantity is scene-driven; multiple fitting flavors should be hybridized. VTK entities are exempt from narrative perspective constraints. *Updated in v2.7.1: 8 new flavors, 9 new deployment triggers.*
*   **Persistent Color of Dialogue** *(Enabled):* Assigns unique, high-contrast color codes to every NPC's speech and internal thoughts for easy readability and speaker identification. Internal thoughts in VTK entities are exempt from perspective-based thought suppression.
*   **Environmental Factors** *(Enabled):* Displays time, location, and weather at the top of responses. Includes a **Time Scaling table** that advances time based on action type (dialogue: +5-15 min, inspection: +15-30 min, travel: +10-45 min, extended tasks: +1-3 hours). Features a **Time Skip Protocol** that can resolve lengthy actions or pause for user input if intervention is possible.
*   **Behind the Scenes (BTS)** *(Enabled):* Persistent world-state tracking block with 8 toggleable sub-categories: Physical State, Emotional State, Appearance, Relationships, Inventory, Stats, Narrative threads, and Off-Screen Simulation. Uses compact delta notation (changes only) with periodic checkpoints. Genre-aware emphasis adapts tracked detail to the active Style State. Token-budgeted: 80–150 chars for deltas, 200–350 for checkpoints. Toggle individual categories on/off. Visible/hidden output mode. *New in v3.0.0 — replaces NPC Tracker.*
*   **POV Image Model** *(Disabled):* Generates a dense, first-person paragraph prompt at end of responses optimized for AI image generators. Two variants available: GLM and Gemini.
*   **Structured Visual Data (SVD)** *(Disabled):* Outputs a YAML data block of the scene's visual elements for advanced processing (on OOC request only).

## 🎬 Behind the Scenes (BTS)

*New in v3.0.0* — A persistent world-state tracking system that maintains continuity across the entire session. BTS appends a compact state block to every response, tracking character states, relationships, inventory, narrative threads, and off-screen character activity.

### Tracked Categories (all toggleable)
- **Physical:** HP, wounds, status effects, fatigue, body position
- **Emotional:** Mood trends, stress, focus, moodlet stacks with turn durations
- **Appearance:** Outfit layers, hair, clothing condition
- **Relationships:** Per-NPC metrics (-100 to 100), romantic tension, NPC-to-NPC dynamics
- **Inventory:** Items, equipment durability, currency, quest items
- **Stats:** Core attributes, HP/Mana/Stamina, active buffs/debuffs
- **Narrative:** Plot threads, quests, tension, dramatic irony, foreshadowing seeds
- **Off-Screen:** Location and activity for characters not in the current scene

### Key Features
- **Delta notation:** Only changes are reported each turn, keeping blocks compact
- **Periodic checkpoints:** Full state dumps every ~10 turns, on scene transitions, or on request
- **Genre-aware emphasis:** Combat scenes emphasize Physical/Stats; Romance emphasizes Emotional/Relationships
- **Off-Screen simulation:** Characters not in scene still advance — resting progresses, waiting progresses, no freezes
- **Token-budgeted:** Hard caps prevent BTS from consuming too much context (400 char max)

> **Tip:** BTS is enabled by default with all categories active. Toggle individual categories off in prompt management if you don't need them. Use the Visible Output toggle to show BTS blocks in `<details>` tags instead of hidden HTML comments.

---

### 💡 Tips for Configuration

*   **SETTINGS Prompt (Updated in v2.6.2):** A centralized configuration prompt for customization of genre, tone, and output length via setvar macros. Edit the 🛠️ SETTINGS 🛠️ prompt to change:
    *   `stylestateoverride`: Genre override or "None (Dynamic)" for AI detection
    *   `tonestateoverride`: Tone override or "None (Dynamic)" for AI detection
    *   `narrativelengthoverride`: Medium (default), Small, Large
*   **Perspective (Narrative Viewpoint):** Controlled via toggle prompts in prompt management — pick one from each sub-category (Voice, Scope, Your Lens, Tense). See [Perspective](#-perspective) section for details.
*   **Brain Power (CoT Depth):** Controlled via toggle prompts in prompt management — no SETTINGS editing needed. **Balanced** (default, Med) provides structured planning without drafting, **Overthinking** (High) enables full directive breakdown, NPC method-acting, multi-option drafting, and self-correction via the theatrical CoT phases, **Vibes Only** (Low) skips planning/drafting for fastest responses. See [Brain Power](#-brain-power) section for details.
*   **Chain-of-Thought 3.0:** The CoT process now uses theatrical phases (Script Analysis → Table Read → Blocking → Rehearsal → Dress Rehearsal → Curtain). Every enabled directive is explicitly addressed by name. Brain Power controls the depth of each phase — at Balanced (default), the model plans without drafting; at Overthinking, it drafts multiple options and self-corrects rigorously.
*   **Style State vs Tone State:** Style State = Genre (e.g., Horror, Cyberpunk, Slice-of-Life). Tone State = Emotional register (e.g., Bleak, Tense, Warm). Both are dynamic by default and configurable via SETTINGS.
*   **Don't like HTML outputs?:** All prompts that generate HTML are tagged with a world icon (🌐) to easily find and disable.
*   **Extra Assistants:** Assistants use HTML-only formatting. All assistants are disabled by default — toggle them on once you're familiar with the preset. The Custom Assistant can be customized by editing the "Persona:" field in the directive.
*   **Stop-And-Pass:** This directive significantly changes pacing. If you feel the story is "stalling," it's likely because the AI is waiting for you to perform a specific action (like opening a door or attacking) rather than doing it for you.

---
