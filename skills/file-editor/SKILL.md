---
name: "file-editor"
description: "Teaches how to use Write tool for file editing: full write, SEARCH/REPLACE, append to end, insert at start. Invoke when editing files. IMPORTANT: This skill is TRAE-specific and uses TRAE's Write tool - not compatible with other agent environments."
---

# File Editor

This skill teaches how to use the Write tool for various file editing operations.

## Write Tool Overview

The Write tool is a powerful file editing tool that supports multiple modes:
- **Full Write**: Write complete file content
- **SEARCH/REPLACE**: Replace specific content (recommended)
- **Append**: Add content to the end
- **Insert**: Add content to the beginning

## Mode 1: Direct Full Write

Use when you want to write the entire file content.

### When to Use
- Creating new files
- Completely replacing file content
- When you have the full new content ready

### Steps
1. Read the existing file (required by Write tool)
2. Prepare the complete new content
3. Use Write tool with `file_path` and `content` parameters

### Example
```python
# 1. Read existing file
old_content = Read(file_path="path/to/file.txt")

# 2. Prepare new content (or use your own)
new_content = "This is the complete new content"

# 3. Write complete content
Write(file_path="path/to/file.txt", content=new_content)
```

### Important Notes
- Write tool requires reading the file first if it exists
- This will overwrite the entire file
- If you want to preserve existing content, use Mode 2 or Mode 3

---

## Mode 2: SEARCH/REPLACE (Recommended)

Use when you want to replace specific content within a file.

### When to Use
- Modifying specific sections of a file
- Updating function implementations
- Changing configuration values
- Any targeted edit

### Steps
1. Identify the content to replace (old_str)
2. Prepare the new content (new_str)
3. Use Write tool with `file_path`, `old_str`, and `new_str` parameters

### Example
```python
Write(
    file_path="path/to/file.txt",
    old_str="This is the old content that needs to be replaced",
    new_str="This is the new content"
)
```

### SEARCH/REPLACE Rules
1. `old_str` must be a contiguous chunk of lines
2. `new_str` is the replacement content
3. `new_str` MUST be different from `old_str`
4. Only replaces the first match occurrence
5. Include enough lines in `old_str` to uniquely match
6. Keep SEARCH and REPLACE sections concise
7. Include just the changing lines, plus a few surrounding lines if needed for uniqueness
8. Only create SEARCH/REPLACE for files user has added to chat
9. To move code within a file, use two separate operations: delete original, then insert in new location

### Advantages
- No need to read the entire file
- Precise, targeted edits
- Less risk of accidental changes
- More efficient for large files

---

## Mode 3: Append to End

Use when you want to add content to the end of a file.

### Method A: SEARCH/REPLACE (Recommended)

Replace the ending marker or last few lines.

#### Example 1: Replace ending marker
```python
Write(
    file_path="path/to/file.md",
    old_str="---",
    new_str="---\n\nNew content appended at the end"
)
```

#### Example 2: Replace last few lines
```python
Write(
    file_path="path/to/file.txt",
    old_str="Last line of the file",
    new_str="Last line of the file\n\nNew content appended"
)
```

### Method B: Read + Write

Read entire file, combine with new content, then write.

```python
# 1. Read existing content
old_content = Read(file_path="path/to/file.txt")

# 2. Combine with new content
combined_content = old_content + "\n\nNew content appended"

# 3. Write combined content
Write(file_path="path/to/file.txt", content=combined_content)
```

### When to Use Each Method
- **Method A (SEARCH/REPLACE)**: When you can identify a unique ending marker or last few lines
- **Method B (Read + Write)**: When you cannot identify a unique ending, or for simple files

---

## Mode 4: Insert at Start

Use when you want to add content to the beginning of a file.

### Method A: SEARCH/REPLACE (Recommended)

Replace the title or first few lines.

#### Example 1: Replace title
```python
Write(
    file_path="path/to/file.md",
    old_str="# File Title",
    new_str="# New Section\n\n# File Title"
)
```

#### Example 2: Replace first few lines
```python
Write(
    file_path="path/to/file.txt",
    old_str="First line of the file\nSecond line of the file",
    new_str="New content inserted at start\n\nFirst line of the file\nSecond line of the file"
)
```

### Method B: Read + Write

Read entire file, prepend new content, then write.

```python
# 1. Read existing content
old_content = Read(file_path="path/to/file.txt")

# 2. Prepend new content
combined_content = "New content inserted at start\n\n" + old_content

# 3. Write combined content
Write(file_path="path/to/file.txt", content=combined_content)
```

### When to Use Each Method
- **Method A (SEARCH/REPLACE)**: When you can identify a unique starting pattern (title, header, etc.)
- **Method B (Read + Write)**: When you cannot identify a unique starting pattern, or for simple files

---

## Mode 5: Delete Content

Use when you want to remove specific content from a file.

