# Lemur 🐒

A smart Git branch switcher that learns from your usage patterns. Like autojump for directories, but for Git branches.

## What is Lemur?

Lemur is a lightweight command-line tool that helps you quickly switch between Git branches based on your usage frequency and patterns. Instead of typing out full branch names or scrolling through `git branch` output, Lemur learns which branches you use most and lets you jump to them with just a few characters.

## Features

- **Smart branch switching**: Jump to branches using partial names or fuzzy matching
- **Usage-based ranking**: Frequently used branches appear first in suggestions
- **Lightweight**: Pure Bash implementation for maximum portability
- **Easy integration**: Works with your existing Git workflow
- **Cross-platform**: Works on macOS, Linux, and other Unix-like systems

## Installation

```bash
# Download and install
curl -o ~/.local/bin/lemur https://raw.githubusercontent.com/skyronic/git-lemur/main/lemur
chmod +x ~/.local/bin/lemur

# Add to PATH if needed
echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc
source ~/.bashrc
```

### Setup in Git Repositories

After installation, initialize Lemur in each Git repository where you want to use it:

```bash
cd /path/to/your/git/repo
lemur init
```

The `lemur init` command will:
- Create the `.git/lemur/` directory for tracking data
- Set up the post-checkout Git hook to track branch switches
- Handle existing hooks by backing them up and appending Lemur tracking
- Start tracking immediately by logging your current branch

**Note**: You only need to run `lemur init` once per repository. The command is safe to run multiple times.

## Usage

### Basic Commands

```bash
# Initialize Lemur in a Git repository
lemur init
# → sets up branch tracking for the current repository

# Switch to a branch (fuzzy matching)
lemur feature
# → switches to "feature/user-authentication" if it's your most used match

# Show available branches (no arguments)
lemur
# → shows your most used branches
```

### Examples

```bash
# You frequently work on these branches:
# - feature/user-authentication
# - feature/user-profile  
# - bugfix/auth-validation
# - main

# Quick switching:
lemur feat        # → feature/user-authentication (most used)
lemur user        # → feature/user-profile
lemur bug         # → bugfix/auth-validation
lemur main        # → main

# Multiple matches - automatically picks most used:
lemur auth        # → feature/user-authentication (most used match)

# See all matches for a pattern:
lemur --list auth
# Branches matching 'auth':
#   1) feature/user-authentication (★★★)
#   2) bugfix/auth-validation (★★)
```

## How It Works

1. **Setup**: Run `lemur init` to set up branch tracking in a Git repository
2. **Tracking**: Git post-checkout hook logs branch switches to `.git/lemur/history.db`
3. **Matching**: When you type a partial name, Lemur finds the best matches from your history
4. **Switching**: Automatically runs `git checkout` to switch branches

## Data Storage

Lemur stores branch usage data locally in each Git repository at `.git/lemur/history.db`.

## Advanced Usage

### Useful Aliases

```bash
# Interactive branch selection with fzf
alias lb='lemur | fzf | xargs lemur'

# Quick patterns
alias gf='lemur feature'
alias gb='lemur bugfix'
alias gm='lemur main'
```

### Scripting

```bash
# Get the most likely branch for a pattern
BRANCH=$(lemur --dry-run feature)
if [ $? -eq 0 ]; then
    echo "Would switch to: $BRANCH"
fi
```

## Commands Reference

| Command | Description |
|---------|-------------|
| `lemur init` | Initialize Lemur tracking in current repository |
| `lemur` | Show most used branches |
| `lemur <pattern>` | Switch to best matching branch |
| `lemur --help` | Show help message |
| `lemur --version` | Show version |
| `lemur --dry-run <pattern>` | Show which branch would be selected without switching |
| `lemur --list <pattern>` | Show all branches matching a pattern without switching |

## Requirements

- Bash 4.0+
- Git 2.0+
- Standard Unix utilities (awk, sed, sort, etc.)

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

## License

MIT License - see LICENSE file for details.

## Similar Tools

- [autojump](https://github.com/wting/autojump) - Smart directory jumping
- [z](https://github.com/rupa/z) - Jump around directories
- [git-recent](https://github.com/paulirish/git-recent) - See recent branches

## Changelog

### v0.1.0
- Initial release
- Basic branch switching and learning
- `lemur init` command for easy repository setup
- Smart Git hook integration with existing hook backup
- Usage-based branch ranking with recency weighting
- Interactive branch selection for multiple matches
- Colored output and star rating system
- Dry-run mode for testing
- Cross-platform support
