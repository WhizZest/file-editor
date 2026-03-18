# TRAE File Editor

A comprehensive skill that teaches how to use the Write tool for various file editing operations.

## Description

This skill provides detailed guidance on using the Write tool for file editing, including:
- Full write operations
- SEARCH/REPLACE functionality
- Append to end
- Insert at start

**IMPORTANT**: This skill is TRAE-specific and uses TRAE's Write tool - not compatible with other agent environments.

## Features

### Multiple Editing Modes

1. **Full Write** - Write complete file content
2. **SEARCH/REPLACE** - Replace specific content (recommended)
3. **Append** - Add content to the end
4. **Insert** - Add content to the beginning
5. **Delete** - Remove specific content
6. **Move** - Move content within a file

### When to Use

Invoke this skill when you need to:
- Edit files in any way
- Perform targeted edits
- Replace specific sections
- Add content to files
- Delete or move content within files

## Quick Examples

### SEARCH/REPLACE (Recommended)
```python
Write(
    file_path="path/to/file.txt",
    old_str="This is the old content",
    new_str="This is the new content"
)
```

### Append to End
```python
Write(
    file_path="path/to/file.md",
    old_str="---",
    new_str="---\n\nNew content appended at the end"
)
```

### Insert at Start
```python
Write(
    file_path="path/to/file.md",
    old_str="# File Title",
    new_str="# New Section\n\n# File Title"
)
```

## Documentation

For complete documentation, usage guidelines, and best practices, see [SKILL.md](skills/SKILL.md).

## Topics

- file-editing
- write-tool
- search-replace
- agent-skill
- code-editor

## License

This skill is part of the TRAE ecosystem and is designed to work specifically with TRAE's Write tool.
