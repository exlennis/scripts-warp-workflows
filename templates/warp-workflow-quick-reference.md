# Warp Workflow Quick Reference

This quick reference provides essential information for creating effective Warp workflows.

## Required Components

Every workflow must include:

- A structured `name` that follows the `[Action]_[Object]_[Context]` format — this improves searchability and organisation
- A concise `description` for human-readability that summarises what it does, when and how to use it
- A `command` to execute with structured comments at the start to help AI agents understand and work with workflows

## Recommended Components

For better usability, include:

- Relevant `tags` that accurately categorise the workflow (avoid repetitive tags to improve searchability)
- Carefully selected `arguments` with appropriate types best suited for each scenario
- Sensible `default value` for each argument to guide users and provide a starting point

## Naming Convention

Use the `[Action]_[Object]_[Context]` format for consistent organisation:
- **Action**: A verb describing what happens (Display, Find, Setup)
- **Object**: Target of the action (Files, Directory, Project)
- **Context**: Further qualification (BySize, WebDev, CloudDirectory)

Examples: `Display_Tree_CloudDirectory`, `Find_Files_BySize`, `Git_Squash_RecentCommits`

## Argument Types

Choose the appropriate argument type for each parameter using this decision tree:

```
START → Can the valid options be generated from the system?
├── YES → Use DYNAMIC ENUM
│         enum_command: "command to generate options"
│         Example: List directories, git branches, processes
│
├── NO → Are there a limited set of valid options?
│        ├── YES → Use STATIC ENUM
│        │         enum_variants: ["option1", "option2", "option3"]
│        │         Example: Format types (json/csv/text), modes (verbose/quiet)
│        │
│        └── NO → Use TEXT
│                 Example: Custom search terms, arbitrary values, one-off inputs
```

### Dynamic Enum Arguments
Use for filesystem selections or other dynamic options. These provide the best user experience when applicable.

```yaml
- name: target_directory
  description: "Directory to search"
  default_value: "."
  enum_command: >-
    find "$PWD" -type d -maxdepth 2 -not -path '*/\.*' | sort
```

### Static Enum Arguments
Use for selection from a fixed set of options.

```yaml
- name: log_format
  description: "Format for git log output"
  default_value: "oneline"
  enum_variants:
    - "oneline"
    - "short"
    - "medium"
    - "full"
```

### Text Arguments
Use for free-form input where enumeration isn't practical.

```yaml
- name: commit_message
  description: "Message for the commit"
  default_value: "Update documentation"
```

## Best Practices

1. **Verification**: Always check for required tools and valid directories
2. **Provide Feedback**: Use echo statements to communicate progress
3. **Error Handling**: Provide clear error messages and proper exit codes
4. **Sensible Defaults**: Provide sensible default values for all arguments
5. **Boolean Flags**: Use enum_variants with true/false for boolean options
6. **Test Thoroughly**: Test workflow with different arguments and edge cases

## Example Workflows

### Git Workflow

```yaml
name: "Git_Squash_RecentCommits"
description: "Squashes the last n commits together. Requires rewriting the commit message."
command: |
  # PURPOSE: Combine multiple recent commits into one
  # CONTEXT: When cleaning up git history before pushing
  # VERIFICATION: Confirms git repo exists
  
  if ! git rev-parse --is-inside-work-tree &> /dev/null; then
    echo "Error: Not in a git repository"
    exit 1
  fi
  
  git reset --soft HEAD~{{num_commits}} && git commit
arguments:
  - name: num_commits
    description: "Number of commits to squash"
    default_value: "2"
    enum_variants:
      - "2"
      - "3"
      - "5"
      - "10"
```

### Filesystem Workflow

```yaml
name: "Find_Files_BySize"
description: "Locates the largest files in a directory. Useful for disk cleanup."
command: |
  # PURPOSE: Find largest files in specified directory
  # CONTEXT: Use when cleaning up disk space
  # VERIFICATION: Checks directory existence
  
  if [ ! -d "{{target_directory}}" ]; then
    echo "Error: Directory not found"
    exit 1
  fi
  
  find "{{target_directory}}" -type f -size +{{min_size}} | xargs du -h | sort -rh | head -n {{file_count}}
arguments:
  - name: target_directory
    description: "Directory to search for large files"
    default_value: "."
    enum_command: >-
      find "$PWD" -type d -maxdepth 2 -not -path '*/\.*' | sort
  - name: file_count
    description: "Number of files to display"
    default_value: "10"
    enum_variants:
      - "5"
      - "10"
      - "20"
  - name: min_size
    description: "Minimum file size to include"
    default_value: "10M"
    enum_variants:
      - "1M"
      - "10M"
      - "100M"
      - "1G"
```
