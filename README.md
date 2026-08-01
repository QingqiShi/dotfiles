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

## Features

Quick reference for the day-to-day features installed by these dotfiles. Forgot what something does? Start here.

### Prompt — Powerlevel10k

A fast, information-rich Zsh prompt showing the current directory, git branch and dirty state, exit code and duration of the previous command, and Node version (in JS projects). Two comfort features worth knowing about:

- **Instant prompt** — the prompt renders from a cache before `.zshrc` finishes loading, so cold shells feel instantaneous.
- **Transient prompt** — previous prompts collapse to a single line in scrollback so history stays tidy.

Reconfigure with `p10k configure` (overwrites `~/.p10k.zsh` — remember to run `chezmoi re-add ~/.p10k.zsh` afterwards to save it back to the repo).

### Fuzzy finder — fzf

The single biggest productivity boost in the whole setup. Key bindings are wired up automatically via the OMZ `fzf` plugin — no extra config needed.

| Shortcut | What it does |
|---|---|
| **Ctrl-R** | Fuzzy-search shell history (the killer feature — never `history \| grep` again) |
| **Ctrl-T** | Fuzzy-find a file under the current directory and paste its path onto the command line |
| **Alt-C** | Fuzzy-`cd` into a subdirectory |
| **`**<Tab>`** | Fuzzy completion for any command: `vim **<tab>`, `cd **<tab>`, `kill **<tab>` (pid picker), `ssh **<tab>`, etc. |

### Git aliases — OMZ `git` plugin

The OMZ `git` plugin ships ~150 aliases. The ones worth memorizing:

| Alias | Expands to |
|---|---|
| `gst` | `git status` |
| `gco <branch>` | `git checkout <branch>` |
| `gcb <branch>` | `git checkout -b <branch>` (create + switch) |
| `gc` | `git commit -v` (opens editor) |
| `gcmsg "msg"` | `git commit -m "msg"` |
| `ga <file>` / `gaa` | `git add <file>` / `git add --all` |
| `gd` / `gds` | `git diff` / `git diff --staged` |
| `gp` / `gl` | `git push` / `git pull` |
| `glog` | `git log --oneline --decorate --graph` |
| `gb` / `gbd <name>` | `git branch` / `git branch -d <name>` |
| `grb` / `grbi` | `git rebase` / `git rebase -i` |
| `gsta` / `gstp` | `git stash` / `git stash pop` |

Full list: [OMZ git plugin reference](https://github.com/ohmyzsh/ohmyzsh/tree/master/plugins/git).

### Per-directory env vars — direnv

Drop a `.envrc` file in a project root, run `direnv allow` once, and its contents auto-load when you `cd` in and auto-unload when you `cd` out. Keeps secrets out of `.zshrc` and scopes env vars to the projects that need them.

```bash
# in some-project/.envrc
export OPENAI_API_KEY="sk-..."
export DATABASE_URL="postgres://localhost/myapp"
```

```bash
$ cd some-project
direnv: loading ~/some-project/.envrc
direnv: export +OPENAI_API_KEY +DATABASE_URL
```

### Node version management — nvm (lazy-loaded)

| Command | What it does |
|---|---|
| `nvm install 22` | Install Node 22 |
| `nvm use 22` | Switch the current shell to Node 22 |
| `nvm alias default 22` | Default Node version for new shells |
| `nvm ls` | List installed versions |
| `nvm current` | Show active version |

**Lazy-loading note:** `nvm` is not sourced when your shell starts — it's stubbed for `nvm`, `node`, `npm`, `npx`, `pnpm`, `yarn`, and `corepack`, and only actually loads the first time you call one of those. Expect a ~200-500ms one-time pause on first use in a new shell, in exchange for dramatically faster shell startup.

**Auto-switching:** `cd` into a directory containing an `.nvmrc` and the shell switches to that Node version automatically, reverting to your default on the way out. Projects pinned to an older Node "just work" without a manual `nvm use`; install the version once with `nvm install` if it's missing.

### Command correction — thefuck

Mistyped the last command? Type `fuck` and hit Enter. It reruns the previous command with the most likely fix, asking for confirmation first.

```bash
$ gti status
zsh: command not found: gti
$ fuck
git status [enter/↑/↓/ctrl+c]
```

### Terminal multiplexing — tmux

The OMZ `tmux` plugin adds convenience aliases:

| Alias | Expands to |
|---|---|
| `ta` | `tmux attach` (attach to the last session) |
| `ts <name>` | `tmux new-session -s <name>` |
| `tl` | `tmux list-sessions` |
| `tkss <name>` | Kill a specific session |

Once inside tmux, the prefix key is `Ctrl-b`:

| Keys | Action |
|---|---|
| `Ctrl-b c` | New window |
| `Ctrl-b n` / `p` | Next / previous window |
| `Ctrl-b %` / `"` | Split pane vertically / horizontally |
| `Ctrl-b <arrow>` | Move focus between panes |
| `Ctrl-b d` | Detach (session keeps running in the background) |
| `Ctrl-b [` | Enter copy / scroll mode (`q` to exit) |

### Ghostty

- **`Ctrl+\`` (global)** — toggle the drop-down quick terminal from anywhere in macOS. Requires Accessibility permission — see [Troubleshooting](#troubleshooting).
- **`Shift+Enter`** — in TUI apps like Claude Code, inserts a literal newline without submitting the current input.
- **Appearance** — Aura theme, 80% opacity with background blur.

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

