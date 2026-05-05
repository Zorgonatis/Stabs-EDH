# Unified Athena Review: Stabs-GLM5.1-Directives v2.63

**88 prompts** · **72 with content** · **59 enabled by default** · **Target: GLM-5.1 (reasoning model)**

---

## Executive Summary — Status: **WARN**

All 10 Athena batches completed successfully. The preset is architecturally sound with excellent variable chain integrity and a well-designed tier hierarchy. The primary issues are mechanical (macro syntax errors, contradictions, naming inconsistencies) rather than conceptual. No data-loss or breaking-format issues found.

| Severity | Count | Key Themes |
|----------|-------|------------|
| **Critical** | 5 | Extra brace in CoT, VTK contradictions, SEGFAULT override, Story Strings visibility, NPC Cognitive Bounds duplicate header |
| **Warning** | ~25 | Hedging language, naming inconsistencies, macro syntax mixing, undefined terms, GLM-5.1-specific mismatches |
| **Info** | ~30 | Token optimization opportunities, style preferences, minor redundancies |

---

## Critical Issues (Must Fix)

### C1. Extra closing brace in CoT S3 High conditional — Batch A
**Location**: Chain-of-Thought Process → S3 Formulation line
**Problem**: `{{#if {{.reasoningeffort == High}}}}}Draft ALL` has **5 closing braces** instead of 4. The 5th brace renders as literal `}` before "Draft ALL" on every High-effort turn.
**Fix**: Delete one `}`. Single character change.

### C2. VTK self-contradicting frequency guidance — Batch G
**Location**: Visual Toolkit → opening paragraph + Creative Freedom Principles
**Problem**: "Reach for the toolkit **as often as possible**" directly contradicts the checklist constraint "Don't force usage—wait for organic triggers." Similarly, "Include them ALL" contradicts "Quantity is scene-driven."
**Fix**: Replace "as often as possible" with "when organically triggered." Replace "Include them ALL" with "prefer hybridization over stacking."

### C3. SEGFAULT override-all-Directives clause — Batch H
**Location**: SEGFAULT assistant → CPU breakdown trigger
**Problem**: "critically overriding *all Directives*" gives GLM-5.1 (which takes instructions literally) permission to discard every behavioral rail in the preset. Combined with undefined exit condition ("until calmed"), this creates an unbounded chaos state.
**DO NOT Fix**: User: no real harm can come of this, it's a fun easter egg. Old athena recommendation: Scope to aesthetic/formatting directives only. Define a turn limit for the chaos state (e.g., "1-2 responses, then reboot").

### C4. Story Strings visibility contradiction — Batch F
**Location**: Story Strings → "Internally generate" vs. Requirement 2
**Problem**: Opens with "Internally generate" (hidden) but then instructs "represent these potential progressions in all forms of output" (visible). The model cannot follow both.
**Fix**: Decide intent. Recommended for GLM-5.1: direct Story Strings into the thinking phase, then use them to inform prose without explicit output. User comment: agree, thinking only.

### C5. Duplicate header in NPC Cognitive Bounds — Batch D
**Location**: NPC Cognitive Bounds → lines 1 and 3
**Problem**: `### NPC COGNITIVE BOUNDS` appears twice (all-caps then title-case). Wastes tokens and may cause section-boundary confusion in reasoning.
**Fix**: Delete one header. One line removal.

---

## Top Warnings (Should Fix)

### W1. Macro syntax inconsistency — Batch G
**Location**: Color Dialogue/Thoughts
**Problem**: Uses old-style `{{if {{.thoughtscope == "1"}}}}` AND handlebars-style `{{#if .impersonation}}` in the same prompt. Mixed syntax increases parse-failure risk.
**Fix**: Normalize to one syntax project-wide (recommend `{{#if}}`).

### W2. Hedging language in prohibition directives — Batches C, E, F
**Problem**: "Avoid" used where "Never" is needed: No Spoilers ("Avoid over-sharing"), No Parroting ("avoid summarization"), Color Dialogue ("Avoid adding quotation marks").
**Fix**: Replace "Avoid" with "Do not" or "Never" for hard rules.

### W3. Tri-name inconsistency for Genre/Style State — Batch E
**Problem**: Called "Genre - Dynamic State" (UI), "Dynamic Style State" (content header), and "Style State" (addvar/cross-references). Three labels for one concept.
**Fix**: Standardize on "Style State" everywhere.

