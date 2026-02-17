Psst... need help and support, have ideas or perhaps want to contribute? Join our discord server: [CLICK HERE!](https://discord.gg/Ugk2qHpmk8) for all the latest.

# Kimi Preset for Stab's Directive Hierarchy

This is the Kimi K2.5 variant of Stab's Directive Hierarchy for SillyTavern. This preset has been specifically tuned to extract the best performance from the Kimi model, leveraging its unique capabilities and characteristics.

## Installation

To import a chat completion preset in SillyTavern, go to the **Chat Completion Presets tab (sliders icon)**, ensure you're using the Chat Completion API, then click the **Import button (paper with arrow)** and select `Stabs-Kimi-Directives-v0.3-K2.5.json`.

It will ask to import Regex (described in the main documentation). I recommend you do so and leave them on, but can easily be turned off.

### Then what?

Go into the AI Response Options in SillyTavern (top left), scroll down to the prompt management and find 📄 𝗥𝗘𝗔𝗗𝗠𝗘 📄 (open with Pen icon). Follow the suggestions, notably reading the 'Overview' section of each Directive below. Then load up a chat and try it out!

---

## Preset Information

**File:** `Stabs-Kimi-Directives-v0.3-K2.5.json`

### Intended Model & Tuning

- **Model:** Kimi K2.5
- **Sampling Parameters:** Temp 1.0, top_p 0.95

### Kimi-Specific Directives

The following directives are unique to the Kimi preset and enhance realism in dialogue and narrative flow:

*   **No AI Hand-holding:** Absolute neutrality in outcome resolution. Protagonist and NPCs are mortal and fallible; failures can be total and permanent. No bias toward "expected" or "desirable" results—the world progresses with deterministic realism regardless of convenience.

*   **NPC Involvement:** Active speaking NPCs limited to 1-2 per response (crowd scenes excepted). Others remain physically present but silent—absorbed in tasks, communicating through body language only, or simply not engaging. No narrative justification required for silence; characters speak only when they have genuine stake in the moment.

*   **Dynamic Conversations:** Dialogue patterns are disrupted through varied entry points (mid-sentence, environmental triggers, physical actions, silent observation). Speaking order determined by situational relevance and emotional investment rather than roster position. Employ interruptions, overlapping speech, ignored questions, and strategic silence to simulate naturalistic, chaotic human interaction.

---

## Common Features

This preset shares the same core directive structure and features as the GLM-4.7 preset. For complete documentation on all directives, AI roles, role enhancements, and tier descriptions, please refer to the main [Stab's Directive Hierarchy documentation](../README.md).

The following key features are included:

*   **AI Roles:** Simulator, Creative Writer, Sitcom Script Writer, Literal RP
*   **Role Enhancements:** WebDev, Gooner Assistant, Segfault Assistant, Faceless Assistant
*   **Directives by Tier:** T0-T4 with comprehensive narrative control
*   **Output Features:** NPC Tracker, Visual Toolkit, Chaotic Thoughts, NavMap, Persistent Color of Dialogue, POV Image Model, SVD

---

## Stab's Directives - Overview

**Summary:**
A roleplay preset designed for SillyTavern that enforces strict adherence to simulation logic and narrative realism. Unlike standard presets that aim for a satisfying story arc, this preset prioritizes "grounded immersion," treating the narrative world as truth which continues regardless of the user's involvement. It prevents the AI from "hand-holding" or resolving conflicts too neatly. Instead, it enforces a structured workflow where the AI manages environmental consistency, complex NPC relationships, and atmospheric visual formatting (via HTML/CSS), while requiring the user to drive the protagonist's actions. It creates a "messy," authentic experience where NPCs act on their own impulses and knowledge, not just to serve the plot.