# Dotfiles

Personal dotfiles managed with [chezmoi](https://www.chezmoi.io/).

## What's Included

- **ZSH configuration** (`.zshrc`) with Oh My Zsh
- **Powerlevel10k theme** configuration (`.p10k.zsh`)
- Cross-platform Homebrew setup (works on macOS and Linux)

## Prerequisites

- [Homebrew](https://brew.sh/) (macOS) or [Linuxbrew](https://docs.brew.sh/Homebrew-on-Linux) (Linux)
- Git

## Fresh Setup on a New Machine

### 1. Install chezmoi and apply dotfiles
```bash
brew install chezmoi
chezmoi init https://github.com/QingqiShi/dotfiles.git
chezmoi diff  # Preview changes
chezmoi apply
```

### 2. Install Oh My Zsh
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

When asked if you want to change your default shell, choose **No** if you're already using ZSH.

### 3. Install Powerlevel10k theme
```bash
git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k
```

### 4. Install required tools
```bash
brew install tmux fzf thefuck
```

### 5. Reload your shell
```bash
source ~/.zshrc
```

Or simply open a new terminal window.

## Updating Dotfiles

Chezmoi is configured to automatically commit and push changes.

### Edit a dotfile
```bash
chezmoi edit ~/.zshrc
```

### Apply changes from another dotfile
```bash
chezmoi add ~/.zshrc
```

This will automatically commit and push to the repository.

### Pull latest changes from repository
```bash
chezmoi update
```

## Manual Git Operations

If you need to manually work with the repository:
```bash
chezmoi cd        # Enter chezmoi source directory
git status        # Check status
git add .         # Stage changes
git commit -m ""  # Commit
git push          # Push to GitHub
exit              # Return to previous directory
```

## Troubleshooting

### Homebrew path errors

The `.zshrc` is configured to work across platforms. It checks for Homebrew in:
1. `/opt/homebrew/bin/brew` (Apple Silicon Mac)
2. `/usr/local/bin/brew` (Intel Mac)
3. `/home/linuxbrew/.linuxbrew/bin/brew` (Linux)

### Missing plugins errors

If you see plugin errors, ensure you've completed steps 2-4 in the setup.

### Theme not found

If Powerlevel10k theme isn't found, ensure you've run step 3 and that Oh My Zsh is installed.

## Tools Used

- **chezmoi**: Dotfile management
- **Oh My Zsh**: ZSH framework
- **Powerlevel10k**: ZSH theme
- **tmux**: Terminal multiplexer
- **fzf**: Fuzzy finder
- **thefuck**: Command correction tool
