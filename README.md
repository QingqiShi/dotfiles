# Dotfiles

Personal dotfiles managed with [chezmoi](https://www.chezmoi.io/).

## What's Included

- **ZSH configuration** (`.zshrc`) with Oh My Zsh
- **Powerlevel10k theme** configuration (`.p10k.zsh`)
- **Ghostty terminal** configuration (`.config/ghostty/config.ghostty`)
- Cross-platform Homebrew setup (works on macOS and Linux)

<img width="1045" height="627" alt="截屏2026-04-11 16 08 04" src="https://github.com/user-attachments/assets/9e955dc8-ffb6-49eb-8f4c-84b10e207fae" />

## Prerequisites

- [Homebrew](https://brew.sh/) (macOS) or [Linuxbrew](https://docs.brew.sh/Homebrew-on-Linux) (Linux)
- Git
- A terminal with a Nerd Font (see below)

### Nerd Font

Powerlevel10k uses Nerd Font glyphs for its icons. Two easy options:

- **Use [Ghostty](https://ghostty.org/)** — it ships with JetBrains Mono Nerd Font bundled as the default font, so icons work with zero configuration. This repo also tracks a Ghostty config (theme, opacity, blur, and a global quick-terminal hotkey) that `chezmoi apply` installs automatically.
- **Install a Nerd Font manually** and set it as your terminal font:
  ```bash
  brew install --cask font-meslo-lg-nerd-font
  ```

## Fresh Setup on a New Machine

> **Important:** The Oh My Zsh installer overwrites `~/.zshrc`, so install Oh My Zsh and Powerlevel10k **first**, then apply chezmoi last.

### 1. Install required tools
```bash
brew install chezmoi tmux fzf thefuck direnv nvm
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

### 4. Apply dotfiles with chezmoi
```bash
chezmoi init https://github.com/QingqiShi/dotfiles.git
chezmoi diff   # Preview changes
chezmoi apply
```

This overwrites the default `~/.zshrc` that Oh My Zsh installed with the one from this repo, along with `~/.p10k.zsh`.

### 5. Reload your shell
```bash
exec zsh
```

Or simply open a new terminal window.

## Updating Dotfiles

Chezmoi is configured to automatically commit and push changes.

### Edit a dotfile
```bash
chezmoi edit ~/.zshrc
```

### Capture local changes back into the repo
```bash
chezmoi re-add ~/.zshrc
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

If you see plugin errors on shell startup, ensure you've completed step 1 — it installs `direnv`, `nvm`, `fzf`, `thefuck`, and `tmux`, which back the Oh My Zsh plugins enabled in `.zshrc`.

### Theme not found

If Powerlevel10k theme isn't found, ensure you've run step 3 and that Oh My Zsh is installed.

### Icons show as boxes (□)

Your terminal font isn't a Nerd Font. See the [Nerd Font](#nerd-font) section above.

### Ghostty's global Ctrl+\` hotkey does nothing

The Ghostty config registers a global (system-wide) hotkey to toggle its quick terminal. On macOS, Ghostty needs **Accessibility** permission to capture global hotkeys. On first launch after applying dotfiles, macOS should prompt you — grant permission in **System Settings → Privacy & Security → Accessibility**. If it doesn't prompt, toggle Ghostty's entry off and on again there.

## Tools Used

- **chezmoi**: Dotfile management
- **Oh My Zsh**: ZSH framework
- **Powerlevel10k**: ZSH theme
- **tmux**: Terminal multiplexer
- **fzf**: Fuzzy finder
- **thefuck**: Command correction tool
- **direnv**: Per-directory environment variables
- **nvm**: Node Version Manager
- **Ghostty** _(optional)_: Terminal emulator with bundled Nerd Font
