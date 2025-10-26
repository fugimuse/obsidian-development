# 🦇 Manjaro GNOME Development Machine Setup Guide
_A fully themed Dracula-powered developer environment for Mac Mini (2012)_

---

## 🧾 Table of Contents
- [Prerequisites](#prerequisites)
- [💿 Phase 1: Base Installation](#-phase-1-base-installation)
- [💾 Phase 2: TimeMachine-Style Backup System](#-phase-2-timemachine-style-backup-system)
- [🔐 Phase 3: Setup SSH](#-phase-3-setup-ssh)
- [⚙️ Phase 4: Base Configuration](#%EF%B8%8F-phase-4-base-configuration)
- [🧛 Phase 5: Terminal Setup and Theming](#-phase-5-terminal-setup-and-theming)
- [🖥️ Phase 6: X11 Configuration](#%EF%B8%8F-phase-6-x11-configuration)
- [🎨 Phase 7: GNOME Theming](#-phase-7-gnome-theming)
- [🧰 Phase 8: Development Tools Installation](#-phase-8-development-tools-installation)
- [🧠 Phase 9: Code Editors Setup](#-phase-9-code-editors-setup)
- [🚀 Phase 10: Final Configuration and Optimization](#-phase-10-final-configuration-and-optimization)
- [🧪 Phase 11: Verification and Testing](#-phase-11-verification-and-testing)
- [🩹 Troubleshooting](#-troubleshooting)
- [🏁 Next Steps](#-next-steps)

---

## 🧰 Prerequisites
✅ **Before you begin, make sure you have:**
- Mac Mini 2012 with SSD  
- Pre-created partition for Linux installation  
- Manjaro GNOME ISO downloaded  
- USB drive for installation media  

---

## 💿 Phase 1: Base Installation
> **Goal:** Install and configure Manjaro GNOME with a BTRFS filesystem and minimal base tools.

### 1.1 Create Installation Media
```bash
# On macOS (if needed)
sudo dd if=manjaro-gnome-*.iso of=/dev/diskX bs=1m status=progress
```

### 1.2 Install Manjaro GNOME with BTRFS
1. Boot from USB (**hold Option key during startup**)  
2. Select “Boot with open source drivers”  
3. Launch **Calamares installer**  
4. Choose “Replace a partition” and select your pre-created partition  
5. Configure filesystem – select **BTRFS**; Calamares handles subvolumes automatically  
6. Set timezone and create user account  
7. Reboot when installation completes  

### 1.3 Initial Settings
🧩 **Display and Keyboard Tweaks**  
- **Resolution:** 2048×1152 (16:9)  
- **Keyboard Options:** Swap Esc ↔ Caps Lock; Left Alt as Ctrl, Left Ctrl as Win  

🧩 **GNOME Extensions**  
✅ ArcMenu ❌ Dash to Dock ✅ Dash to Panel ✅ System Monitor ✅ Workspace Indicator  

🧩 **Terminal Preferences** – Enable Unlimited Scrollback  

### 1.4 Configure Fastest Mirrors
```bash
sudo pacman-mirrors --fasttrack 5 && sudo pacman -Sy
sudo pacman -Syyu
```
> 💡 **Tip:** For specific countries, use `sudo pacman-mirrors --country-list`.

### 1.5 Create Swap File
> ⚠️ **Note:** Skip this step if you have plenty of RAM.
```bash
sudo touch /swapfile
sudo chattr +C /swapfile
sudo dd if=/dev/zero of=/swapfile bs=1M count=4096 status=progress
sudo chmod 600 /swapfile
sudo mkswap /swapfile
sudo swapon /swapfile
echo '/swapfile none swap defaults 0 0' | sudo tee -a /etc/fstab
sudo swapon --show
```

### 1.6 Install Nerd Fonts
```bash
mkdir -p ~/.local/share/fonts
wget https://github.com/ryanoasis/nerd-fonts/releases/download/v3.0.2/Hack.zip
unzip Hack.zip -d ~/.local/share/fonts/Hack/
fc-cache -fv
gsettings set org.gnome.desktop.interface monospace-font-name 'Hack Nerd Font 12'
```

### 1.7 Essential Tools
```bash
sudo pacman -Syu
sudo pacman -S base-devel git lazygit curl wget btrfs-progs ripgrep fd make unzip gcc xclip neovim helix obsidian bat lsd httpie alacritty
```
> 💡 **Note:** `lsd` replaces both `ls` and `tree`.

### 1.8 Minimal Alacritty Configuration
```bash
mkdir -p ~/.config/alacritty
cat > ~/.config/alacritty/alacritty.toml << 'EOF'
[window]
opacity = 0.95
[font]
size = 12.0
[font.normal]
family = "Hack Nerd Font"
style = "Regular"
[colors.primary]
background = "#1e1e1e"
foreground = "#d4d4d4"
EOF
gsettings set org.gnome.desktop.default-applications.terminal exec 'alacritty'
```

### 1.9 Configure pamac on Manjaro
```bash
sudo cp /etc/pamac.conf /etc/pamac.conf.orig
sudo sed -i '/^#RemoveUnrequiredDeps/s/^#//g' /etc/pamac.conf
sudo sed -i '/^#EnableAUR/s/^#//g' /etc/pamac.conf
sudo sed -i '/^#KeepBuiltPkgs/s/^#//g' /etc/pamac.conf
sudo sed -i '/^#OfflineUpgrade/s/^#//g' /etc/pamac.conf
sudo sed -i '/^#EnableFlatpak/s/^#//g' /etc/pamac.conf
exec $SHELL
```

### 1.10 Install Basic Tools with pamac
```bash
pamac search 1password
pamac install 1password
```

### 1.11 Verify BTRFS Setup
```bash
df -T /
sudo btrfs subvolume list /
sudo btrfs filesystem show
sudo btrfs filesystem usage /
```

---

## 💾 Phase 2: TimeMachine-Style Backup System
> **Goal:** Configure automatic, BTRFS-based snapshots with Timeshift.

### 2.1 Configure Timeshift
1. Open **Timeshift** from the Applications menu.  
2. Select **BTRFS** snapshot type.  
3. Configure snapshot schedule – Boot 3, Daily 5, Weekly 3, Monthly 2.  
4. Enable “Include Home directories.”  
5. Finish setup.  

> 💾 Timeshift will now automatically create and manage snapshots.

### 2.2 Optional: External Backup Drive
> 🔐 Connect your external drive and select it under *Settings → Backup Device* in Timeshift.

### 2.3 Test Your Backup Setup
```bash
sudo timeshift --create --comments "Initial setup test"
sudo timeshift --list
```
> 💡 You can also test manually via GUI → **Create Snapshot**.

### 2.4 Verify Automatic Backups
Open Timeshift → Settings → Schedule and confirm daily/weekly/monthly snapshot routines.

---

## 🔐 Phase 3: Setup SSH
> **Goal:** Create secure SSH access for GitHub and other repositories.

### 3.1 Generate SSH Key
```bash
ssh-keygen -t ed25519 -f ~/.ssh/mefm_ed25519 -C "fugimuse@gmail.com"
```

### 3.2 Copy Public Key
```bash
cat ~/.ssh/mefm_ed25519.pub
```
Copy the displayed public key to your GitHub, GitLab, and Bitbucket SSH-key settings.

### 3.3 Configure SSH for Named Host
```bash
cat >> ~/.ssh/config << "EOF"
Host mefm.github.com
    HostName github.com
    User fugimuse
    IdentityFile ~/.ssh/mefm_ed25519
    IdentitiesOnly yes
EOF
```

---

## ⚙️ Phase 4: Base Configuration
> **Goal:** Clone your configuration repositories and apply system dotfiles.

### 4.1 Clone Configuration Repositories
```bash
git clone git@mefm.github.com:fmconfig/bash.git ~/.config/bash
git clone git@mefm.github.com:fmconfig/dotfiles.git ~/.config/dotfiles
git clone git@mefm.github.com:fmconfig/nvim.git ~/.config/nvim
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

### 4.4 Install Oh My Zsh
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
exec $SHELL
```

---

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

## 🖥️ Phase 6: X11 Configuration
> **Goal:** Switch GNOME to use X11 instead of Wayland for stability on older Mac Mini hardware.

### 6.1 Configure X11 as Default Session
```bash
sudo cp /etc/gdm/custom.conf /etc/gdm/custom.conf.orig
sudo nvim /etc/gdm/custom.conf
# Add or uncomment under [daemon]:
WaylandEnable=false
sudo systemctl restart gdm
```

### 6.2 Verify X11 Session
```bash
echo $XDG_SESSION_TYPE  # should output x11
loginctl show-session $(loginctl | grep $(whoami) | awk '{print $1}') -p Type
```

### 6.3 Display Configuration
```bash
sudo pacman -S xorg-xrandr arandr
xrandr --output HDMI-1 --mode 1280x720 --rate 60
```
> 💡 Use `arandr` GUI to save display layouts persistently.

---

## 🎨 Phase 7: GNOME Theming
> **Goal:** Apply Dracula and TokyoNight themes for a consistent desktop look.

### 7.1 Install Dracula Theme
```bash
git clone https://github.com/dracula/gtk.git
sudo cp -r gtk/Dracula /usr/share/themes/
sudo cp -r gtk/Dracula /usr/share/icons/
yay -S dracula-cursors-git
```

### 7.2 Install TokyoNight Theme (Optional)
```bash
yay -S tokyonight-gtk-theme-git
```

### 7.3 Apply Themes
```bash
gsettings set org.gnome.desktop.interface gtk-theme 'Dracula'
gsettings set org.gnome.desktop.interface icon-theme 'Dracula'
gsettings set org.gnome.desktop.interface cursor-theme 'Dracula-cursors'
```
> 🎨 Or use **GNOME Tweaks** for easier switching between themes.

---

## 🧰 Phase 8: Development Tools Installation
> **Goal:** Install programming language managers and development toolchains.

### 8.1 Install ASDF Version Manager
```bash
pamac install asdf-vm
echo '. /opt/asdf-vm/asdf.sh' >> ~/.config/zsh/.zshrc
exec $SHELL
```

### 8.2 Install Programming Languages via ASDF

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

#### Elixir and Erlang
```bash
asdf plugin add erlang
asdf plugin add elixir
asdf install erlang latest
asdf install elixir latest
asdf set -u erlang latest
asdf set -u elixir latest
```

#### Ruby and Perl
```bash
asdf plugin add ruby
asdf install ruby latest
asdf set -u ruby latest

asdf plugin add perl
asdf install perl latest
asdf set -u perl latest
```

#### Java, Kotlin, and Scala
```bash
asdf plugin add java
asdf plugin add kotlin
asdf plugin add scala
asdf install java latest
asdf install kotlin latest
asdf install scala latest
asdf set -u java latest
asdf set -u kotlin latest
asdf set -u scala latest
```

### 8.3 Install Go and Rust
```bash
sudo pacman -S go
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
source ~/.cargo/env
```
> 🧩 Go and Rust are installed via their native tools for best compatibility.

---

## 🧠 Phase 9: Code Editors Setup
> **Goal:** Install and configure VS Code and essential extensions for all major languages.

### 9.1 Install Visual Studio Code
```bash
pamac install visual-studio-code-bin
```

### 9.2 Install VS Code Extensions
```bash
# Language support
code --install-extension ms-python.python
code --install-extension golang.go
code --install-extension rust-lang.rust-analyzer
code --install-extension phoenixframework.phoenix
code --install-extension haskell.haskell
code --install-extension bradlc.vscode-tailwindcss
code --install-extension esbenp.prettier-vscode
code --install-extension ms-vscode.vscode-typescript-next

# JVM languages
code --install-extension vscjava.vscode-java-pack
code --install-extension mathiasfrohlich.kotlin
code --install-extension scala-lang.scala

# Theming
code --install-extension dracula-theme.theme-dracula
```

> 💡 **Tip:** Launch VS Code and open the Command Palette → “Reload Window” after installing extensions to apply the Dracula theme.

---

## 🚀 Phase 10: Final Configuration and Optimization
> **Goal:** Finalize system defaults, create project workspaces, and optimize GNOME for performance.

### 10.1 Set Default Applications
```bash
# Set Alacritty as the default terminal
gsettings set org.gnome.desktop.default-applications.terminal exec 'alacritty'
```

### 10.2 Create Development Workspace
```bash
# Organize projects by language
mkdir -p ~/Projects/{go,rust,python,elixir,javascript,haskell,java,kotlin,scala}
```
> 💡 **Tip:** Use consistent folder naming for ASDF-managed projects.

### 10.3 Optimize GNOME for Performance (Mac Mini 2012)
- Disable unnecessary GNOME animations in **Tweaks → Appearance → Animations**  
- Use lightweight extensions (disable weather, media players, etc.)  
- Limit background services  
- Prefer **X11** over Wayland for smoother GPU handling  
- Monitor CPU and RAM usage with the **System Monitor** GNOME extension

---

## 🧪 Phase 11: Verification and Testing
> **Goal:** Verify that all installations, tools, editors, and backup systems are functioning properly.

### 11.1 Test Language Environments
```bash
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
nvim --version
hx --version
code --version
```

### 11.3 Verify X11 Configuration
```bash
echo $XDG_SESSION_TYPE  # should output x11
glxinfo | grep "OpenGL renderer"
xrandr
```

### 11.4 Test Backup System
```bash
sudo timeshift --list
```

### 11.5 Test Theming and CLI Tools
```bash
# Terminals
alacritty
tmux
terminator

# Tools
bat ~/.zshrc
lazygit
lsd -la
http https://httpbin.org/get
```
> ✅ Confirm all Dracula colors appear correctly and your prompt loads with Starship.

---

## 🩹 Troubleshooting
> **Goal:** Quickly diagnose and fix common setup issues.

### Common Issues and Fixes

- ⚠️ **ASDF not loading:**  
  Ensure your shell configuration sources the ASDF script.  
  ```bash
  echo '. /opt/asdf-vm/asdf.sh' >> ~/.zshrc
  exec $SHELL
  ```

- 🧱 **Fonts not displaying correctly:**  
  Rebuild the font cache and restart applications.  
  ```bash
  fc-cache -fv
  ```

- 💻 **Theme not applying:**  
  Ensure permissions are correct and restart the GNOME session.  
  ```bash
  gsettings set org.gnome.desktop.interface gtk-theme 'Dracula'
  gnome-shell --replace &
  ```

- 🔄 **X11 not active:**  
  Verify that Wayland is disabled in the GDM configuration and restart GDM.  
  ```bash
  sudo nvim /etc/gdm/custom.conf
  # Ensure WaylandEnable=false
  sudo systemctl restart gdm
  ```

- 🧰 **Starship not loading:**  
  Make sure the initialization line exists in your `.zshrc`.  
  ```bash
  eval "$(starship init zsh)"
  exec $SHELL
  ```

- 🧛 **Tmux plugins not loading:**  
  Open tmux and press `Ctrl+A` then `I` to install plugins manually.

- 💾 **Timeshift snapshots missing:**  
  Confirm Timeshift is configured for BTRFS and has enough free space.  
  ```bash
  sudo timeshift --list
  ```

> 💡 **Performance Tips for Mac Mini 2012:**  
> - Disable unnecessary GNOME animations  
> - Keep your background apps minimal  
> - Use X11 for smoother graphics  
> - Periodically clean up old Timeshift snapshots  

---

## 🏁 Next Steps
🎯 **Your Environment Is Ready!**

Now that your system is fully configured, here’s what you can do next:

1. **Customize your workflow**
   - Tune Neovim and Helix further to match your preferences  
   - Add language-specific LSP plugins via Lazy.nvim  

2. **Enhance backup reliability**
   - Plug in an external drive for redundant Timeshift backups  
   - Optionally, sync important folders to the cloud  

3. **Keep it beautiful**
   - Try other Dracula-compatible themes for apps like Obsidian and Firefox  
   - Add new GNOME extensions for productivity  

4. **Optimize and maintain**
   - Use `sudo pacman -Syu` weekly to stay up to date  
   - Verify Timeshift snapshots after major updates  
   - Clean up cached packages with `sudo paccache -r`  

5. **Explore more**
   - Add containerized dev environments (Podman, DevPod, or Docker)  
   - Configure Terraform, Go, and Rust projects with reproducible dev containers  

---

## ✅ Summary

You now have:

- A **fully themed Dracula environment** across GNOME, terminals, and editors  
- **ASDF-managed language runtimes** for all major stacks  
- **Automatic, snapshot-based backups** via Timeshift  
- **Optimized X11 graphics** for the 2012 Mac Mini  
- A **consistent developer experience** across all tools  

> 🧛‍♂️ Enjoy your sleek, powerful, and reliable **Manjaro GNOME Development Environment** — crafted for elegance, resilience, and creativity!
