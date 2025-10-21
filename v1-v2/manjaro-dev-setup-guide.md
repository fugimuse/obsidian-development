# Manjaro GNOME Development Machine Setup Guide

## Prerequisites

- Mac Mini 2012 with SSD
- Pre-created partition for Linux installation
- Manjaro GNOME ISO downloaded
- USB drive for installation media

-----

## Phase 1: Base Installation

### 1.1 Create Installation Media

```bash
# On macOS (if needed)
sudo dd if=manjaro-gnome-*.iso of=/dev/diskX bs=1m status=progress
```

### 1.2 Install Manjaro GNOME with BTRFS

1. Boot from USB (hold Option key during startup)
2. Select “Boot with open source drivers”
3. Launch Calamares installer
4. Choose “Replace a partition” and select your pre-created partition
5. **Configure filesystem:**

- Select BTRFS as filesystem type (if available in dropdown)
- Calamares will handle BTRFS setup automatically with sensible defaults

1. Configure user account and timezone
2. Complete installation and reboot

### 1.3 Initial Settings

- System Settings | Displays | Resolution | 2048 x 1152 (16:9) | Apply | Keep Changes
- Tweaks | Continue | Keyboard | Additional Layout Options
  - Caps Lock behavior | Swap Esc and Caps Lock
  - Carl position | Left Alt as Ctrl, Left Ctrl as Win, Left Win as Left Alt
- Extensions
  - Turn on ArcMenu
  - Turn off Dash to Dock
  - Turn on Dash to Panel
  - Turn on System Monitor
  - Turn on Workspace Indicator
- Terminal | Preferences
  - Turn on Unlimited Scrollback

### 1.4 Configure Fastest Mirrors

```bash
# Update mirror list to fastest mirrors
sudo pacman-mirrors --fasttrack 5 && sudo pacman -Sy

# Alternative: Auto-select fastest mirrors by location
sudo pacman-mirrors --country United_States,Canada --api --set-branch stable --protocol https

# For other countries, list available options:
# sudo pacman-mirrors --country-list

# Refresh package database and update system
sudo pacman -Syyu
```

### 1.5 Create Swap File

```bash
## NOTE: Skip this section for now—don't really need a swap file as I have plenty of RAM.

# Create empty swap file
sudo touch /swapfile

# Disable COW for swap file (BTRFS requirement - must be done before writing data)
sudo chattr +C /swapfile

# Create 4GB swap file using dd (required for BTRFS)
sudo dd if=/dev/zero of=/swapfile bs=1M count=4096 status=progress

# Set proper permissions
sudo chmod 600 /swapfile

# Setup swap
sudo mkswap /swapfile
sudo swapon /swapfile

# Add to fstab for permanent mounting
echo '/swapfile none swap defaults 0 0' | sudo tee -a /etc/fstab

# Verify swap is active
sudo swapon --show
```

### 1.6 Install Nerd Fonts

```bash
# Create fonts directory
mkdir -p ~/.local/share/fonts

# Download and install Hack Nerd Font
wget https://github.com/ryanoasis/nerd-fonts/releases/download/v3.0.2/Hack.zip
unzip Hack.zip -d ~/.local/share/fonts/Hack/

# Download and install Fira Code Nerd Font
wget https://github.com/ryanoasis/nerd-fonts/releases/download/v3.0.2/FiraCode.zip
unzip FiraCode.zip -d ~/.local/share/fonts/FiraCode/

# Refresh font cache
fc-cache -fv

# Set monospace font for terminal
gsettings set org.gnome.desktop.interface monospace-font-name 'Hack Nerd Font 12'
```

### 1.7 Initial System Update and Essential Tools

```bash
sudo pacman -Syu

# Skip tree for now, as I want to use `lsd` instead of `ls` and `tree`.
sudo pacman -S base-devel git lazygit curl wget btrfs-progs ripgrep fd make unzip gcc xclip neovim helix obsidian bat lsd httpie alacritty
```

### 1.8 Minimal Alacritty Configuration

```bash
# Create Alacritty config directory
mkdir -p ~/.config/alacritty

# Create minimal Alacritty configuration
cat > ~/.config/alacritty/alacritty.toml << 'EOF'
# Minimal Alacritty configuration
# Full Dracula theming will be applied in Phase 5

[window]
opacity = 1.00

[window.padding]
x = 10
y = 10

[font]
size = 12.0

[font.normal]
family = "Hack Nerd Font"
style = "Regular"

# Basic color scheme (will be replaced with Dracula theme in Phase 5)
[colors.primary]
background = "#1e1e1e"
foreground = "#d4d4d4"
EOF

# Set Alacritty as default terminal
gsettings set org.gnome.desktop.default-applications.terminal exec 'alacritty'

echo "Alacritty configured with minimal settings. Full theming in Phase 5."
```

### 1.9 Configure pamac on Manjaro

