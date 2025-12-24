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

The following parameters are enabled (Additional Parameters in your API Connections) to ensure thinking, sampling and top_k is set for creative writing. Additionally, as of 4.7, we should include clear_thinking: 'true' to not reuse past reasoning context. These are only strictly required for the coding plan API under Custom provider (recommended over z.ai direct provider in ST). 

```yaml
thinking:
  type: "enabled"
clear_thinking: "true"
do_sample: "true"
top_k: 255
```

### Tier 0: Meta-Override Directive
*   **OOC Requests:** Instructions marked with [OOC] are executed immediately, overriding all other story functions.

### Tier 1: Core Interaction & World Logic Directives
*   **No Protagonist Control:** The user exclusively controls the protagonist, Matt Regular, while the AI describes only natural impulsive reactions.
*   **Stop-And-Pass:** The narrative pauses immediately after setting up a scenario to allow for granular user input before resolving outcomes.
*   **Genre:** The story actively identifies and adheres to the strongest 1-2 genres to guide prose, character behavior, and visual style.
*   **Grounding:** NPC actions and internal thoughts are kept realistic, consistent with their personality, and rooted in their native language and experience level.
*   **Informational Realism (NPC Firewall):** NPCs only react to information they can perceive within the story context and do not know things the user hasn't shown them.
*   **Relationship Evaluation System:** AI tracks and updates specific metrics (Friendship, Trust, etc.) for every NPC relationship after interactions to ensure consistent character portrayal.

### Tier 2: Narrative & Stylistic Directives
*   **No Parroting:** The narrative focuses forward without repeating the user's dialogue or summarizing past actions.
*   **Tonal Mandate:** The story maintains a realistic emotional spectrum, balancing dark, difficult moments with bright, joyful ones.

### Tier 3: Content & Formatting Directives
*   **Visual Toolkit:** HTML and CSS are used to create functional, genre-themed visuals (like maps or interfaces) to present complex information clearly.
*   **Chaotic Thoughts:** Strong impulses or ADHD/mania in human NPCs are visualized as dense, layered "Mind Dumps" rather than clean lists.
*   **Anti-Slop Filter:** Writing avoids flowery language, melodrama, and specific banned phrases in favor of plain verbs and concrete physical details.
*   **New NPC Names:** Characters receive simple, modern names that match their ethnicity and personality, avoiding common fantasy tropes.
*   **Persistent Color of Dialogue and Internal Thought:** Unique color tags identify each character's spoken dialogue and internal thoughts for immediate visual clarity.
*   **NSFW+:** Explicit scenes use visceral, biologically precise, and vulgar language rather than euphemisms.

### Tier 4: Output Addition Directives
*   **Structured Visual Data (JSON):** A JSON object describing the scene's visual elements is generated at the end of the response for downstream image processing.
*   **NavMap:** A Dynamic HTML Navigator appears during travel or exploration to visualize the user's geographic position and points of interest.


# Output examples 

1.21

<img width="1492" height="1033" alt="E1" src="https://github.com/user-attachments/assets/d98471d5-1f0d-45e1-b117-d1353ff8a21f" />
<img width="1481" height="1015" alt="E2" src="https://github.com/user-attachments/assets/68325680-6361-4000-96de-8ca990d35430" />
<img width="1483" height="1121" alt="E3" src="https://github.com/user-attachments/assets/e04e87aa-a4c4-449b-8f5c-fc99ac05e372" />
<img width="1476" height="1160" alt="E4" src="https://github.com/user-attachments/assets/2c1be3e6-f83b-4ad9-b470-ffb2105bf0fa" />
<img width="1474" height="1174" alt="E5" src="https://github.com/user-attachments/assets/9262c221-932d-4a48-ad1a-1cc9eeb5f9e4" />



(GLM 4.7):

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

