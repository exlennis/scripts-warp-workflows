<!-- Warp Workflows Documentation Template -->
## [Action]_[Object]_[Context/Qualifier] <!-- Each entry starts with the workflow name using heading 2 following the standardised naming convention -->

### What it does
<!-- This section should mirror the AI-Tags used in the command comments -->
**PURPOSE**: [Concise description of command function] <!-- Mirror from command's PURPOSE comment -->
**CONTEXT**: [When to use this workflow] <!-- Mirror from command's CONTEXT comment -->
**COMPLEXITY**: [Basic/Intermediate/Advanced] <!-- Indicate whether this is Basic, Intermediate, or Advanced -->
**VERIFICATION**: [When required] <!-- Mirror from command's VERIFICATION comment if present -->

### Syntax
<!-- Include only the actual command with all arguments -->

```bash
[command with {{argument1}} {{argument2}}]
```

### Arguments
<!-- Use arguments only when required and stick to the supported types -->

| Argument | Type | Description | Default | Example Values |
|----------|----------|-------------|---------|----------------|
| `argument1`   | Text/Enum/wCommand   | Purpose of this argument | `default_value` | `example1`, `example2` |
| `argument2`   | Text/Enum/wCommand   | Purpose of this argument | `default_value` | `example1`, `example2` |

### Example Usage
**Basic Example(s)**

```bash
[Example of Simple command with minimal arguments]
```

**Advanced Example(s)**

```bash
[Example of complex command with multiple arguments]
```

### Example Output
<!-- Show what users can expect to see when running the workflow -->

```
[Sample output of the workflow]
```

### When to Use
- [Specific scenario where this workflow is useful]
- [Alternative use case]
- [Problem this workflow solves]

### Notes
- [Important considerations]
- [Scenarios where this workflow may not be appropriate]
- [Potential issues]
- [Best practices]

### See Also
<!-- References to related documentation, workflows, or resources -->
- [Related_Workflow_One](link-to-workflow)
- [Official Documentation](link-to-docs)
- [External Resources](link-to-resources)

---
*Last Updated: YYYY-MM-DD* <!-- Consistent version tracking -->

<!-- EXAMPLE IMPLEMENTATION -->

## Find_Files_BySize

### What it does
**PURPOSE**: Find largest files in specified directory
**CONTEXT**: Use when cleaning up disk space or analysing usage
**COMPLEXITY**: Basic
**VERIFICATION**: Checks directory existence and find command

### Syntax

```bash
find "{{target_directory}}" -type f -size +{{min_size}} -print0 | \
  xargs -0 du -h | sort -rh | head -n {{file_count}} | \
  awk '{printf "%-10s %s\n", $1, $2}'
```

### Arguments

| Argument | Type | Description | Default | Example Values |
|----------|----------|-------------|---------|----------------|
| `target_directory` | Dynamic Enum | Directory to search for large files | `.` | `~/Downloads`, `~/Documents` |
| `file_count` | Static Enum | Number of files to display | `10` | `5`, `10`, `20`, `50` |
| `min_size` | Static Enum | Minimum file size to include | `10M` | `1M`, `10M`, `100M`, `1G` |

### Example Usage
**Basic Example**

```bash
find "." -type f -size +10M -print0 | xargs -0 du -h | sort -rh | head -n 10 | awk '{printf "%-10s %s\n", $1, $2}'
```

**Advanced Example**

```bash
find "~/Projects" -type f -size +100M -print0 | xargs -0 du -h | sort -rh | head -n 20 | awk '{printf "%-10s %s\n", $1, $2}'
```

### Example Output

```
1.2G      ./Movies/vacation2023.mp4
650M      ./Downloads/software-installer.dmg
320M      ./Documents/project-archive.zip
120M      ./Pictures/photo-library.zip
90M       ./Music/playlist.mp3
```

### When to Use
- When disk space is running low and you need to identify large files
- Before system backups to reduce backup size by removing unnecessary large files
- When preparing an external drive and need to prioritise what to transfer based on size
- For investigating unexpected disk usage increases

### Notes
- This command focuses only on files, not directories
- Requires the `find`, `du`, `sort`, and `awk` commands to be available
- May take time to execute on large directories or network drives
- For very large file systems, consider adding more path exclusions
- Does not follow symbolic links by default - add `-L` to find command to change this

### See Also
- [Calculate_Usage_Directory](link-to-workflow)
- [Clean_Files_OlderThan](link-to-workflow)
- [Find command documentation](https://www.gnu.org/software/findutils/manual/html_mono/find.html)

---
*Last Updated: 2025-03-05*