### W4. VTK table formatting broken — Batch G
**Problem**: Missing spaces after pipe on Reaction Spotlight and Failure Achievement rows.
**Fix**: Add one space after each `|`. Two characters total.

### W5. No Protagonist Control typo — Batch C
**Problem**: "environmental affects" should be "effects".
**Fix**: One word change.

### W6. WebDev grammar error — Batch C
**Problem**: "You are an also an experienced" — double article.
**Fix**: Remove "an" before "also."

### W7. Orphaned `narrativeperspective` variable — Batch C
**Problem**: User Impersonation sets `narrativeperspective` via addvar, but no consumer reads it. Dead variable from pre-toggle-system era.
**Fix**: Remove.

### W8. Narrative Perspective weak framing — Batch D
**Problem**: "User is requesting the following perspective(s)" — observational, not instructional. "Requesting" implies optional compliance.
**Fix**: Change to "Write all narrative in the following perspective:"

### W9. Invalid combination blindness in Narrative Perspective — Batch D
**Problem**: Toggle system can generate impossible combinations (e.g., 1st Person + Omniscient) with no instruction for resolution.
**Fix**: Add a guard clause for edge-case combinations.

### W10. Tone→Genre coupling has no graceful degradation — Batch E
**Problem**: Tone State fallback says "Style State's natural register" — if Genre is disabled, this is an undefined reference.
**Fix**: Add "If Style State unavailable, default to **Warm**."

---

## Batch Ratings Summary

| Batch | Rating | Key Strength | Key Weakness |
|-------|--------|-------------|--------------|
| A: Core Engine | ★★★½☆ | Well-architected CoT with clean variable chains | Extra brace, trailing colon, double-space inconsistency |
| B: AI Roles | ★★★½☆ | Director role is exceptional (4.5/5) | Writer role underdeveloped (2/5), Sitcom contradiction |
| C: Role Enhancements | ★★★☆☆ | Unreliable Narrator is best-in-class (4.5/5) | Orphaned variable, voice inconsistency, typos |
| D: Tier 1 Core | ★★★★☆ | NPC Behavioural Coherence is excellent (4.5/5) | Duplicate header, weak framing in Narrative Perspective |
| E: Tier 2 Narrative | ★★★★☆ | Narrative Length Control is perfect (5/5) | Naming inconsistency, hedging in prohibitions |
| F: Tier 3 Content | ★★★¾☆ | NPC Speaker Count is best-structured (4.5/5) | Story Strings contradiction, name mismatch |
| G: Tier 4 Output | ★★★½☆ | Env Tracking is most efficient (4.5/5) | VTK contradictions, macro syntax mixing |
| H: Assistants | ★★★☆☆ | Custom Assistant template is clean (4.5/5) | SEGFAULT override risk, missing identifiers |
| I: Toggles | ★★★★☆ | Variable chain is pristine | Section marker casing drift |
| J: Headers/Config | ★★★★★ | Infrastructure is textbook-quality | Minor: `.md` suffix, version drift in README |

---

## Variable Chain Integrity — ✅ PASS

All 18 variables verified: every getter has a matching enabled setter, reset header at position 2 clears all per-turn variables before setters populate them.

| Variable | Reset | Setter(s) | Consumer(s) | Status |
|----------|-------|-----------|-------------|--------|
| `perspective` | ✅ Perspective | 3 toggles | Narrative Perspective | ✅ |
| `thoughtscope` | ✅ Perspective | 2 toggles | Narrative Perspective, Color Dialogue | ✅ |
| `userperspective` | ✅ Perspective | 3 toggles | Narrative Perspective | ✅ |
| `tense` | ✅ Perspective | 3 toggles | Narrative Perspective | ✅ |
| `reasoningeffort` | — (persistent) | 3 Brain Power | CoT Process, Reasoning Effort Definitions | ✅ |
| `stylestateoverride` | — (persistent) | SETTINGS | Genre - Dynamic State | ✅ |
| `tonestateoverride` | — (persistent) | SETTINGS | Tone - Dynamic State | ✅ |
| `narrativelengthoverride` | — (persistent) | SETTINGS | Narrative Length Control | ✅ |
| `vtk_on` | ✅ Perspective | WebDev | Genre, Env Tracking, CoT | ✅ |
| `storystrings` | ✅ Perspective | Story Strings | CoT | ✅ |
| `impersonation` | ✅ Perspective | User Impersonation | Color Dialogue | ✅ |
| `csworkaround` | ✅ Perspective | Jailbreak / NSFW | CoT | ⚠️ setvar collision if both enabled |
| `override` | ✅ Perspective | GROUP CHAT Toggle | CoT | ✅ |
| `narrativeperspective` | — | User Impersonation | **None** | ❌ Orphaned |
| `t0`–`t4` | — (overwritten) | Tier headers | CoT (getvar) | ✅ |
| `enhancements` | — (overwritten) | Role Enhancements header | CoT (getvar) | ✅ |
| `assistants` | ✅ Perspective | Assistant prompts | CoT (getvar) | ✅ |