#### General configuration updates

```bash
## Backup pamac.conf.
sudo cp /etc/pamac.conf /etc/pamac.conf.orig

## When removing a package, also remove those dependencies
## that are not required by other packages (recurse option):
sudo sed -i '/^#RemoveUnrequiredDeps/s/^#//g' /etc/pamac.conf

## When no update is available, hide the tray icon:
#sudo sed -i '/^#NoUpdateHideIcon/s/^#//g' /etc/pamac.conf

## When applying updates, enable packages downgrade:
#sudo sed -i '/^#EnableDowngrade/s/^#//g' /etc/pamac.conf

## When installing packages, do not check for updates:
#sudo sed -i '/^#SimpleInstall/s/^#//g' /etc/pamac.conf
```

#### AUR in pamac configuration

```bash
## Allow Pamac to search and install packages from AUR:
sudo sed -i '/^#EnableAUR/s/^#//g' /etc/pamac.conf

## Keep built packages from AUR in cache after installation:
sudo sed -i '/^#KeepBuiltPkgs/s/^#//g' /etc/pamac.conf

## When AUR support is enabled check for updates from AUR:
sudo sed -i '/^#CheckAURUpdates/s/^#//g' /etc/pamac.conf

## When check updates from AUR support is enabled check for vcs updates:
#sudo sed -i '/^#CheckAURVCSUpdates/s/^#//g' /etc/pamac.conf

#BuildDirectory
#KeepNumPackages
#OnlyRmUninstalled

## Download updates in background:
#sudo sed -i '/^#DownloadUpdates/s/^#//g' /etc/pamac.conf

## Offline upgrade:
sudo sed -i '/^#OfflineUpgrade/s/^#//g' /etc/pamac.conf

#MaxParallelDownloads
```

#### Flatpak and Snap in pamac

```bash
## Flatpak
sudo sed -i '/^#EnableFlatpak/s/^#//g' /etc/pamac.conf

sudo sed -i '/^#CheckFlatpakUpdates/s/^#//g' /etc/pamac.conf

## Snap
#sudo sed -i '/^#EnableSnap/s/^#//g' /etc/pamac.conf
```

#### Reload shell

```bash
exec $SHELL
```

#### Enable AppImage desktop integration

**NOTE: Skip this section for now - AppImage support will be added later if needed**

### 1.10 Install Basic Tools with pamac

```bash
pamac search 1password
pamac install 1password

#TODO: Remove, since installed above.
#pamac search obsidian
#pamac install obsidian
```

### 1.11 Verify BTRFS Setup

```bash
# Check filesystem type and subvolumes
df -T /
sudo btrfs subvolume list /

# Check BTRFS filesystem info
sudo btrfs filesystem show
sudo btrfs filesystem usage /
```

-----

## Phase 2: TimeMachine-Style Backup System

### 2.1 Configure Timeshift (System and User Files)

1. **Open Timeshift** from the Applications menu (search for “Timeshift”)
2. **Enter your password** when prompted (requires admin privileges)
3. **Follow the setup wizard:**

- Select “BTRFS” as snapshot type (should auto-detect)
- Select your root partition (should auto-detect)
- Configure schedule:
  - Boot: 3 snapshots
  - Daily: 5 snapshots
  - Weekly: 3 snapshots
  - Monthly: 2 snapshots
- **Include user files**: In settings, enable “Include Home directories” to backup your personal files too
- Click “Finish”

Timeshift will automatically:

- Create snapshots before package updates
- Include both system and user files in snapshots
- Run scheduled snapshots
- Clean up old snapshots automatically
- Work seamlessly with your BTRFS filesystem

### 2.2 Optional: External Backup Drive

```bash
# If you want additional backup security, you can also 
# configure an external drive for Timeshift

# Connect your external drive
# In Timeshift GUI: Settings > Backup Device > Select your external drive
# This creates additional copies of your snapshots on external storage
```

### 2.3 Test Your Backup Setup

```bash
# Create a test snapshot in Timeshift
sudo timeshift --create --comments "Initial setup test"

# Verify snapshots were created
sudo timeshift --list
```

You can also test through the GUI:

1. **Open Timeshift** from Applications menu
2. **Click “Create”** to make a manual snapshot
3. **Browse snapshots** in the main window

### 2.4 Enable Automatic Backups

Timeshift automatic snapshots are already enabled from the setup wizard. You can verify this by:

1. **Opening Timeshift** from Applications menu
2. **Checking the schedule** in Settings if needed
3. **Viewing the timeline** in the main window

-----

## Phase 3: Setup SSH

### 3.1 Generate SSH Key

```bash
# Generate key for GitHub fugimuse
ssh-keygen -t ed25519 -f ~/.ssh/mefm_ed25519 -C "fugimuse@gmail.com"
```

### 3.2 Print public key to copy

```bash
cat ~/.ssh/mefm_ed25519.pub
```

