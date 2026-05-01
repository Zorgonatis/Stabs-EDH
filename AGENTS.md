# Stabs-EDH — Agent Release Process

This project is a SillyTavern chat completion preset (JSON) with supporting documentation. This file describes the end-to-end release process for new versions.

---

## Project Structure

```
Stabs-EDH/
├── Stabs-GLM5.1-Directives-v2.6X.json   # Current release (root)
├── old/                                   # Archived versions
├── CHANGELOG.md                           # Version history (newest first)
├── README.md                              # User-facing documentation
├── UPDATE_PROCESS.md                      # Diffing methodology (reference)
├── AGENTS.md                              # This file
└── assets/                                # Assistant avatar images
```

## Naming Convention

Preset files follow: `Stabs-GLM{version}-Directives-v{MAJOR}.{MINOR}.{PATCH}.json`

---

## Release Workflow

### 1. User drops new preset version into root

The user places the updated JSON file alongside the current one. Both versions coexist during review.

### 2. Full Review (using review guidelines)

The review skill document lives at: `~/Documents/Skills/st-03-ai-review-guidelines.md`

Load it before reviewing. It defines the review levels, validation rules, and output format.

#### Review checks to perform:

**Format validation:**
- Valid JSON (parse with `python3 -c "import json; json.load(open(...))"`)
- No missing required fields on built-in prompt identifiers (`main`, `jailbreak`, `nsfw`, `chatHistory`, etc.)
- No invalid enum values on `names_behavior`, `injection_position`, etc.

**Prompt activation audit (critical distinction):**
SillyTavern has three tiers of "enabled":
1. **In `prompts[]` array** — prompt definition exists. The `enabled` field in the prompt object is **irrelevant for custom prompts**.
2. **In `prompt_order[].order[]`** — prompt is listed and user-toggleable.
3. **`enabled: true` in prompt_order** — prompt is **actually active** and sent to the model.

Only tier 3 prompts should be checked for conflicts, variable chains, and token budget.

**Variable chain integrity:**
- Extract all `{{getvar::X}}` calls from all prompts (including disabled — they document intent)
- Extract all `{{setvar::X}}` and `{{addvar::X}}` from **enabled prompts only**
- Verify every getter has a matching enabled setter
- Check reset headers appear early in prompt_order (before the setters that populate them)
- Flag inconsistent macro syntax (e.g., `.vtk_on` vs `{{getvar::vtk_on}}` for the same variable)

**Content conflicts (enabled prompts only):**
- Contradictory instructions between enabled directives
- Duplicate functionality across tiers
- Macro validity (`{{original}}` outside system_prompt/PHI, handlebars in wrong context)

**Inactive prompt audit:**
- Any prompt in `prompts[]` but NOT in `prompt_order[]` is inactive waste — flag for removal
- Cross-reference against changelog for deprecated/superseded status

#### Review output format:
```
# Review: [filename]
## Summary — Status: PASS | WARN | FAIL
## Critical Issues (C1, C2...)
## Warnings (W1, W2...)
## Info (I1, I2...)
## Performance Notes
```

### 3. Apply fixes

User confirms which issues to address. Common actions:

**Surgical prompt removal:**
```python
python3 -c "
import json
with open('preset.json') as f: data = json.load(f)
in_order = set()
for po in data['prompt_order']:
    for e in po['order']: in_order.add(e['identifier'])
data['prompts'] = [p for p in data['prompts'] if p['identifier'] in in_order]
with open('preset.json', 'w') as f: json.dump(data, f, indent=4, ensure_ascii=False)
"
```
Always verify: no ghost references (prompt_order entries pointing to removed prompts), valid JSON, 1:1 match between prompts and prompt_order.

### 4. Version diff

Run a structured diff between old and new versions:
- Content changes (prompt `content` field diffs)
- Name renames (same identifier, different name)
- Toggle changes (enabled/disabled flips in prompt_order)
- Top-level config changes (sampling params, provider fields, etc.)

Python diff script pattern:
```python
import json
with open('old.json') as f: old = json.load(f)
with open('new.json') as f: new = json.load(f)
old_p = {p['identifier']: p for p in old['prompts']}
new_p = {p['identifier']: p for p in new['prompts']}
# Compare content, names, prompt_order toggles, top-level config
```

### 5. Update documentation

**CHANGELOG.md** — Prepend new entry before the previous version:
```markdown
## [X.Y.Z] - YYYY-MM-DD

### Added
### Changed
### Removed
### Configuration
```

**README.md** — Update to match current preset state:
- File reference (filename in GLM-5.1 Preset section)
- Enabled by Default table (if toggle defaults changed)
- Directive descriptions (if new/removed/renamed)
- GLM-5.1 section (provider config as recommendations, not shipped defaults)
- Extra Assistants list (if assistants added/removed)
- Brain Power / Perspective notes (if toggle behavior changed)

**Important:** Provider-specific config fields (custom_model, custom_url, etc.) should be documented as **recommendations** in the README, not as shipped defaults. The preset omits these fields so import doesn't overwrite user settings.

Delegate changelog and README updates to writing agents in parallel for speed.

### 6. Archive and publish

```bash
# Move old version to archive
mv Stabs-GLM5.1-Directives-v{OLD}.json old/

# Stage, commit, push
git add -A
git commit -m "Release vX.Y.Z - <brief summary>"
git push
```

### 7. Community announcement

Draft a short summary suitable for Discord/Reddit covering:
- What changed (user-facing)
- Why (motivation for removals/changes)
- Link to GitHub release
- Discord invite link

---

## Key Reference Files

| File | Purpose |
|------|---------|
| `~/Documents/Skills/st-03-ai-review-guidelines.md` | Full review methodology and validation rules |
| `UPDATE_PROCESS.md` | Diffing methodology and grep patterns |
| `CHANGELOG.md` | Version history — first entry is current |

## Common Gotchas

- **Read tool truncation:** Long JSON lines (prompt `content` fields) truncate at 2000 chars. Always use `python3` to extract full content when checking for specific macros or patterns.
- **`vtk_on` variable:** Set in the Visual Toolkit prompt (end of content), NOT in WebDev. Cleared in the Perspective reset header each turn.
- **Variable reset order:** The Perspective header prompt resets all variables to empty. It MUST appear before the toggle prompts that set values in prompt_order.
- **Provider config:** Never ship provider-specific fields in distributable presets. Users configure their own API connections.
