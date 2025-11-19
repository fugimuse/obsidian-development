## 💿 Phase 1: Base Installation
> **Goal:** Install and configure Manjaro GNOME with a BTRFS filesystem and minimal base tools.

### 1.1 Create Installation Media
```bash
# On Linux
sudo dd if=manjaro-gnome-*.iso of=/dev/sdX bs=1M status=progress

# On macOS (Catalina and newer - if status is supported)
#sudo dd if=manjaro-gnome-*.iso of=/dev/diskX bs=1m status=progress

# On macOS (older versions without status support)
# Option 1: Without progress indicator
#sudo dd if=manjaro-gnome-*.iso of=/dev/diskX bs=1m

# Option 2: With pv for progress (install with: brew install pv)
#pv manjaro-gnome-*.iso | sudo dd of=/dev/diskX bs=1m

# Option 3: Send SIGINFO signal while dd is running
# In another terminal, run: `sudo killall -INFO dd`
# This shows progress each time you run it
```

> ⚠️ **Note:** Linux uses uppercase `bs=1M`, macOS uses lowercase `bs=1m`
> ⚠️ **Important:** Replace `/dev/sdX` (Linux) or `/dev/diskX` (macOS) with your actual USB drive identifier
> 💡 **Tip:** Find your USB drive with `lsblk` on Linux or `diskutil list` on macOS
> 💡 **macOS progress:** Use `Ctrl+T` while dd is running to see progress, or use `pv` for continuous updates

### 1.2 Install Manjaro GNOME with BTRFS
1. Boot from USB (**hold Option key during startup**)  
2. Select “Boot with open source drivers”  
3. Launch **Calamares installer**  
4. Choose “Replace a partition” and select your pre-created partition  
5. Configure filesystem – select **BTRFS**; Calamares handles subvolumes automatically  
6. Set timezone and create user account  
7. Reboot when installation completes  

### 1.3 Initial Settings
- 🧩 **Display and Keyboard Tweaks**  
    - **Resolution:** 2048×1152 (16:9)  
    - **Keyboard Options:** Swap Esc ↔ Caps Lock; Left Alt as Ctrl, Left Ctrl as Win  

- 🧩 **GNOME Extensions**  
    - ✅ Apps Menu
    - ❌ Dash to Dock
    - ✅ Dash to Panel
    - ✅ System Monitor
    - ✅ Workspace Indicator  

- 🧩 **Terminal Preferences**
    – Enable Unlimited Scrollback  

### 1.4 Configure Fastest Mirrors
```bash
sudo pacman-mirrors --country United_States,Canada --api --set-branch stable --protocol https
sudo pacman -Syyu
```

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

### 1.6 Essential Tools
```bash
sudo pacman -Syu
sudo pacman -S base-devel git lazygit curl wget btrfs-progs ripgrep fd make unzip gcc xclip neovim obsidian bat lsd httpie terminator

# Make Terminator the default GNOME terminal
gsettings set org.gnome.desktop.default-applications.terminal exec '/usr/bin/terminator'

# Ensure that 'Open in Terminal' functionality works
gsettings set org.gnome.desktop.default-applications.terminal exec-arg '-x'
```
> 💡 **Note:** `lsd` replaces both `ls` and `tree`.

### 1.7 Configure pamac on Manjaro

- Open Add/Remove Software | Preferences
    - General
        - Updates
            - ✅ Check for updates
            - Updates check frequency: every 6 hours
            - ❌ Automatically download updates
            - ✅ Upgrade the system at shutdown
            - ❌ Hide tray icon when no update
        - Downloads 
            - Parallel downloads: 4
        - Official Repositories 
            - Use mirrors from: United_States
        - Cache 
            - Number of versions of each package to keep: 3
            - ❌ Remove only the uninstalled packages
    - Advanced
        - Advanced
            - ✅ Check available disk space
            - ✅ Remove unrequired dependencies
            - ✅ Do not check for updates when installing
            - ❌ Enable downgrade
        - Ignored Upgrades
            - None
    - Third Party
        - AUR
            - ✅ Enable AUR support
            - ✅ Keep built packages
            - ✅ Check for updates
            - ❌ Check for development packages updates
            - Build directory: `tmp`
        - Flatpak
            - ✅ Enable Flatpak support
            - ✅ Check for updates 

### 1.8 Install Basic Tools with pamac
```bash
pamac search 1password
pamac install 1password

pamac search github-cli
pamac install github-cli
```

### 1.9 Install NoMachine for Remote Access
```bash
pamac install nomachine
sudo systemctl enable nxserver
sudo systemctl start nxserver
```
> 💡 **Repo:** https://github.com/fmconfig/nomachine
> 🔐 This allows remote desktop access from iPad, laptops, etc.

### 1.10 Verify BTRFS Setup
```bash
df -T /
sudo btrfs subvolume list /
sudo btrfs filesystem show
sudo btrfs filesystem usage /
```

---