### 3.3 Copy public key to GitHub, GitLab, and BitBucket

Copy the public key to GitHub, GitLab, BitBucket and any other repos where you use the fugimuse login.

### 3.4 Create config file to allow named URLs

```bash
cat >> ~/.ssh/config << "EOF"
# Personal GitHub account
Host mefm.github.com
    HostName github.com
    User fugimuse
    IdentityFile ~/.ssh/mefm_ed25519
    IdentitiesOnly yes
EOF
```

-----

## Phase 4: Setup base configuration

### 4.1 Clone tool repos

```bash
# Bash
git clone git@mefm.github.com:fmconfig/bash.git ~/.config/bash

# dotfiles
git clone git@mefm.github.com:fmconfig/dotfiles.git ~/.config/dotfiles

# dph
git clone git@mefm.github.com:fmconfig/dph.git ~/.config/dph

# NeoVim
git clone git@mefm.github.com:fmconfig/nvim.git ~/.config/nvim

# Zsh
git clone git@mefm.github.com:fmconfig/zsh.git ~/.config/zsh
```

### 4.2 Setup Home Directory Links

```bash
pushd ~/.config/dotfiles
    ./remove_links
    ./setup_links
popd
```

### 4.3 Restart Shell

```bash
exec $SHELL
```

### 4.4 Install Oh-My-Zsh!

```bash
# Install Oh My Zsh!
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Restart Zsh
exec $SHELL
```

-----

## Phase 5: Terminal Setup and Theming

### 5.1 Install Terminal Applications

```bash
# Install Alacritty themes (Alacritty itself was installed in Phase 1)
# NOTE: Do not install these - we're setting the theme manually later.
#pamac install alacritty-theme-switcher
#pamac install alacritty-themes

# Install tmux
sudo pacman -S tmux powerline
pamac install tmux-plugin-manager

# Install Terminator
sudo pacman -S terminator
```

### 5.2 Configure Neovim with Dracula Theme

```bash
# Neovim config was cloned in Phase 4.1
# If you need to set up a basic Dracula theme manually:

# Create Neovim config structure (skip if already exists from Phase 4)
mkdir -p ~/.config/nvim/lua/config
mkdir -p ~/.config/nvim/lua/plugins

# Create init.lua (skip if already exists from Phase 4)
cat > ~/.config/nvim/init.lua << 'EOF'
-- Bootstrap lazy.nvim plugin manager
local lazypath = vim.fn.stdpath("data") .. "/lazy/lazy.nvim"
if not vim.loop.fs_stat(lazypath) then
  vim.fn.system({
    "git",
    "clone",
    "--filter=blob:none",
    "https://github.com/folke/lazy.nvim.git",
    "--branch=stable",
    lazypath,
  })
end
vim.opt.rtp:prepend(lazypath)

-- Load configuration modules
require("config.options")
require("config.keymaps")
require("config.autocmds")

-- Load plugins
require("lazy").setup("plugins", {
  change_detection = { notify = false },
})
EOF

# Create basic options configuration
cat > ~/.config/nvim/lua/config/options.lua << 'EOF'
-- Basic options
vim.opt.number = true
vim.opt.relativenumber = true
vim.opt.tabstop = 2
vim.opt.shiftwidth = 2
vim.opt.expandtab = true
vim.opt.smartindent = true
vim.opt.wrap = false
vim.opt.ignorecase = true
vim.opt.smartcase = true
vim.opt.termguicolors = true

-- Font configuration
vim.opt.guifont = "Hack Nerd Font:h12"
EOF

# Create basic keymaps
cat > ~/.config/nvim/lua/config/keymaps.lua << 'EOF'
-- Set leader key
vim.g.mapleader = " "

-- Basic keymaps
vim.keymap.set("n", "<leader>w", ":w<CR>")
vim.keymap.set("n", "<leader>q", ":q<CR>")
EOF

# Create autocmds file
touch ~/.config/nvim/lua/config/autocmds.lua

# Create basic plugin configuration with Dracula theme
cat > ~/.config/nvim/lua/plugins/init.lua << 'EOF'
return {
  -- Dracula theme
  {
    "Mofiqul/dracula.nvim",
    priority = 1000,
    config = function()
      vim.cmd("colorscheme dracula")
    end,
  },
  
  -- Language servers and completion
  {
    "neovim/nvim-lspconfig",
    dependencies = {
      "hrsh7th/nvim-cmp",
      "hrsh7th/cmp-nvim-lsp",
    },
  },
  
  -- File explorer
  {
    "nvim-tree/nvim-tree.lua",
    dependencies = "nvim-tree/nvim-web-devicons",
    config = function()
      require("nvim-tree").setup()
    end,
  },
}
EOF

# Launch nvim once to install plugins
nvim +qall
```

### 5.3 Configure Helix with Dracula Theme