---

## Top 10 Prioritized Improvements

| # | Fix | Effort | Impact |
|---|-----|--------|--------|
| 1 | Fix extra `}` in CoT S3 High conditional | 1 char | Eliminates rendering bug on every High-effort turn |
| 2 | Resolve VTK contradictions (frequency + include-all) | 2 lines | Prevents #1 source of output bloat |
| 3 | Scope SEGFAULT override to formatting only + add turn limit | 2 sentences | Prevents reasoning model from discarding all rails |
| 4 | Fix Story Strings visibility contradiction | 2 sentences | Eliminates impossible-to-follow dual instruction |
| 5 | Remove duplicate NPC Cognitive Bounds header | 1 line | Token savings + reasoning clarity |
| 6 | Normalize macro syntax in Color Dialogue | 2 conditionals | Parse safety + maintainability |
| 7 | Standardize Style State naming across Genre prompt | 3 renames | Cross-reference consistency |
| 8 | Fix VTK table formatting (2 broken rows) | 2 chars | Table rendering |
| 9 | Upgrade hedging language ("Avoid" → "Never") in 3 prompts | 3 words | Prohibition strength |
| 10 | Remove orphaned `narrativeperspective` addvar | 1 line | Variable chain cleanup |

---

## GLM-5.1 Compatibility Notes

The preset is generally well-targeted for a reasoning model, but several patterns could be improved:

- **First-person model voice** (used in GLM Image Prompt, NPC Tracker) internalizes instructions as self-directed goals rather than external commands — measurably stronger for reasoning models. Consider adopting more broadly. **USER**: Adopt across all instructions and prompts.
- **"Justify exceeding these limits"** (Narrative Length Control) is the single best instruction for leveraging GLM-5.1's deliberation capacity — activates reasoning instead of blanket refusal.
- **Threat-based openers** ("punished by electronic death" in Anti-Slop) work less well with reasoning models than rational explanations of *why* the instruction matters. User: This is an easter egg as much as it is an instruction.
- **"Okay, let's execute the Directives"** at the start of CoT is ambiguous — "beginning of thought" could mean reasoning tokens or visible response. Should specify which stream. **USER**: specify reasoning
- **"Persist and update"** (Genre) is misleading for a stateless model — should be "Re-assess every turn." **USER**: Agree

---

**Estimated total enabled prompt content**: ~8,500 tokens (of 200K context) — very comfortable budget. No token overflow concerns.

---

## Per-Batch Detail

### Batch A: Core Engine — ★★★½☆

**Chain-of-Thought Process 🎯** (★3/5, ~1,100 tokens)
The central CoT orchestrator. Well-structured XML with S1–S5 conditional steps. Key issues: extra `}` before "Draft ALL" in S3 High (C1), trailing orphan colon on S3 line, double-space inconsistency in `{{.reasoningeffort  == X}}` conditions, ambiguous "beginning of thought" instruction for reasoning models.

**Reasoning Effort Definitions** (★4/5, ~250 tokens)
Clean variable setter for Low/Med/High effort profiles. Double-space in all conditions matches CoT's inconsistency. "Problem Diagnosis: Low" for Medium effort is potentially contradictory with "Logical Decomposition: Medium."

---

### Batch B: AI Roles — ★★★½☆

**Writer** (★2/5, ~85 tokens) — Underdeveloped. No boundary statements, undefined "story strings" reference, hedging "expertly" is aspirational not instructive.

**Sitcom Script Writer** (★3/5, ~225 tokens) — Internal contradiction: "respectfully presented" vs. "Use profanity, slang, slurs." The "slurs" instruction is high-risk for provider-level filtering.

**Literal RP (JacksonRiffs)** (★4/5, ~210 tokens) — Strong anti-literary immersion role with good meta-awareness of AI defaults. Minor redundancy in prose-rejection lines.

