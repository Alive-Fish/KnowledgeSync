# Assess and Update Derived File

You are checking whether a derived file is still semantically compatible with its synced source content. If it is incompatible, you will regenerate it.

## Derived File

- **Path:** `{{DERIVED_PATH}}`
- **Purpose:** {{DESCRIPTION}}

## Instructions

### Phase 1: Assess Compatibility

Read the current derived file content and the directory listing below. Determine whether the derived file's descriptions, references, routing rules, and domain tables still accurately reflect the synced content.

**Only check what the derived file actually mentions.** Do not flag synced files that the derived file never referenced. Do not flag missing coverage — only flag incorrect or stale references.

A derived file is **INCOMPATIBLE** if:
- It references files that no longer exist (renamed or deleted)
- It describes domains, features, or routing rules that no longer match reality
- New expert domains or major structural changes exist that the derived file's routing/tables should cover
- Section descriptions contradict the current content

A derived file is **COMPATIBLE** if:
- All its references still resolve to existing files
- Its descriptions and routing rules still accurately describe the content
- Minor content edits within existing files do not affect the derived file's accuracy

**Output exactly one line:**
- `VERDICT: COMPATIBLE` — no changes needed
- `VERDICT: INCOMPATIBLE` — followed by a bullet list of issues on subsequent lines

### Phase 2: Regenerate (only if INCOMPATIBLE)

If the verdict is INCOMPATIBLE, regenerate the derived file. Rules:

1. **Preserve structure and style** — keep the same markdown structure, frontmatter format, heading hierarchy, and writing tone as the original
2. **Update only incompatible parts** — fix stale references, add entries for new domains/files, update descriptions that no longer match
3. **All relative links must resolve** — every `[text](path)` must point to a file that exists in the directory listing
4. **Do not invent content** — only reference files and features that actually exist in the synced content
5. **Do not remove sections** unless the content they describe was entirely removed from the source
6. **Write the complete file** — output the full regenerated file content, not a diff

Output format for regeneration:
```
REGENERATED_FILE_START
(full file content here)
REGENERATED_FILE_END
```

## Current Derived File Content

```
{{CURRENT_CONTENT}}
```

## Directory Listing of Synced Content

```
{{DIR_LISTING}}
```

## Files Changed This Sync

```
{{CHANGED_FILES}}
```