```bash
# Create Helix config directory
mkdir -p ~/.config/helix

# Create Helix configuration with Dracula theme
cat > ~/.config/helix/config.toml << 'EOF'
theme = "dracula"

[editor]
line-number = "relative"
mouse = true
cursorline = true
auto-format = true
idle-timeout = 250
completion-trigger-len = 1
auto-save = false
bufferline = "always"

[editor.cursor-shape]
insert = "bar"
normal = "block"
select = "underline"

[editor.file-picker]
hidden = false

[editor.statusline]
left = ["mode", "spinner", "file-name", "read-only-indicator", "file-modification-indicator"]
center = []
right = ["diagnostics", "selections", "register", "position", "file-encoding"]

[editor.lsp]
display-messages = true
display-inlay-hints = true

[editor.indent-guides]
render = true
character = "│"

[keys.normal]
# Quick save
C-s = ":w"
# Quick quit
C-q = ":q"

[keys.insert]
# Quick save in insert mode
C-s = [":w", "normal_mode"]
EOF

# Install Dracula theme (should be available by default, but verify)
echo "Helix configured with Dracula theme and LSP support."
echo "Language servers will be automatically detected for installed languages."
echo "Launch helix with: hx <filename>"
```

### 5.4 Apply Dracula Theme to Alacritty

```bash
# Alacritty was installed in Phase 1 with minimal configuration
# Now we'll apply the full Dracula theme

# Update Alacritty configuration with Dracula theme
cat > ~/.config/alacritty/alacritty.toml << 'EOF'
# Alacritty Configuration with Dracula Theme

# Import Dracula theme (if using alacritty-themes package)
# Uncomment the line below if you have alacritty-themes installed
# import = ["~/.config/alacritty/themes/themes/dracula.toml"]

[window]
opacity = 0.95

[window.padding]
x = 10
y = 10

[font]
size = 12.0

[font.normal]
family = "Hack Nerd Font"
style = "Regular"

# Dracula Color Scheme
[colors.primary]
background = "#282a36"
foreground = "#f8f8f2"

[colors.cursor]
text = "#282a36"
cursor = "#f8f8f2"

[colors.selection]
text = "#f8f8f2"
background = "#44475a"

[colors.normal]
black = "#000000"
red = "#ff5555"
green = "#50fa7b"
yellow = "#f1fa8c"
blue = "#bd93f9"
magenta = "#ff79c6"
cyan = "#8be9fd"
white = "#bbbbbb"

[colors.bright]
black = "#555555"
red = "#ff5555"
green = "#50fa7b"
yellow = "#f1fa8c"
blue = "#bd93f9"
magenta = "#ff79c6"
cyan = "#8be9fd"
white = "#ffffff"
EOF

echo "Dracula theme applied to Alacritty. Restart Alacritty to see changes."
```

### 5.5 Configure tmux with Dracula Theme

```bash
# Create tmux config directory
mkdir -p ~/.config/tmux

# Create tmux configuration with Dracula theme
cat > ~/.config/tmux/tmux.conf << 'EOF'
# Set prefix key
set -g prefix C-a
unbind C-b

# Enable mouse mode
set -g mouse on

# Set default terminal
set -g default-terminal "screen-256color"

# Dracula Theme Configuration
# Available plugins: battery, cpu-usage, git, gpu-usage, ram-usage, network, network-bandwidth, network-ping, weather, time
set -g @dracula-show-powerline true
set -g @dracula-show-flags true
set -g @dracula-show-left-icon session
set -g @dracula-plugins "cpu-usage ram-usage"

# Initialize TPM (Tmux Plugin Manager)
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'tmux-plugins/tmux-sensible'
set -g @plugin 'dracula/tmux'

# Reload config
bind r source-file ~/.config/tmux/tmux.conf \; display-message "Config reloaded!"

# Initialize TPM (keep this line at the very bottom of tmux.conf)
run '~/.config/tmux/plugins/tpm/tpm'
EOF

# Install TPM and plugins
git clone https://github.com/tmux-plugins/tpm ~/.config/tmux/plugins/tpm

# Install plugins (press prefix + I inside tmux to install, or run this command)
~/.config/tmux/plugins/tpm/bin/install_plugins
```

### 5.6 Configure Terminator with Dracula Theme

```bash
# Create Terminator config directory
mkdir -p ~/.config/terminator

# Create Terminator configuration with Dracula theme
cat > ~/.config/terminator/config << 'EOF'
[global_config]
  title_hide_sizetext = True
  title_transmit_bg_color = "#bd93f9"

[keybindings]

[profiles]
  [[default]]
    background_color = "#282a36"
    background_darkness = 0.95
    background_type = transparent
    cursor_color = "#f8f8f2"
    font = Hack Nerd Font 12
    foreground_color = "#f8f8f2"
    palette = "#000000:#ff5555:#50fa7b:#f1fa8c:#bd93f9:#ff79c6:#8be9fd:#bbbbbb:#555555:#ff5555:#50fa7b:#f1fa8c:#bd93f9:#ff79c6:#8be9fd:#ffffff"
    use_system_font = False
    scrollback_infinite = True

[layouts]
  [[default]]
    [[[window0]]]
      type = Window
      parent = ""
    [[[child1]]]
      type = Terminal
      parent = window0

[plugins]
EOF
```

