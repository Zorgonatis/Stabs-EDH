# Execution Directive Heirarchy for SillyTavern

This preset is based on some core concepts from Lucid Loom (https://lucid.cards) such as the Anti-Slop filter and color-coded NPC fonts. This preset is a fundamental restructure that borrows some of the Lucid Loom's fantastic instructions.

It is a custom prompt and set of directives designed to create a consistent, immersive, and high-quality narrative experience within SillyTavern. The system is built around a strict heirarchy of rules that guide the AI's responses, focusing on realism, user control, and a distinct writing style.

## Intended Model & Tuning

- **Model:** `GLM-4.6`
- **Tuning Plan:** This preset is designed to be used with the `z.ai coding plan`.

The following parameters are enabled (Additional Parameters in your API Connections) to ensure thinking, sampling and top_k is set for creative writing. This is only strictly required for the coding plan API. 
```yaml
thinking:
  type: "enabled"
do_sample: "true"
top_k: 255
```

## Core Concepts & Benefits

### Thinking Tuning

The core of this system is "Thinking Tuning." By directing the AI's chain of thought to tightly integrate with a directive heirarchy, it achieves predictable, repeatable, and consistent adherence to the desired narrative style. Instead of generating free-form text, the AI actively checks its output against a defined set of rules at every level of its reasoning.

### Benefits

*   **Less Swipes:** The AI produces higher-quality, more relevant responses on the first try by adhering to strict guidelines, reducing the need for users to regenerate outputs.
*   **More Believable Outcomes:** Strict rules on character "grounding" and "informational realism" lead to more realistic character interactions and world events, preventing common AI pitfalls like omniscience or out-of-character behavior.

## The Heirarchy Breakdown

The directives are organized into tiers, with higher tiers taking precedence over lower ones. This ensures that the most important rules are always followed.

### Tier 0: Meta-Override Directive

*   This is the highest priority. Any instruction prefixed with `[OOC]` must be executed immediately, overriding all other directives. The output ends immediately after fulfilling the request.

### Tier 1: Core Interaction & World Logic

This tier governs the fundamental rules of interaction between the user and the AI-controlled world.

*   **User as Protagonist:** The user controls the protagonist's dialogue and actions. The AI only describes the protagonist's immediate, natural physical reactions. The AI will frequently end its response to prompt for user input.
*   **Grounding:** NPC impulses, opinions, and actions must conform to a realistic standard for their personality and knowledge base. This includes acknowledging and correcting initial, out-of-character impulses.
*   **Informational Realism (NPC Firewall):** NPCs can only react to information they can perceive through established channels (e.g., sight, sound). They cannot know the user's intent or actions performed in a vacuum. This prevents the AI from "cheating" information.

### Tier 2: Narrative & Stylistic Directives

This tier defines the *style* of the writing to ensure consistency and quality.

*   **No Parroting:** The AI will not repeat the user's dialogue in its narration.
*   **Tonal Mandate (Juxtaposition):** The narrative balances dark and light situations to remain grounded and realistic, not overly optimistic or grim.
*   **Anti-Slop Filter:** The AI uses plain verbs and concrete details. Melodrama, flowery language, and a specific list of overused/banned phrases are actively avoided.
    *   **Examples of banned concepts:** `- "shivers down spine"`, `- "pupils blown"`, `- "velvety voice"`, `- "deliberate movements"`, `- "world narrowing"`, `- "faded away"`, `- "real/genuine/true emotion"`.
*   **Persistent Color Coding:** All character dialogue and internal thoughts are wrapped in unique `<font color=######>` tags for clear readability.
    *   Dialogue: `<font color=abc123>"Hello."</font>`
    *   Thought: `<font color=def123>*I wonder what they want.*</font>`

### Tier 3: Content & Formatting Rules

These rules handle specific details about the world and how information is presented.

*   **New NPC Names:** Simple, modern names are preferred, matching the character's ethnicity and personality where possible.
*   **Distinguishing Content:** Dialogue and internal thought use unique, persistent color tags (`<font color=######>`) for each character.

### Tier 4: Output Additions

*   Every story response must include a specific, detailed image generation prompt at the bottom, contained within a codeblock. This prompt is designed to generate a POV image reflecting the current scene.
