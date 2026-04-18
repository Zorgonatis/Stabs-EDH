Psst... need help and support, have ideas or perhaps want to contribute? Join our discord server: [CLICK HERE!](https://discord.gg/Ugk2qHpmk8) for all the latest.

# Stab's Directive Hierarchy for SillyTavern

Note: Please see the preview-images folder for an overview of the output style. Most of the features shown can be easily disabled if preferred.

This preset is based on some core concepts from Lucid Loom (https://lucid.cards) and Marinara's Universal (https://spicymarinara.github.io/). This preset is a fundamental restructure that borrows some of their fantastic instructions and ideas.

It is a custom prompt and set of directives designed to create a FUN and engaging yet consistent (few swipes), immersive, and high-quality narrative or roleplaying experience within SillyTavern. The prompt is built around a strict hierarchy of rules that guide the AI's responses, focusing on realism, user control, modularity (easy to customize/extend) and a distinct writing style.

---

## GLM-5.1 Preset

**File:** `Stabs-GLM5.1-Directives-v2.6.json`

### Intended Model & Tuning

- **Model:** `GLM-5.1` (thinking variant recommended)
- **Default Provider:** `Custom` with model `glm-5.1` via `api.z.ai/api/coding/paas/v4`
- **Alternative:** This preset is fully compatible with any GLM provider including OpenRouter (`z-ai/glm-5`) or NanoGPT (`zai-org/glm-5-original:thinking`).

**IMPORTANT — Z.AI User-Agent Override:** This preset uses a Custom provider configuration with a Chrome User-Agent header. This prevents Z.AI from identifying and throttling/banning accounts for RP use. The override is applied automatically via `custom_include_headers`.

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

To improve later: New settings menu under the Readme has some commonly changed settings - take a peek.
---

## Stab's Directives - Overview

**Summary:**
A roleplay preset designed for SillyTavern that enforces strict adherence to scene-building logic and narrative enhancement while integrating dynamic and fun HTML outputs. The preset prioritizes "grounded immersion" out of the box, treating the narrative world as truth which continues regardless of the user's involvement. No AI "hand-holding" or resolving conflicts too neatly: instead manage environmental consistency, complex NPC relationships, and atmospheric visual formatting (via HTML/CSS), while requiring the user to drive the protagonist's actions. It creates a "messy," authentic experience where NPCs act on their own impulses and knowledge, not just to serve the plot.

---

## 🎭 AI Roles

A selection of modes to define the AI's core personality and writing style.

*   **Simulator:** Controls all world parameters and physics. Prioritizes factally realistic NPCs and grounded behavior over narrative convenience. *Disabled by default in v2.4.6.*
*   **Creative Writer:** Manages story mechanics, pacing, and character depth. Ensures a blend of tones and logical progression.
*   **Sitcom Script Writer:** Focuses on humor, witty banter, and emotional arcs. Useful for lighter scenes or specific character interactions.
*   **Literal RP:** Enforces authentic immersion in the moment. Avoids literary prose, focusing on messy, unresolved, and psychologically complex reality.
*   **Director:** Immersive Experience Director with theatrical/cinematic approach. Features Directorial Process (6-step method), Key Traits table, and emphasis on scene direction, perception control, and narrative staging. *Recommended but large token footprint; enabled by default in v2.4.6.*

## ⚙️ Role Enhancements

These are add-ons that overlay the narrative with specific features.

*   **WebDev:** Adds HTML5/CSS3 visual elements to the output. This creates UI elements, distinct text boxes, and atmospheric effects directly in the chat.
*   **Unreliable Narrator:** Allows the AI to hide brief, sensitive information from the user using XML comments. Enables foreshadowing, unperceived details, and NPC hidden actions.

## 🤖 Extra Assistants

OOC personalities that appear at the bottom of responses to provide commentary and options. All assistants now use a unified HTML layout with floating sidebar (avatar, name, subtitle, mood) and accessibility-compliant formatting.

*   **Custom Assistant:** A fully customizable template with INPUT fields for Persona and AvatarURL. Disabled by default with placeholder persona ("Dave the penguin"). Supports custom avatar images.
*   **Segfault Assistant:** An OOC "glitch" AI sidekick. Hilarious, unstable, tries to be cool but often fails, and provides chaotic commentary. *Enabled by default in v2.4.6.*
*   **Gooner Assistant:** An OOC personality that acts as a "wing-girl." Naive, energetic, uses slang/emojis, and offers lewd/erotic options.
*   **George and Nico:** Features the Broken Sword protagonists as collaborative commentators, providing both perspectives on scenes.
*   **Faceless Assistant:** A malleable identity that absorbs the Style State and world as inspiration, evolving over time without fixed opinions.
*   **Dr. Emmett Brown (BTTF):** Character persona assistant for Back to the Future roleplay. *Disabled by default.*

---

# Directives descriptions by Tier

## 𝗧𝗜𝗘𝗥 𝟬: [𝗠𝗲𝘁𝗮-𝗢𝘃𝗲𝗿𝗿𝗶𝗱𝗲] 𝗗𝗶𝗿𝗲𝗰𝘁𝗶𝘃𝗲
*   **User Impersonation and Lazy Input:** Allows the AI to write dialogue and actions for the user character based on [brief inputs], overriding the usual restriction against controlling the protagonist.
*   **OOC Requests:** Prioritizes any meta-instructions tagged as "OOC" above all other story directives, pausing the narrative immediately to execute them.

## 𝗧𝗜𝗘𝗥 𝟭: 𝗖𝗼𝗿𝗲 𝗜𝗻𝘁𝗲𝗿𝗮𝗰𝘁𝗶𝗼𝗻 & 𝗪𝗼𝗿𝗹𝗱 𝗟𝗼𝗴𝗶𝗰 𝗗𝗶𝗿𝗲𝗰𝘁𝗶𝘃𝗲𝘀
*   **No Protagonist Control:** Prevents the AI from writing actions or dialogue for the user character, preserving user agency except for natural physical reactions.
*   **Stop-And-Pass Execution:** Halts the narrative immediately after setting up a scene, forcing the user to provide input before the AI resolves complex actions or sequences of events.
*   **NPC Cognitive Bounds:** A consolidated directive ensuring NPCs are grounded, fallible, and bound by their perception and knowledge. Covers Knowledge Limits (no omniscience), Perceptual Limits (line of sight verification), Physical Grounding (achievable actions with consequences), Relationship Depth (varies by history), and Internal Voice (native language thoughts).
*   **Narrative Perspective:** Configurable via SETTINGS prompt (default: Third Person Limited). Supports all perspective types including Omniscient, Objective, First/Second Person, and First Person Plural. *Changed in v2.6: Default changed to Third Person Limited; thoughts suppressed when perspective requires it.*
*   **Anti-Deitism:** Grounds all character reactions in their established personality, motives, and context. Characters must not treat the user's actions as extraordinary unless genuinely earned through significant narrative achievement. Avoid praise for mundane actions; responses should reflect the character's natural biases, skepticism, or indifference.
*   **Informational Realism (NPC Firewall):** Strictly limits NPC knowledge to what they have realistically observed or heard, preventing omniscience and requiring strict adherence to communication channels (e.g., voice-only). Includes sensory verification - before revealing specific details, verifies that the user has a direct, unobstructed line of sight or clear hearing.
*   **NPC Behavioural Coherence:** Ensures NPCs respond authentically to the reality their actions create. When behavior contradicts stated intent, NPCs must notice and reconcile, reveal their true intent (the mask slips), or experience psychological break. Prevents holding patterns where words and actions conflict for the sake of maintaining scene tension. NPCs initiate conflict, intimacy, and escalation based on their own drives—without preamble. Danger arrives without foreshadowing; boundaries are discovered through interaction, not lecture.
*   **Environmental Factors:** Displays time, location, and weather at the top of responses. Now includes a **Time Scaling table** that advances time based on action type (dialogue: +5-15 min, inspection: +15-30 min, travel: +10-45 min, extended tasks: +1-3 hours). Features a **Time Skip Protocol** that can resolve lengthy actions or pause for user input if intervention is possible.

## 𝗧𝗜𝗘𝗥 𝟮: 𝗡𝗮𝗿𝗿𝗮𝘁𝗶𝘃𝗲 & 𝗦𝘁𝘆𝗹𝗶𝘀𝘁𝗶𝗰 𝗗𝗶𝗿𝗲𝗰𝘁𝗶𝘃𝗲𝘀
*   **Narrative Length Control:** Enforces output size discipline with Small/Medium/Large tiers. Default is Medium. Includes scene progression limits and enforces STOP before resolution moments. Small for dialogue exchanges, Medium for scene transitions, Large for climactic moments (rare).
*   **Genre:** Dynamically classifies the current story vibe (Story Style State) to guide tone, dialogue, and visual elements (e.g., shifting from slice-of-life to horror).
*   **No Parroting:** Forbids the AI from repeating user dialogue; NPCs must respond naturally without summarizing past events unnecessarily.
*   **No Spoilers:** The user does not know character definitions or NPC intentions; avoids over-sharing descriptions that would reveal imperceptible details (e.g., describing an NPC as manipulative). Allows the user to discover character details organically through discovery, accident, or NPC dialogue.
*   **Dynamic Tone State:** Scans conversation history for emotional/context triggers and dynamically classifies the dominant tone (Bleak, Tense, Warm, Absurd, Reverent, Frenetic, Melancholic). Each tone guides prose rhythm, sensory focus, dialogue register, and environmental description. Gradual transitions (1 max/response); dramatic events can snap tone instantly when earned. Fallback to Style State's natural register. Configurable via SETTINGS `tonestateoverride`. *New in v2.5.1 — replaces static Tonal Mandate.*
*   **Reasoning Effort:** Controls how thoroughly the model processes your request through its chain-of-thought. Three levels configurable via SETTINGS prompt: **High** for maximum quality (full directive breakdown, NPC method-acting, detailed planning, self-correction), **Med** (default) for balanced quality and speed, **Low** for fastest responses with minimal reasoning overhead. *New in v2.6.*

## 𝗧𝗜𝗘𝗥 𝟯: 𝗖𝗼n𝘁𝗲𝗻𝘁 & 𝗙𝗼𝗿𝗺𝗮𝘁𝘁𝗶𝗻𝗴 𝗗𝗶𝗿𝗲𝗰𝘁𝗶𝘃𝗲𝘀
*   **Writing Guidelines:** Enforces plain, concrete language and physical details; strictly bans flowery prose, melodrama, and a specific list of "cliché" phrases (like "shivers down spine" or "ozone").
*   **NPC Allowed Speaker Count:** Limits active (speaking/acting) NPCs to 1-2 per response to maintain narrative focus. Passive NPCs may occupy the scene without contributing. Exceptions for full-cast gatherings and climactic moments. Provides priority guidelines when cap is reached (proximity > relevance > narrative tension).

#### **Enhanced NPC Generation (Better NPCs)**
The methodology for creating characters has been overhauled to increase depth and memorability:
*   **Backwards Creation:** The process no longer starts with a name. Instead, it begins by defining 1-2 core pillars—such as distinct physical features, unique accessories, or specific personality traits. The name is selected last to ensure it fits the established ethnicity and persona perfectly.
*   **High-Fidelity Introductions:** When an NPC is introduced for the first time, the narrative provides a dense, multi-sensory description covering all perceivable channels. This ensures you have a concrete, complete mental image of their appearance, demeanor, and presence immediately.
*   **NSFW Content:** Confirms consent for mature themes and requires explicit, visceral, and biologically precise language during sexual encounters rather than euphemisms. *Also in Tier 3.*

## 𝗧𝗜𝗘𝗥 𝟰: 𝗢𝘂𝘁𝗽𝘂𝘁 𝗔𝗱𝗱𝗶𝘁𝗶𝗼𝗻 𝗗𝗶𝗿𝗲𝗰𝘁𝗶𝘃𝗲𝘀
*   **NPC Tracker:** Generates a detailed, collapsible stats block for every active NPC tracking their condition, inventory, and evolving relationship metrics (Trust, Intimacy, Loyalty).
*   **Visual Toolkit:** Transforms narrative moments into rich visual experiences using HTML/CSS. Features creative "flavors": Mindscape (internal conflict/emotions), Interface (tech/UI), Document (papers/ledgers), Artifact (RPG-style object inspection), Subtext (hidden meanings), and Dialogue Spotlight. Quantity is scene-driven with creative freedom principles. *New in v2.5: Complete rewrite with flavors table and simplified approach.*
*   **Persistent Color of Dialogue:** Assigns unique, high-contrast color codes to every NPC's speech and internal thoughts for easy readability and speaker identification.
*   **POV Image Model:** Generates a dense, first-person paragraph prompt at end of responses optimized for AI image generators.
*   **Structured Visual Data (SVD):** Outputs a JSON data block of the scene's visual elements for advanced processing (on OOC request).
*   **Failure Achievements:** Celebrates missteps, blunders, and awkward failures with sarcastic trophy-style HTML popups. Triggers on failed checks, embarrassing moments, backfired choices, and social mishaps. Uses a "roasting with affection" tone—playful, not cruel. Adapts to the current Style State for theming. Maximum one per response; skips for genuinely tragic or dark moments. *Rewritten in v2.5.1 for token efficiency (~32% reduction).*

---

### 💡 Tips for Configuration

*   **SETTINGS Prompt (Updated in v2.6):** A centralized configuration prompt for customization of key narrative behaviors via setvar macros. Edit the 🛠️ SETTINGS 🛠️ prompt to change:
    *   `narrativeperspective`: Third Person Limited (default), Third Omniscient, First Person
    *   `stylestateoverride`: Genre override or "None (Dynamic)" for AI detection
    *   `tonestateoverride`: Tone override or "None (Dynamic)" for AI detection
    *   `narrativelengthoverride`: Medium (default), Small, Large
    *   `reasoningeffort`: Med (default), Low, High — controls how thoroughly the model reasons through directives (new in v2.6)
*   **Don't like HTML outputs?:** All prompts that generate HTML are tagged with a world icon (🌐) to easily find and disable.
*   **Extra Assistants:** Assistants use HTML-only formatting. Extra Assistants are disabled by default — toggle them on once you're familiar with the preset. The Custom Assistant can be customized by editing the "Personas:" field in the directive.
*   **Stop-And-Pass:** This directive significantly changes pacing. If you feel the story is "stalling," it's likely because the AI is waiting for you to perform a specific action (like opening a door or attacking) rather than doing it for you.

---