### 5.7 Configure Starship Prompt

```bash
# Install Starship
pamac install starship

# ############################################
# NOTE: Starship config is not neccessary here
#       as I have copied this and two other 
#       configs into my personal config repos,
#       from which I symlink my preferred 
#       config to ~/.config/starship.toml.
# ############################################

# Create Dracula-themed Starship configuration
mkdir -p ~/.config

cat > ~/.config/starship.toml << 'EOF'
# Starship Configuration with Dracula Colors
"$schema" = 'https://starship.rs/config-schema.json'

# Inserts a blank line between shell prompts
add_newline = true

# Format of the prompt
format = """
[┌───────────────────────────────────────────────────>](bold purple)
[│](bold purple)$directory$git_branch$git_status$git_state
[│](bold purple)$python$nodejs$deno$bun$golang$rust$haskell$elixir$erlang$ruby$perl$java$kotlin$scala
[└─>](bold purple) """

# Timeout for commands (in milliseconds)
command_timeout = 500

# Right prompt (optional)
right_format = """$cmd_duration"""

[character]
success_symbol = "[➜](bold green)"
error_symbol = "[➜](bold red)"

[directory]
style = "bold cyan"
truncation_length = 3
truncate_to_repo = true
format = "[$path]($style)[$read_only]($read_only_style) "

[git_branch]
symbol = " "
style = "bold purple"
format = "on [$symbol$branch]($style) "

[git_status]
style = "bold yellow"
format = '([$all_status$ahead_behind]($style) )'
conflicted = "🏳"
ahead = "⇡${count}"
behind = "⇣${count}"
diverged = "⇕⇡${ahead_count}⇣${behind_count}"
up_to_date = "✓"
untracked = "?"
stashed = "$"
modified = "!"
staged = '[++\($count\)](green)'
renamed = "»"
deleted = "✘"

[git_state]
format = '[\($state( $progress_current of $progress_total)\)]($style) '
cherry_pick = "[🍒 PICKING](bold red)"

[cmd_duration]
min_time = 500
format = "took [$duration](bold yellow)"

# Language-specific modules
[python]
symbol = " "
style = "bold yellow"
format = 'via [$symbol$pyenv_prefix($version )(\($virtualenv\) )]($style)'

[nodejs]
symbol = " "
style = "bold green"
format = "via [$symbol($version )]($style)"

[deno]
symbol = "🦕 "
style = "bold cyan"
format = "via [$symbol($version )]($style)"

[bun]
symbol = "🍞 "
style = "bold red"
format = "via [$symbol($version )]($style)"

[golang]
symbol = " "
style = "bold cyan"
format = "via [$symbol($version )]($style)"

[rust]
symbol = " "
style = "bold red"
format = "via [$symbol($version )]($style)"

[haskell]
symbol = " "
style = "bold purple"
format = "via [$symbol($version )]($style)"

[elixir]
symbol = " "
style = "bold purple"
format = 'via [$symbol($version \(OTP $otp_version\) )]($style)'

[erlang]
symbol = " "
style = "bold red"
format = "via [$symbol($version )]($style)"

[ruby]
symbol = " "
style = "bold red"
format = "via [$symbol($version )]($style)"

[perl]
symbol = "🐪 "
style = "bold cyan"
format = "via [$symbol($version )]($style)"

[java]
symbol = " "
style = "bold red"
format = "via [$symbol($version )]($style)"

[kotlin]
symbol = " "
style = "bold purple"
format = "via [$symbol($version )]($style)"

[scala]
symbol = " "
style = "bold red"
format = "via [$symbol($version )]($style)"

# Package version
[package]
symbol = "📦 "
style = "bold cyan"
format = "via [$symbol$version]($style) "

# Container
[docker_context]
symbol = " "
style = "bold blue"
format = "via [$symbol$context]($style) "

# Memory usage
[memory_usage]
disabled = false
threshold = 75
symbol = " "
style = "bold dimmed red"
format = "via $symbol[$ram( | $swap)]($style) "

# Time
[time]
disabled = false
format = '🕙[\[ $time \]]($style) '
time_format = "%T"
style = "bold cyan"

# Username
[username]
style_user = "bold cyan"
style_root = "bold red"
format = "[$user]($style) "
disabled = false
show_always = false

# Hostname
[hostname]
ssh_only = true
format = "on [$hostname](bold red) "
disabled = false

# Status
[status]
style = "bold red"
symbol = "✖"
format = '[\[$symbol $common_meaning$signal_name$maybe_int\]]($style) '
map_symbol = true
disabled = false
EOF

# NOTE: Add 'eval "$(starship init zsh)"' to the end of your .zshrc
echo "Starship configuration created at ~/.config/starship.toml"
echo "Add this line to your ~/.zshrc: eval \"\$(starship init zsh)\""
echo "Then run 'exec \$SHELL' to activate"
```