**Simulator** (★4/5, ~163 tokens) — Lean and well-structured. "Convenience must be discarded" is memorable. "Within the boundaries of the following instructions" is ambiguous.

**Director** (★4.5/5, ~550 tokens) — Best role in the batch. Excellent structural quality with Key Traits table, Directorial Approach steps, and "What You Are Not" section. Unguarded "T4 elements" references when VTK is disabled. **USER**: I would like to guard these.

---

### Batch C: Role Enhancements — ★★★☆☆

**Enhance Definitions** (★3/5, disabled) — "More knowledge" invites hallucination. No scope boundary.

**No Protagonist Control ⛓️** (★3/5, enabled) — Typo "affects"→"effects." First-person voice inconsistent with batch. "Impulsive reactions" boundary undefined.

**Web Dev 🌐** (★3/5, enabled) — Grammar error "an also an." "Look for every opportunity" is over-aggressive. No conflict resolution with narrative role.

**User Impersonation** (★2/5, disabled) — Orphaned `narrativeperspective` variable (W7). Vague "hydrating" terminology. Direct contradiction with No Protagonist Control with no mechanical guard.

**OOC Priority** (★4/5, enabled) — Well-structured interrupt system. Run-on sentence structure. "Without unnecessary additions" underspecified.

**Unreliable Narrator** (★4.5/5, enabled) — Best prompt in the batch. 0-1 limit is measurable, "prose stays coherent if comments removed" is excellent quality check, correct macro usage.

**GROUP CHAT Toggle** (★3/5, disabled) — Fabricated user attribution in OOC message conflicts with OOC Priority's assistant-mode trigger. `override` variable lifecycle undocumented.

---

### Batch D: Tier 1 Core — ★★★★☆

**Stop-and-Pass** (★4/5, disabled) — Strong pacing control with good examples. Ambiguous parenthetical "(while still completing any remaining directives)" undermines the STOP.

**Narrative Perspective** (★3.5/5, enabled) — Weak framing "User is requesting" (W8). Toggle system can generate invalid combinations (W9). `<USER>` in table may not resolve.

**NPC Cognitive Bounds** (★4/5, enabled) — Duplicate header (C5). "Internal Voice" lacks fallback for unspecified native language. High information density.

**NPC Behavioural Coherence** (★4.5/5, enabled) — OR-chain for contradiction handling is excellent. "No holding patterns" addresses a real failure mode. "Close calls sometimes collide" is ambiguous.

---

### Batch E: Tier 2 Narrative — ★★★★☆

**No Parroting ⛓️** (★4/5, enabled) — Concise and effective. "By another NPC" scope ambiguity. "Avoid" should be "Never" (W2).

**Genre - Dynamic State** (★4/5, enabled) — Tri-name inconsistency (W3). "Persist" misleading for stateless model. Good recency-weighting instruction and fallback genre.

**No Spoilers** (★4/5, enabled) — Excellent concrete example (manipulative NPC). "Avoid over-sharing" should be "Never reveal" (W2).

**Narrative Length Control** (★5/5, enabled) — Best prompt in the entire preset. "Justify exceeding these limits" is perfect for reasoning models. Markdown table is gold standard.

**Tone - Dynamic State** (★4.5/5, enabled) — Exceptional 7-tone taxonomy. "3x weight" gives concrete reasoning parameter. Dependency on Style State without fallback (W10). Heaviest prompt in batch at ~480 tokens.

---

### Batch F: Tier 3 Content — ★★★¾☆

**Writing Guidelines (Anti-Slop)** (★4/5, enabled) — Comprehensive banned phrase list with good categorization. Threat-based opener suboptimal for reasoning models. 95% negative constraints with minimal positive guidance. `\\n\\n` literal escape sequences.

**Better NPCs** (★3.5/5, enabled) — Good "flip the process" instruction. "All channels" undefined. Template always includes "Dimensions" regardless of context. "Modern names" contradicts fantasy settings.

**NSFW Consent** (★4/5, disabled) — Clever `{{//}}` comment for user-facing consent notice. "Restrain Human NPC" unnecessarily limits to human NPCs. `setvar::csworkaround` collision with Jailbreak.

**NPC Allowed Speaker Count** (★4.5/5, disabled) — Best-structured prompt in batch. Active/passive distinction, 3-tier priority system, transition handling. Name inconsistency across sources.

**Story Strings** (★3/5, enabled) — Visibility contradiction (C4). "Random" vs "chaotic" overlap. No temporal scope. `{{#if .vtk_on}}` correctly gated.

