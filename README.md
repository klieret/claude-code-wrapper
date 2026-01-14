# claude-code-wrapper

```bash
# Prompt for task with multiline input support
_prompt_for_task() {
    # Color codes: #a45138 (Anthropic orange) in RGB
    local orange_on="\033[38;2;164;81;56m"
    local dim_white=$(tput dim)
    local color_off=$(tput sgr0)

    echo "" >&2
    echo -e "${orange_on} ▐▛███▜▌   Claude Code (fast entry)${color_off}" >&2
    echo -e "${orange_on}▝▜█████▛▘${color_off}" >&2
    echo -e "${orange_on}  ▘▘ ▝▝    ${color_off}$(pwd)" >&2
    echo "" >&2
    echo -e "${dim_white}Enter your task below (supports multiple lines)${color_off}" >&2
    echo -e "${dim_white}Press Ctrl-D on a new line when done, or Ctrl-C to cancel${color_off}" >&2
    echo "" >&2
    echo -ne "${orange_on}❯${color_off} " >&2

    # Read multiline input from terminal
    local task=$(cat </dev/tty)
    local result=$?

    # Print completion message
    if [ $result -eq 0 ] && [ -n "$task" ]; then
        echo "" >&2
        echo -e "${orange_on}✓ Task received! Launching Claude...${color_off}" >&2
        echo "" >&2
        echo "$task"
        return 0
    else
        echo "" >&2
        echo -e "${orange_on}✗ Cancelled or no input provided${color_off}" >&2
        echo "" >&2
        return 1
    fi
}

# cc: Claude with fast proimpt
function cc() {
    local task
    task=$(_prompt_for_task) || return 1
    if [ -n "$task" ]; then
        claude "$task" "$@"
    fi
}
```
