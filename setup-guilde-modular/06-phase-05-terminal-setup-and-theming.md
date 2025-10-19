## 🧛 Phase 5: Terminal Setup and Theming
> **Goal:** Apply consistent Dracula theme across all terminals and editors.

### 5.1 Install Terminal Applications
```bash
sudo pacman -S tmux powerline terminator
pamac install tmux-plugin-manager
```

### 5.2 Apply Dracula Theme to Alacritty
```bash
cat > ~/.config/alacritty/alacritty.toml << 'EOF'
# Dracula Theme
[window]
opacity = 0.95

[font]
size = 12.0

[colors.primary]
background = "#282a36"
foreground = "#f8f8f2"
EOF
```
> 🎨 Restart **Alacritty** to see the changes.

### 5.3 Configure Neovim with Dracula Theme
```bash
mkdir -p ~/.config/nvim/lua/config
mkdir -p ~/.config/nvim/lua/plugins

cat > ~/.config/nvim/init.lua << 'EOF'
-- Minimal Dracula configuration
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
require("lazy").setup("plugins", { change_detection = { notify = false } })
EOF
```

### 5.4 Configure Helix with Dracula Theme
```bash
mkdir -p ~/.config/helix
cat > ~/.config/helix/config.toml << 'EOF'
theme = "dracula"
[editor]
line-number = "relative"
cursorline = true
auto-format = true
EOF
```

### 5.5 Configure tmux with Dracula Theme
```bash
mkdir -p ~/.config/tmux
cat > ~/.config/tmux/tmux.conf << 'EOF'
set -g prefix C-a
unbind C-b
set -g mouse on
set -g default-terminal "screen-256color"
set -g @plugin 'tmux-plugins/tpm'
set -g @plugin 'dracula/tmux'
run '~/.config/tmux/plugins/tpm/tpm'
EOF
git clone https://github.com/tmux-plugins/tpm ~/.config/tmux/plugins/tpm
~/.config/tmux/plugins/tpm/bin/install_plugins
```

### 5.6 Configure Terminator with Dracula Theme
```bash
mkdir -p ~/.config/terminator
cat > ~/.config/terminator/config << 'EOF'
[global_config]
  title_hide_sizetext = True
[keybindings]
[profiles]
  [[default]]
    background_color = "#282a36"
    font = Hack Nerd Font 12
    foreground_color = "#f8f8f2"
EOF
```

### 5.7 Configure Starship Prompt
```bash
pamac install starship
mkdir -p ~/.config
cat > ~/.config/starship.toml << 'EOF'
add_newline = true
format = "$directory$git_branch$git_status$cmd_duration$python$nodejs$rust"
[directory]
style = "bold cyan"
[git_branch]
style = "bold purple"
[cmd_duration]
min_time = 500
format = "took [$duration](bold yellow)"
EOF
echo 'eval "$(starship init zsh)"' >> ~/.zshrc
exec $SHELL
```

### 5.8 Theme bat, lazygit, and lsd
```bash
# bat
mkdir -p "$(bat --config-dir)/themes"
wget -P "$(bat --config-dir)/themes" https://github.com/dracula/sublime/raw/master/Dracula.tmTheme
bat cache --build
echo '--theme="Dracula"' > ~/.config/bat/config

# lazygit
mkdir -p ~/.config/lazygit
cat > ~/.config/lazygit/config.yml << 'EOF'
gui:
  theme:
    activeBorderColor: ['#ff79c6', 'bold']
EOF

# lsd
mkdir -p ~/.config/lsd
cat > ~/.config/lsd/colors.yaml << 'EOF'
permission:
  read: 212
  write: 228
  exec: 84
EOF
```

### 5.9 Theme HTTPie with Dracula
```bash
mkdir -p ~/.config/httpie
cat > ~/.config/httpie/config.json << 'EOF'
{
  "default_options": ["--style=dracula"]
}
EOF
```
> 💡 HTTPie includes Dracula styling by default.

---