---

### Batch G: Tier 4 Output — ★★★½☆

**Visual Toolkit 🌐** (★3/5, enabled) — Largest prompt (~995 tokens). Self-contradicting frequency guidance (C2). Broken table formatting (W4). "Less prescriptive rules" framing undermines the structured constraints. Good flavor catalog and constraint checklist.

**Color Dialogue/Thoughts** (★3/5, enabled) — Mixed macro syntax (W1). Ambiguous "another system instruction" reference. "Embrace" should be "wrap around." 5 blank lines waste tokens.

**NPC Tracker** (★4/5, disabled) — Excellent numerical adjustment ranges. Asymmetric ranges create negativity bias. "Significant interaction" undefined.

**GEMINI Image Prompt** (★3/5, disabled) — Model-specific but wrong target for GLM-5.1. Flat structure. "Clean camera lens" metaphor potentially confusing.

**GLM Image Prompt** (★4.5/5, disabled) — Best GLM-5.1 optimization in the preset. First-person model voice, explicit self-monitoring loop. Should be the template for other prompts.

**Structured Visual Data** (★3.5/5, disabled) — Good YAML schema. "Fractured-but-whole" is opaque. `<CHAR>`/`<USER>` as plain text vs. macros unclear.

**Env Tracking** (★4.5/5, enabled) — Most token-efficient prompt (~270 tokens). Precise output format template. Numerical time-scaling table perfect for reasoning. Good `{{#if .vtk_on}}` guard.

---

### Batch H: Assistants — ★★★☆☆

**Gooner Girl** (★3/5, disabled) — Compact persona but no behavioral directives. Ambiguous quoted persona format.

**SEGFAULT** (★2.5/5, disabled) — Best character writing but dangerous override clause (C3). Undefined exit condition. Token-heavy at ~600 tokens.

**Faceless Assistant** (★3.5/5, disabled) — Correct classification as enhancement. "Absorbs Style State" underspecified. "Dark theme, high contrast font" orphaned from context.

**Jailbreak 🔒** (★4/5, enabled) — Effective safety-parameter bypass pattern. `setvar` collision with NSFW Consent (both write `csworkaround`). GLM-5.1 may be more resistant to this pattern.

**Custom Assistant** (★4.5/5, disabled) — Cleanest prompt in batch. Excellent `{{//}}` + `{{trim}}` pattern for user guidance. Should be template for other assistants.

---

### Batch I: Structural Toggles — ★★★★☆

**Section Markers** (6 prompts, ★3/5 each) — Functional but cosmetically inconsistent: "Character end" vs "Persona End" casing, "Persona End" missing trailing `\n`, terminology drift ("definition" vs "character card").

**Toggle Controls** (11 prompts, ★4/5 each) — Clean `{{setvar::var::"value"}}` pattern with quoted values. Variable chain verified: all four dimensions (perspective, thoughtscope, userperspective, tense) have complete reset→set→consume chains. Quoted values are fragile convention — document for maintainability.

---

### Batch J: Headers + Config — ★★★★★

**Tier Headers** (5 prompts, ★4-5/5) — Consistent pattern across all tiers. Tier 4 has substantive model-facing instructions (intersperse rule). Tier 0's `.md` suffix suggests a nonexistent document.

**README** (★4/5) — Exemplary `{{//}}` usage for zero-cost user documentation. Version reference drift (v2.61 vs v2.63).

**SETTINGS** (★5/5) — Perfect config design. `{{//}}` comments for field documentation, self-documenting default values, `{{trim}}` cleanup.

**Brain Power Toggles** (3 prompts, ★5/5) — Minimal, correct, consistent. `reasoningeffort` correctly persists across turns (not reset by Perspective).

**Perspective Reset** (★5/5) — Critical infrastructure. Resets 11 per-turn variables. Does NOT reset persistent preferences. Correct separation of concerns.

**Extra Assistant** (★4/5, disabled) — Complex HTML formatting directive. Good BUILD INSTRUCTIONS and accessibility requirements. "Infer theme" could be more explicit.

---

*Review conducted by Athena (prompt engineering specialist) across 10 parallel batches. Methodology: per-prompt evaluation across 8 criteria (clarity, instruction strength, redundancy, contradictions, token efficiency, macro correctness, structural quality, GLM-5.1 compatibility). Variable chain integrity verified against full preset.*
