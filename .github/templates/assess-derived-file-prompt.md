# Assess and Update Derived File

You are checking whether a derived file needs updating after upstream content was synced. If incompatible, regenerate it.

## Target

- **Path:** `{{DERIVED_PATH}}`
- **Purpose:** {{DESCRIPTION}}

## Context

### Files added this sync (NEW — did not exist before)

```
{{ADDED_FILES}}
```

### Files modified this sync

```
{{MODIFIED_FILES}}
```

### Directory listing of all synced content

```
{{DIR_LISTING}}
```

## Instructions

### Phase 1: Assess Compatibility

Read the derived file at `{{DERIVED_PATH}}`. Run ALL of these checks:

#### Check 1 — Reference integrity
Every `[text](path)` link must resolve to a file that exists. Check against the directory listing. Flag any broken links (targets renamed or deleted).

#### Check 2 — Enumeration completeness (CRITICAL)
Identify sections that **enumerate files from a directory** — for example:
- A bullet list of all docs (`docs/*.md`) with descriptions
- A table of expert domains with links to index files
- A file inventory line listing all experts in a domain

For EACH such enumeration section:
1. Determine which directory/directories it covers
2. List ALL `.md` files in that directory from the directory listing
3. Check if every file has a corresponding entry in the enumeration
4. **Any newly ADDED file that belongs to an enumerated directory but is missing from the list is an INCOMPATIBILITY**

This is the most common failure mode. When the derived file maintains a curated list and new files are added upstream, they MUST appear in the list.

#### Check 3 — Description accuracy
Check that counts, descriptions, and factual claims still match reality. Examples:
- "27 experts" but directory now has 28
- A domain description that no longer matches its index file's content

#### Check 4 — Structural coherence
If newly added files introduce an entirely new CATEGORY or DOMAIN (not just new files within an existing domain), check whether the derived file's high-level structure needs updating.

### Verdict

Output exactly one of:
- `VERDICT: COMPATIBLE` — all checks pass, no changes needed
- `VERDICT: INCOMPATIBLE` — followed by a bullet list of specific failing checks

### Phase 2: Regenerate (only if INCOMPATIBLE)

If incompatible, regenerate the file. Rules:

1. **Read before writing** — for each newly added file that needs a description, READ its content (at least the title and first 30 lines) to write an accurate, style-consistent description. Do NOT guess what a file contains.
2. **Preserve structure and style** — keep the same markdown structure, frontmatter, heading hierarchy, and writing tone as the original
3. **Update only incompatible parts** — fix broken links, add missing entries, update stale descriptions. Do not rewrite sections that passed all checks.
4. **All links must resolve** — every `[text](path)` must point to an existing file in the directory listing
5. **Do not remove sections** unless the content they describe was entirely deleted
6. **Write the complete file** — output the full regenerated file, not a diff

Output format:
```
REGENERATED_FILE_START
(complete file content here)
REGENERATED_FILE_END
```
