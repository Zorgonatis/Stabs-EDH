# Stab's Directive Heirarchy for SillyTavern and GLM-4.7 / Gemini 3.0
Note: Please see the output examples at the bottom of the page to get an overview of the output style. Most of the features shown can be easily disabled if preferred.

This preset is based on some core concepts from Lucid Loom (https://lucid.cards) such as the Anti-Slop filter and color-coded NPC fonts. This preset is a fundamental restructure that borrows some of the Lucid Loom's fantastic instructions.

It is a custom prompt and set of directives designed to create a consistent (few swipes), immersive, and high-quality narrative or roleplaying experience within SillyTavern. The prompt is built around a strict heirarchy of rules that guide the AI's responses, focusing on realism, user control, modularity (easy to customize/extend) and a distinct writing style.

## Hotfix 1.21
Most of these changes have allowed for reducuction of token count, provide more engaging and coherent prose, and further improve responses to closer align to the narrative.

- **Problem 1: Guardrails/Content Safety** - It's been discovered that some types of instructions in system prompt cause the model to validate against content safety guidelines, particularly if you try to modify it's chain of thought or apply any kind of jailbreak.
This is an issue with very specific kinds of training data, but is not a core part of the model, it just has to be avoided.

*The solution*: 'Thinking Fix' (basically, everything I just said we shouldn't do) is now optional, renamed to 'Think Adjustment' and _disabled by default_. 

The directives have been sured-up, and reframed as successful output criteria, which results in more attention on the directives through reasoning.
The primary Role (GM) is extended to enable _drafting_ during the reasoning.

Pros: Same net-result as the previous system (heavily instruct CoT), high attention chain of thought. 

Cons: slightly worse prompt adherence overall, small changes in multiple locations -> hard for users to reconcile changes (sorry)

NOTE: Enabling the thinking fix will *improve* prompt adherence, but may trigger the content filters (useful when coming back to old conversations, see Problem 4).

- **Problem 2: SVG** - TL;DR SillyTavern has a really annoying bug that means half the GLM SVG output doesn't get rendered.

*The Solution* - stripped SVG instructions out entirely in favour of HTML/CSS, which GLM 4.7 in particular is VERY strong with.

Cons: If you have a conversation history with SVG in it, it is now 'poisoned' (see Problem 4)

- **Problem 3: Everything is Sci-Fi or high tech** - Why's everything shiny and metallic?

*The Solution*: A new directive, Genre Determination. It will analyze any available context for overall writing genre and use it as a basis for visual outputs, character behaviour etc.

Pros: Most things aren't shiny. Will default to 'Drama, Slice-of-life' if unspecified, and adapt as needed.

Cons: You have to untick this if you specify your own genres.
  
- **Problem 4: A poison in my context** - GLM 4.7 loves to copy examples and structures.
- 
It may not adapt well on top of old context from previous versions of this preset.

Tips: enable the 'Think Adjustment' and/or submit as part of your input: [OOC: Your directives have changed, make sure they are adhered to]


## What's new in 1.2?

- **Support for GLM 4.7**
- **Support for Gemini 3.0 Flash Preview** (note)
- Reduced frequency of 'Visual Toolkit' output frequency
- Adjusted thinking direction
- Countless adjustments and corrections throughout all instructions, reducing confusion or uncertainty during reasoning

**Note**: For use with Gemini, toggle off all prompts with 'GLM' in the name and optionally toggle on the image prompt generator for Gemini.



## What's new in 1.1?

- **First person definitions** - rewrote the entire prompt from the first person perspective to encourage reasoning as an inner monologue rather than analytical analysis.
- **Additional AI identities** - Chose from Creative Writer (Default), Sitcom script writer (comedic focus) and Full Character immersive RP.
- **Relationship Tracker** - Encourages slow burn and iterative, measurable story progress. Helps to push the bot towards guided character impersonations, but can slow things down too much for one-on-one (single NPC) scenarios.
- **General cleanup** - Lots of tweaking, formatting and structural cleanup to present the final prompt to the model as practically as possible.

## Intended Model & Tuning

- **Model:** `GLM-4.7`
- **Tuning Plan:** This preset was created with the `z.ai coding plan`, but is fully compatible with any GLM-4.6 provider. I continue to get the best output and most reliable thinking with the official z.ai API.

The following parameters are enabled (Additional Parameters in your API Connections) to ensure thinking, sampling and top_k is set for creative writing. This is only strictly required for the coding plan API under Custom provider (recommended over z.ai direct provider in ST). 
```yaml
thinking:
  type: "enabled"
do_sample: "true"
top_k: 255
```

## Core Concepts & Benefits

### Thinking Tuning

The core of this system is "Thinking Tuning." By directing the AI's chain of thought to tightly integrate with a directive heirarchy, it achieves predictable, repeatable, and consistent adherence to the desired rules & narrative style. Instead of generating free-form text, the AI actively checks its output against a defined set of rules at every level of its reasoning.

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
*   **Relationship Tracker:** Metrics are tracked for each aquainted NPC allowing for a realistic development of relationships over time. **Recommended: Disabled:** The relationship tracker works best for multi-NPC or slow burn scenarios. 
### Tier 2: Narrative & Stylistic Directives

This tier defines the *style* of the writing to ensure consistency and quality.

*   **No Parroting:** The AI will not repeat the user's dialogue in its narration.
*   **Tonal Mandate:** The narrative features the full spectrum dark and light situations to remain grounded and realistic.
*   **Anti-Slop Filter:** The AI uses plain verbs and concrete details. Melodrama, flowery language, and a specific list of overused/banned phrases are actively avoided.
    *   **Examples of banned concepts:** `- "shivers down spine"`, `- "pupils blown"`, `- "velvety voice"`, `- "deliberate movements"`, `- "world narrowing"`, `- "faded away"`, `- "real/genuine/true emotion"`.


### Tier 3: Content & Formatting Rules

These rules handle specific details about the world and how information is presented.

*   **New NPC Names:** Simple, modern names are preferred, matching the character's ethnicity and personality where possible.
*   **Distinguishing Content:** Dialogue and internal thought use unique, persistent color tags (`<font color=######>`) for each character.
*   **Persistent Color Coding:** All character dialogue and internal thoughts are wrapped in unique `<font color=######>` tags for clear readability.
    *   Dialogue: `<font color=abc123>"Hello."</font>`
    *   Thought: `<font color=def123>*I wonder what they want.*</font>`
 
### Tier 4: Output Additions

*   Every story response must include a specific, detailed image generation prompt at the bottom, contained within a codeblock. This prompt is designed to generate a POV image reflecting the current scene.


# Output examples (GLM 4.7):

<img width="1113" height="1185" alt="Conversation-GLM4 7" src="https://github.com/user-attachments/assets/adc5b0fc-52f6-4795-9888-cac5169c8c8d" />
<img width="832" height="1328" alt="Chroma_01073_" src="https://github.com/user-attachments/assets/6736b6eb-c17c-4d6e-a7a5-54c58320b550" />
<img width="1097" height="1176" alt="Conversation2-GLM4 7" src="https://github.com/user-attachments/assets/f6522b6d-b004-48dd-bc88-2966d748533c" />



## Older
<img width="1454" height="1077" alt="image" src="https://github.com/user-attachments/assets/4cecd050-f6ff-4ba6-a376-c7f0ee1a7d72" />
<img width="1465" height="1038" alt="image" src="https://github.com/user-attachments/assets/35a5721b-16fb-48c1-b6dd-df045d84d309" />

All below are from a single response (custom scenario, multi-NPC, relationship tracking + image output enabled)
<img width="1418" height="956" alt="image" src="https://github.com/user-attachments/assets/71ea8fc5-8271-44b8-8296-4ed6587bb4f2" />
<img width="1455" height="930" alt="image" src="https://github.com/user-attachments/assets/6c611c85-1311-4a86-8bce-1c9a4ccfe636" />
Chroma generation for the above prompt at end of response:
<img width="744" height="1328" alt="image" src="https://github.com/user-attachments/assets/45d99ed6-395a-43d8-a380-d3ce831707a8" />