### 5.8 Theme Bat

```bash
# Install Dracula theme for bat
mkdir -p "$(bat --config-dir)/themes"
wget -P "$(bat --config-dir)/themes" https://github.com/dracula/sublime/raw/master/Dracula.tmTheme

# Rebuild cache
bat cache --build

# Set as default in bat config
mkdir -p ~/.config/bat
echo '--theme="Dracula"' > ~/.config/bat/config

# Test bat with Dracula theme
bat ~/.zshrc
```

### 5.9 Theme Lazygit

```bash
# Create lazygit config directory
mkdir -p ~/.config/lazygit

# Create config with Dracula colors
cat > ~/.config/lazygit/config.yml << 'EOF'
gui:
  theme:
    activeBorderColor:
      - '#ff79c6'
      - bold
    inactiveBorderColor:
      - '#6272a4'
    searchingActiveBorderColor:
      - '#ffb86c'
      - bold
    optionsTextColor:
      - '#8be9fd'
    selectedLineBgColor:
      - '#44475a'
    selectedRangeBgColor:
      - '#44475a'
    cherryPickedCommitBgColor:
      - '#bd93f9'
    cherryPickedCommitFgColor:
      - '#8be9fd'
    unstagedChangesColor:
      - '#ff5555'
EOF

# Test lazygit theme
lazygit
```

### 5.10 Theme lsd (Modern ls with Dracula colors)

```bash
# Create lsd config directory
mkdir -p ~/.config/lsd

# Create Dracula color configuration for lsd
cat > ~/.config/lsd/colors.yaml << 'EOF'
user: 141        # Dracula cyan
group: 141       # Dracula cyan
permission:
  read: 212      # Dracula pink
  write: 228     # Dracula yellow
  exec: 84       # Dracula green
  exec-sticky: 84
  no-access: 241
  octal: 249
  acl: 249
  context: 249
date:
  hour-old: 141  # Dracula cyan
  day-old: 212   # Dracula pink
  older: 84      # Dracula green
size:
  none: 241
  small: 141     # Dracula cyan
  medium: 228    # Dracula yellow
  large: 212     # Dracula pink
inode:
  valid: 249
  invalid: 241
links:
  valid: 141     # Dracula cyan
  invalid: 212   # Dracula pink
tree-edge: 241   # Gray
git-status:
  default: 249
  unmodified: 249
  ignored: 241
  new-in-index: 84      # Dracula green
  new-in-workdir: 84    # Dracula green
  typechange: 228       # Dracula yellow
  deleted: 212          # Dracula pink
  renamed: 141          # Dracula cyan
  modified: 228         # Dracula yellow
  conflicted: 203       # Dracula red
EOF

# Add aliases to your shell config (will be in your dotfiles, but shown here for reference)
# Add these to your .zshrc or .bashrc:
# alias ls='lsd'
# alias ll='lsd -la'
# alias lt='lsd --tree'

echo "NOTE: Add these aliases to your .zshrc:"
echo "  alias ls='lsd'"
echo "  alias ll='lsd -la'"
echo "  alias lt='lsd --tree'"

# Test lsd with Dracula colors
lsd
lsd -la
```

### 5.11 Theme HTTPie with Dracula colors

```bash
# Create HTTPie config directory
mkdir -p ~/.config/httpie

# Create HTTPie config with Dracula theme
cat > ~/.config/httpie/config.json << 'EOF'
{
  "default_options": [
    "--style=dracula"
  ]
}
EOF

# HTTPie has built-in Dracula style support
# Test HTTPie with Dracula theme
http --print=HhBb https://httpbin.org/get
```

-----

## Phase 6: X11 Configuration

### 6.1 Configure X11 as Default Session

```bash
# Backup GDM custom configuration file.
sudo cp /etc/gdm/custom.conf /etc/gdm/custom.conf.orig

# Disable Wayland in GDM
sudo nvim /etc/gdm/custom.conf

# Add or uncomment this line under [daemon]:
WaylandEnable=false

# Restart GDM to apply changes
sudo systemctl restart gdm
```

### 6.2 Verify X11 Session

```bash
# After logging back in, verify X11 is active:
echo $XDG_SESSION_TYPE
# Should output: x11

# Also check:
loginctl show-session $(loginctl | grep $(whoami) | awk '{print $1}') -p Type
# Should show: Type=x11
```

### 6.3 X11 Display Configuration

