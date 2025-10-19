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

