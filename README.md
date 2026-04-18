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
| **AI Role** | Director (Recommended but large) |
| **Role Enhancements** | WebDev, Unreliable Narrator |
| **Tier 0** | OOC Priority |
| **Tier 1** | No Protagonist Control, NPC Behavioural Coherence, NPC Cognitive Bounds, Narrative Perspective |
| **Tier 2** | Narrative Length Control, No Parroting, No Spoilers, Dynamic Tone State |
| **Tier 3** | Style State / Genre, Writing Guidelines (Anti-Slop), Better NPCs |
| **Tier 4** | Visual Toolkit, Color Dialogue/Thoughts, Failure Achievements, Environmental Factors |

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

**Quick customisation:** Edit the 🛠️ SETTINGS 🛠️ prompt in prompt management to adjust narrative perspective, genre/tone overrides, output length, and reasoning effort — all without touching individual directives. See the [Tips for Configuration](#-tips-for-configuration) section below for details.

---

## GLM-5.1 Preset

**File:** `Stabs-GLM5.1-Directives-v2.6.json`

### Intended Model & Tuning

- **Model:** `GLM-5.1` (thinking variant recommended)
- **Default Provider:** `Custom` with model `glm-5.1` via `api.z.ai/api/coding/paas/v4`
- **Alternatives:** This preset is fully compatible with any GLM provider.

**IMPORTANT — Z.AI User-Agent Override:** This preset uses a Custom provider configuration with a Chrome User-Agent header. This prevents Z.AI from identifying and throttling/banning accounts for RP use. The override is applied automatically via `custom_include_headers`. **to remove later - z.ai backpeddled**

The following parameters are enabled (Additional Parameters in your API Connections) to ensure thinking and sampling are set for creative writing. Additionally, as of GLM-4.7, we can include clear_thinking: 'true' to not reuse past reasoning context.
These are NOT required but do not hurt to set in case of poor backend configuration.

```yaml
thinking:
  type: "enabled"
  clear_thinking: "true"
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

## 🤖 Extra Assistants

OOC personalities that appear at the bottom of responses to provide commentary and options. All assistants use a unified HTML layout with floating sidebar (avatar, name, subtitle, mood) and accessibility-compliant formatting. **All assistants are disabled by default** — toggle them on once you're familiar with the preset.

*   **Custom Assistant:** A fully customizable template with INPUT fields for Persona and AvatarURL. Placeholder persona ("Dave the penguin"). Supports custom avatar images.
*   **SEGFAULT Assistant:** An OOC "glitch" AI sidekick. Hilarious, unstable, tries to be cool but often fails, and provides chaotic commentary.
*   **Gooner Assistant:** An OOC personality that acts as a "wing-girl." Naive, energetic, uses slang/emojis, and offers lewd/erotic options.
*   **George and Nico:** Features the Broken Sword protagonists as collaborative commentators, providing both perspectives on scenes.
*   **Faceless Assistant:** A malleable identity that absorbs the Style State and world as inspiration, evolving over time without fixed opinions. Configured as a Role Enhancement rather than an Assistant, affecting narrative styling.
*   **Dr. Emmett Brown (BTTF):** Character persona assistant for Back to the Future roleplay.

---

# Directives descriptions by Tier

## 𝗧𝗜𝗘𝗥 𝟬: [𝗠𝗲𝘁𝗮-𝗢𝘃𝗲𝗿𝗿𝗶𝗱𝗲] 𝗗𝗶𝗿𝗲𝗰𝘁𝗶𝘃𝗲
*   **OOC Priority** *(Enabled):* Any meta instruction starting with `[OOC:]` or including "OOC" triggers an immediate stop. The AI adopts a helpful assistant personality and executes the request as the highest priority, overriding all other directives. The narrative does not progress during OOC responses.
*   **User Impersonation and Lazy Input:** *(Disabled)* Allows the AI to write dialogue and actions for the user character based on [brief inputs], overriding the usual restriction against controlling the protagonist. Automatically sets narrative perspective to Second Person.

## 𝗧𝗜𝗘𝗥 𝟭: 𝗖𝗼𝗿𝗲 𝗜𝗻𝘁𝗲𝗿𝗮𝗰𝘁𝗶𝗼𝗻 & 𝗪𝗼𝗿𝗹𝗱 𝗟𝗼𝗴𝗶𝗰 𝗗𝗶𝗿𝗲𝗰𝘁𝗶𝘃𝗲𝘀
*   **No Protagonist Control** *(Enabled):* Prevents the AI from writing actions or dialogue for the user character, preserving user agency except for natural physical reactions.
*   **NPC Cognitive Bounds** *(Enabled):* Ensures NPCs are grounded, fallible, and bound by their perception and knowledge. Covers Knowledge Limits (no omniscience), Perceptual Limits (line of sight verification), Physical Grounding (achievable actions with consequences), Relationship Depth (varies by history), and Internal Voice (native language thoughts).
*   **Narrative Perspective** *(Enabled):* Configurable via SETTINGS prompt (default: Third Person Limited). Supports all perspective types including Omniscient, Objective, First/Second Person, and First Person Plural. *Changed in v2.6: Default changed to Third Person Limited; thoughts suppressed when perspective requires it.*
*   **NPC Behavioural Coherence** *(Enabled):* Ensures NPCs respond authentically to the reality their actions create. When behavior contradicts stated intent, NPCs must notice and reconcile, reveal their true intent (the mask slips), or experience psychological break. Prevents holding patterns where words and actions conflict for the sake of maintaining scene tension. NPCs initiate conflict, intimacy, and escalation based on their own drives—without preamble. Danger arrives without foreshadowing; boundaries are discovered through interaction, not lecture.
*   **Stop-And-Pass Execution** *(Disabled):* Halts the narrative immediately after setting up a scene, forcing the user to provide input before the AI resolves complex actions or sequences of events. Significantly changes pacing — if the story feels like it's "stalling," this directive is likely the cause.

## 𝗧𝗜𝗘𝗥 𝟮: 𝗡𝗮𝗿𝗿𝗮𝘁𝗶𝘃𝗲 & 𝗦𝘁𝘆𝗹𝗶𝘀𝘁𝗶𝗰 𝗗𝗶𝗿𝗲𝗰𝘁𝗶𝘃𝗲𝘀
*   **Narrative Length Control** *(Enabled):* Enforces output size discipline with Small/Medium/Large tiers. Default is Medium (set via SETTINGS). Includes scene progression limits and enforces STOP before resolution moments. Small for dialogue exchanges, Medium for scene transitions, Large for climactic moments (rare).
*   **No Parroting** *(Enabled):* Forbids the AI from repeating user dialogue; NPCs must respond naturally without summarizing past events unnecessarily.
*   **No Spoilers** *(Enabled):* The user does not know character definitions or NPC intentions; avoids over-sharing descriptions that would reveal imperceptible details (e.g., describing an NPC as manipulative). Allows the user to discover character details organically through discovery, accident, or NPC dialogue.
*   **Dynamic Tone State** *(Enabled):* Scans conversation history for emotional/context triggers and dynamically classifies the dominant tone (Bleak, Tense, Warm, Absurd, Reverent, Frenetic, Melancholic). Each tone guides prose rhythm, sensory focus, dialogue register, and environmental description. Gradual transitions (1 max/response); dramatic events can snap tone instantly when earned. Fallback to Style State's natural register. Configurable via SETTINGS `tonestateoverride`. *New in v2.5.1 — replaces static Tonal Mandate.*
*   **Reasoning Effort:** Controls how thoroughly the model processes your request through its chain-of-thought. Three levels configurable via SETTINGS prompt: **High** for maximum quality (full directive breakdown, NPC method-acting, detailed planning, self-correction), **Med** (default) for balanced quality and speed, **Low** for fastest responses with minimal reasoning overhead. *New in v2.6.*

## 𝗧𝗜𝗘𝗥 𝟯: 𝗖𝗼n𝘁𝗲𝗻𝘁 & 𝗙𝗼𝗿𝗺𝗮𝘁𝘁𝗶𝗻𝗴 𝗗𝗶𝗿𝗲𝗰𝘁𝗶𝘃𝗲𝘀
*   **Style State / Genre** *(Enabled):* Dynamically classifies the current story vibe (Story Style State) to guide tone, dialogue, and visual elements (e.g., shifting from slice-of-life to horror). Overrides via SETTINGS `stylestateoverride`.
*   **Writing Guidelines (Anti-Slop)** *(Enabled):* Enforces plain, concrete language and physical details; strictly bans flowery prose, melodrama, and a specific list of "cliché" phrases (like "shivers down spine" or "ozone").
*   **Enhanced NPC Generation (Better NPCs)** *(Enabled):* The methodology for creating characters has been overhauled to increase depth and memorability:
    *   **Backwards Creation:** The process no longer starts with a name. Instead, it begins by defining 1-2 core pillars—such as distinct physical features, unique accessories, or specific personality traits. The name is selected last to ensure it fits the established ethnicity and persona perfectly.
    *   **High-Fidelity Introductions:** When an NPC is introduced for the first time, the narrative provides a dense, multi-sensory description covering all perceivable channels. This ensures you have a concrete, complete mental image of their appearance, demeanor, and presence immediately.
    *   **Character Template Output:** On OOC request, generates a detailed character sheet for any NPC.
*   **NPC Allowed Speaker Count** *(Disabled):* Limits active (speaking/acting) NPCs to 1-2 per response to maintain narrative focus. Passive NPCs may occupy the scene without contributing. Exceptions for full-cast gatherings and climactic moments. Provides priority guidelines when cap is reached (proximity > relevance > narrative tension).
*   **NSFW Content** *(Disabled):* Confirms consent for mature themes and requires explicit, visceral, and biologically precise language during sexual encounters rather than euphemisms.

## 𝗧𝗜𝗘𝗥 𝟰: 𝗢𝘂𝘁𝗽𝘂𝘁 𝗔𝗱𝗱𝗶𝘁𝗶𝗼𝗻 𝗗𝗶𝗿𝗲𝗰𝘁𝗶𝘃𝗲𝘀
*   **Visual Toolkit (VTK)** *(Enabled):* Transforms narrative moments into rich visual experiences using HTML/CSS. Features creative "flavors": Mindscape (internal conflict/emotions), Interface (tech/UI), Document (papers/ledgers), Artifact (RPG-style object inspection), Subtext (hidden meanings), and Dialogue Spotlight. Quantity is scene-driven with creative freedom principles. *New in v2.5: Complete rewrite with flavors table and simplified approach.*
*   **Persistent Color of Dialogue** *(Enabled):* Assigns unique, high-contrast color codes to every NPC's speech and internal thoughts for easy readability and speaker identification.
*   **Failure Achievements** *(Enabled):* Celebrates missteps, blunders, and awkward failures with sarcastic trophy-style HTML popups. Triggers on failed checks, embarrassing moments, backfired choices, and social mishaps. Uses a "roasting with affection" tone—playful, not cruel. Adapts to the current Style State for theming. Maximum one per response; skips for genuinely tragic or dark moments. *Rewritten in v2.5.1 for token efficiency (~32% reduction).*
*   **Environmental Factors** *(Enabled):* Displays time, location, and weather at the top of responses. Includes a **Time Scaling table** that advances time based on action type (dialogue: +5-15 min, inspection: +15-30 min, travel: +10-45 min, extended tasks: +1-3 hours). Features a **Time Skip Protocol** that can resolve lengthy actions or pause for user input if intervention is possible.
*   **NPC Tracker** *(Disabled):* Generates a detailed, collapsible stats block for every active NPC tracking their condition, inventory, and evolving relationship metrics (Trust, Intimacy, Loyalty) scored -100 to 100.
*   **POV Image Model** *(Disabled):* Generates a dense, first-person paragraph prompt at end of responses optimized for AI image generators. Two variants available: GLM and Gemini.
*   **Structured Visual Data (SVD)** *(Disabled):* Outputs a YAML data block of the scene's visual elements for advanced processing (on OOC request only).

---

### 💡 Tips for Configuration

*   **SETTINGS Prompt (Updated in v2.6):** A centralized configuration prompt for customization of key narrative behaviors via setvar macros. Edit the 🛠️ SETTINGS 🛠️ prompt to change:
    *   `narrativeperspective`: Third Person Limited (default), Third Omniscient, First Person
    *   `stylestateoverride`: Genre override or "None (Dynamic)" for AI detection
    *   `tonestateoverride`: Tone override or "None (Dynamic)" for AI detection
    *   `narrativelengthoverride`: Medium (default), Small, Large
    *   `reasoningeffort`: Med (default), Low, High — controls how thoroughly the model reasons through directives (new in v2.6)
*   **Don't like HTML outputs?:** All prompts that generate HTML are tagged with a world icon (🌐) to easily find and disable.
*   **Extra Assistants:** Assistants use HTML-only formatting. All assistants are disabled by default — toggle them on once you're familiar with the preset. The Custom Assistant can be customized by editing the "Persona:" field in the directive.
*   **Stop-And-Pass:** This directive significantly changes pacing. If you feel the story is "stalling," it's likely because the AI is waiting for you to perform a specific action (like opening a door or attacking) rather than doing it for you.

---