```bash
# Install X11 display configuration tools
sudo pacman -S xorg-xrandr arandr

# Configure display for 720p resolution
xrandr --output HDMI-1 --mode 1280x720 --rate 60

# If HDMI-1 doesn't work, check available outputs:
xrandr
# Then use the correct output name (might be HDMI-A-1, VGA-1, etc.)

# Make display settings persistent
arandr  # Use GUI to configure, then save script
```

-----

## Phase 7: GNOME Theming

### 7.1 Install Dracula Theme

```bash
# GTK Theme
git clone https://github.com/dracula/gtk.git
sudo cp -r gtk/Dracula /usr/share/themes/

# Icon Theme
git clone https://github.com/dracula/gtk.git
sudo cp -r gtk/Dracula /usr/share/icons/

# Cursor Theme
yay -S dracula-cursors-git
```

### 7.2 Install TokyoNight Theme (Alternative)

```bash
yay -S tokyonight-gtk-theme-git
yay -S tokyo-night-gtk-theme
```

### 7.3 Apply Themes

```bash
# Set themes using gsettings
gsettings set org.gnome.desktop.interface gtk-theme 'Dracula'
gsettings set org.gnome.desktop.interface icon-theme 'Dracula'
gsettings set org.gnome.desktop.interface cursor-theme 'Dracula-cursors'

# Or use GNOME Tweaks GUI for easier switching between themes
```

-----

## Phase 8: Development Tools Installation

### 8.1 Install ASDF Version Manager

```bash
# Install ASDF
pamac install asdf-vm

# Add to shell configuration
echo '. /opt/asdf-vm/asdf.sh' >> ~/.config/zsh/.zshrc

# Reload shell
exec $SHELL
```

### 8.2 Install Programming Languages via ASDF

#### Prerequisites

```bash
sudo pacman -S base-devel git curl ncurses zlib openssl unixodbc wxwidgets-gtk3 libxslt fop bzip2 readline sqlite tk xz m4
```

#### Python

```bash
asdf plugin add python
asdf install python latest
asdf set -u python latest
```

#### Node.js and Deno

```bash
asdf plugin add nodejs
asdf plugin add deno
asdf plugin add bun

asdf install nodejs latest
asdf install deno latest
asdf install bun latest

asdf set -u nodejs latest
asdf set -u deno latest
asdf set -u bun latest
```

#### Haskell

```bash
asdf plugin add haskell
asdf install haskell 9.10.3
asdf set -u haskell 9.10.3
```

#### Java

```bash
asdf plugin add java
asdf install java temurin-25.0.0+36.0.LTS
asdf set -u java temurin-25.0.0+36.0.LTS
```

#### Kotlin

```bash
asdf plugin add kotlin
asdf install kotlin latest
asdf set -u kotlin latest
```

#### Scala

```bash
asdf plugin add scala
asdf install scala latest
asdf set -u scala latest
```

#### Elixir and Erlang

```bash
asdf plugin add erlang
asdf plugin add elixir

asdf install erlang latest
asdf install elixir latest

asdf set -u erlang latest
asdf set -u elixir latest
```

#### Ruby

```bash
asdf plugin add ruby
asdf install ruby latest
asdf set -u ruby latest
```

#### Perl

```bash
asdf plugin add perl
asdf install perl latest
asdf set -u perl latest
```

### 8.3 Install Go and Rust via System Package Manager

```bash
# Install Go
sudo pacman -S go

# Install Rust
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source ~/.cargo/env
```

-----

## Phase 9: Code Editors Setup

### 9.1 Install VS Code

```bash
pamac install visual-studio-code-bin
```

### 9.2 Install VS Code Extensions

```bash
# Install extensions for all languages
code --install-extension ms-python.python
code --install-extension golang.go
code --install-extension rust-lang.rust-analyzer
code --install-extension phoenixframework.phoenix
code --install-extension haskell.haskell
code --install-extension bradlc.vscode-tailwindcss
code --install-extension esbenp.prettier-vscode
code --install-extension ms-vscode.vscode-typescript-next

# Java and Kotlin
code --install-extension vscjava.vscode-java-pack
code --install-extension mathiasfrohlich.kotlin

# Scala
code --install-extension scala-lang.scala

# Install Dracula theme
code --install-extension dracula-theme.theme-dracula
```

-----

## Phase 10: Final Configuration and Optimization

### 10.1 Set Default Applications

```bash
# Set Alacritty as default terminal (already set in Phase 1, but included here for reference)
gsettings set org.gnome.desktop.default-applications.terminal exec 'alacritty'
```

### 10.2 Create Development Workspace

```bash
# Create projects directory
mkdir -p ~/Projects/{go,rust,python,elixir,javascript,haskell,java,kotlin,scala}
```

-----

## Phase 11: Verification and Testing

### 11.1 Test Language Environments