### SEARCH/REPLACE Method

Replace the content to delete with empty string or minimal content.

```python
# Delete specific section
Write(
    file_path="path/to/file.txt",
    old_str="Content to be deleted\nThis should be removed",
    new_str=""
)
```

### Example: Delete a section between markers
```python
Write(
    file_path="path/to/file.txt",
    old_str="<!-- START DELETE -->\nContent to delete\n<!-- END DELETE -->",
    new_str=""
)
```

---

## Mode 6: Move Content Within File

Use when you want to move content from one location to another.

### Steps
1. Delete content from original location
2. Insert content in new location

### Example
```python
# Step 1: Delete from original location
Write(
    file_path="path/to/file.txt",
    old_str="Content to move\nThis should be moved elsewhere",
    new_str=""
)

# Step 2: Insert in new location
Write(
    file_path="path/to/file.txt",
    old_str="Target location marker",
    new_str="Target location marker\n\nContent to move\nThis should be moved elsewhere"
)
```

---

## Best Practices

### Choosing the Right Mode

| Scenario | Recommended Mode | Why |
|----------|-----------------|-----|
| Create new file | Mode 1 (Full Write) | No existing content to preserve |
| Replace entire file | Mode 1 (Full Write) | Complete replacement needed |
| Edit specific section | Mode 2 (SEARCH/REPLACE) | Precise, efficient |
| Append to end | Mode 3A (SEARCH/REPLACE) | No need to read entire file |
| Insert at start | Mode 4A (SEARCH/REPLACE) | No need to read entire file |
| Delete content | Mode 5 (SEARCH/REPLACE) | Precise removal |
| Move content | Mode 6 (Two SEARCH/REPLACE) | Delete + Insert |

### General Guidelines

1. **Prefer SEARCH/REPLACE** over Read + Write for targeted edits
2. **Include enough context** in `old_str` to ensure unique matching
3. **Keep sections concise** - only include lines that need to change
4. **Verify uniqueness** - ensure `old_str` matches only one location
5. **Test on copy** - for critical files, test on a backup first
6. **Use Read + Write** when SEARCH/REPLACE is not feasible
7. **Always read first** when using Mode 1 (Full Write) on existing files

### Common Mistakes to Avoid

1. **Not reading first** when using Mode 1 on existing files (Write tool will fail)
2. **Too little context** in `old_str` (may match wrong location)
3. **Too much context** in `old_str` (harder to maintain, may fail if context changes)
4. **Forgetting newlines** when appending/inserting (use `\n\n` for paragraph breaks)
5. **Using Mode 1** when Mode 2 would be more appropriate (inefficient, risky)

---

## Troubleshooting

### Issue: Write tool fails with "must read file first"

**Cause**: You're using Mode 1 (Full Write) on an existing file without reading it first.

**Solution**: Read the file first, or use Mode 2 (SEARCH/REPLACE) instead.

### Issue: SEARCH/REPLACE replaces wrong content

**Cause**: `old_str` matches multiple locations or not specific enough.

**Solution**: Add more context to `old_str` to make it unique.

### Issue: SEARCH/REPLACE doesn't match anything

**Cause**: `old_str` doesn't exactly match the file content (whitespace, indentation, etc.).

**Solution**: Copy the exact content from the file, including whitespace and indentation.

### Issue: New content has wrong formatting

**Cause**: Forgot to include newlines (`\n`) when appending/inserting.

**Solution**: Use `\n\n` for paragraph breaks, `\n` for line breaks.

---

## Quick Reference

| Operation | Method | Parameters |
|-----------|---------|-------------|
| Full Write | Mode 1 | `file_path`, `content` |
| SEARCH/REPLACE | Mode 2 | `file_path`, `old_str`, `new_str` |
| Append to End | Mode 3A | `file_path`, `old_str`, `new_str` |
| Insert at Start | Mode 4A | `file_path`, `old_str`, `new_str` |
| Delete Content | Mode 5 | `file_path`, `old_str`, `new_str=""` |
| Move Content | Mode 6 | Two SEARCH/REPLACE operations |

---

## Examples Summary

### Full Write
```python
old_content = Read(file_path="file.txt")
new_content = "Complete new content"
Write(file_path="file.txt", content=new_content)
```

### SEARCH/REPLACE
```python
Write(file_path="file.txt", old_str="old", new_str="new")
```

### Append to End
```python
Write(file_path="file.txt", old_str="---", new_str="---\n\nappended")
```

### Insert at Start
```python
Write(file_path="file.txt", old_str="# Title", new_str="# New\n\n# Title")
```

### Delete Content
```python
Write(file_path="file.txt", old_str="delete me", new_str="")
```

### Move Content
```python
Write(file_path="file.txt", old_str="move me", new_str="")
Write(file_path="file.txt", old_str="here", new_str="here\n\nmove me")
```
