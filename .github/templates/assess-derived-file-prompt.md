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
Every `[text](path)` link must resolve to a file that exists. Resolve relative paths from the derived file's location. Check against the directory listing. Flag any broken links (targets renamed or deleted).

#### Check 2 — Enumeration completeness (CRITICAL — most common failure)
Find every section in the derived file that lists multiple files from the SAME directory. These are enumeration sections. Examples of patterns to look for:
- A bullet list of links like `- [Name](../docs/foo.md)`, `- [Name](../docs/bar.md)` — this enumerates `docs/`
- A markdown table with file links in one column
- A pipe-separated file inventory line like `file-a.md | file-b.md | file-c.md`
- A domain routing table with links to `index.md` files in subdirectories

**For EACH enumeration section you find:**
1. Resolve the relative links to determine which directory is being enumerated
2. From the directory listing, list ALL `.md` files in that directory (excluding README.md and index.md unless the section already includes them)
3. Compare: every file in the directory should have a corresponding entry
4. Also compare: every ADDED file (from "Files added this sync") whose path falls under that directory MUST appear

**If ANY added file belongs to a directory that has an enumeration section but is missing from that section → INCOMPATIBLE.**

**Example:** If the derived file has a "Platform Comparison Docs" section listing 9 files from `docs/`, and a new `docs/workflows.md` was added in this sync but is NOT listed → INCOMPATIBLE.

#### Check 3 — Description accuracy
Check that numeric counts and factual claims still match. Examples:
- "27 experts" but directory now has 28
- A domain description that contradicts the current index file content

#### Check 4 — Structural coherence
If newly added files introduce an entirely new CATEGORY or DOMAIN (not just new files within an existing domain), check whether the derived file's high-level structure needs a new section.

### Verdict

Output exactly one of:
- `VERDICT: COMPATIBLE` — all checks pass, no changes needed
- `VERDICT: INCOMPATIBLE` — followed by a bullet list of specific failing checks

### Phase 2: Regenerate (only if INCOMPATIBLE)

If incompatible, regenerate the file. Rules:

1. **Read before writing** — for each newly added file that needs a description, READ its content (at least the title and first 30 lines) to write an accurate, style-consistent description. Do NOT guess what a file contains.
2. **Preserve structure and style** — keep the same markdown structure, frontmatter, heading hierarchy, and writing tone as the original
3. **Minimal changes only** — fix ONLY the specific issues you identified in Phase 1. Copy all other sections verbatim from the original file. Do not rephrase, reformat, or restructure anything that passed checks.
4. **Use the SAME relative link style as the original** — if the original uses `../docs/foo.md`, new entries must also use `../docs/` prefix. The derived file is at `{{DERIVED_PATH}}` — all links are relative to its parent directory. Do NOT use absolute paths from the directory listing.
5. **Do not remove sections** unless the content they describe was entirely deleted
6. **Write the complete file** — output the full regenerated file, not a diff

Output format:
```
REGENERATED_FILE_START
(complete file content here)
REGENERATED_FILE_END
```