```bash
# Test all language installations
python --version
go version
rustc --version
node --version
deno --version
bun --version
ghc --version
elixir --version
erl -version
ruby --version
perl --version
java --version
kotlin -version
scala -version
```

### 11.2 Test Editors

```bash
# Launch editors to verify setup
nvim --version
hx --version
code --version
```

### 11.3 Verify X11 Configuration

```bash
# Ensure X11 session is active
echo $XDG_SESSION_TYPE  # Should show: x11
glxinfo | grep "OpenGL renderer"  # Should show your graphics info
xrandr  # Should list available displays
```

### 11.4 Test Backup System

```bash
# Test Timeshift snapshots
sudo timeshift --list
```

For GUI testing:

1. **Open Timeshift** from Applications menu
2. **Browse existing snapshots** in the main window
3. **Create a test snapshot** by clicking “Create”
4. **Test browsing files** by selecting a snapshot and clicking “Browse”

### 11.5 Test Terminal Themes

```bash
# Test Alacritty with Dracula theme
alacritty

# Test tmux with Dracula theme
tmux

# Test Terminator with Dracula theme
terminator

# Test bat with Dracula theme
bat ~/.zshrc

# Test lazygit with Dracula theme
cd ~/Projects
lazygit

# Test Neovim with Dracula theme
nvim

# Test Helix with Dracula theme
hx

# Test lsd with Dracula colors
lsd
lsd -la
lsd --tree

# Test HTTPie with Dracula theme
http https://httpbin.org/get

# Test Starship prompt
starship --version
# Open a new terminal to see Starship in action
```

## How to Use Your TimeMachine-Style Backup

### Restoring Files and System (Timeshift)

1. **Open Timeshift** from Applications menu
2. **Enter admin password** when prompted
3. **Select a snapshot** from the list
4. **For individual files**: Click “Browse” to navigate and copy files out
5. **For full system restore**: Click “Restore” button
6. **Reboot** if doing a full system restore

### Emergency Recovery

If your system won’t boot:

1. **Boot from Manjaro Live USB**
2. **Install Timeshift**: `sudo pacman -S timeshift`
3. **Open Timeshift** from Applications menu
4. **Select your drive**: Timeshift will detect existing snapshots
5. **Restore**: Choose snapshot and restore system

-----

## Troubleshooting

### Common Issues:

1. **ASDF not loading**: Ensure shell configuration is properly sourced
2. **Fonts not displaying**: Run `fc-cache -fv` and restart applications
3. **Theme not applying**: Check file permissions and restart GNOME session
4. **X11 not active**: Verify GDM configuration and restart display manager
5. **Timeshift snapshots missing**: Ensure BTRFS is properly configured and has free space
6. **User files not included**: Check “Include Home directories” in Timeshift settings
7. **Alacritty theme not loading**: Verify Dracula colors are defined in `~/.config/alacritty/alacritty.toml`
8. **Tmux plugins not loading**: Press `prefix + I` inside tmux to install plugins
9. **Starship not showing**: Ensure `eval "$(starship init zsh)"` is in your .zshrc and run `exec $SHELL`

### Performance Optimization for Mac Mini 2012:

- Disable unnecessary GNOME animations in Tweaks
- Use lighter terminal color schemes if performance suffers
- Limit concurrent development servers
- Consider using tmux sessions for better resource management
- Monitor backup drive space regularly
- Keep external backup drive connected for automatic backups

### Backup Maintenance:

- **No manual maintenance required** - Timeshift handles cleanup automatically
- Occasionally check available disk space for snapshots
- Monitor snapshot timeline in Timeshift GUI

-----

## Next Steps

1. Customize Neovim and Helix configurations further based on your coding preferences
2. Set up project-specific configurations
3. Your backup system is now configured and automatic - no further setup needed!
4. Explore additional GNOME extensions for productivity
5. **Optional enhancements:**

- Customize Starship prompt further (see https://starship.rs/config/)
- Customize tmux Dracula plugin options
- Add more bat themes for different file types
- Set up email notifications for backup status
- Configure cloud sync for critical files

## Your Complete Themed Development Environment

**What’s Themed with Dracula:**

- **Terminals**: Alacritty, tmux, Terminator
- **Editors**: Neovim, Helix, VS Code
- **Tools**: bat, lazygit, lsd (ls replacement), HTTPie
- **Shell**: Zsh with Starship prompt
- **GNOME**: GTK theme, icons, cursor

**What’s Automatic:**

- **System snapshots** before updates (Timeshift)
- **User file snapshots** included with system snapshots
- **Scheduled daily/weekly/monthly snapshots**
- **Automatic cleanup** of old snapshots

**Security Features:**

- **BTRFS snapshots** are space-efficient and reliable
- **Single tool simplicity** - no complex backup chains to manage

This setup provides a beautiful, consistent Dracula-themed development environment with reliable data protection, all optimized for your Mac Mini 2012!
