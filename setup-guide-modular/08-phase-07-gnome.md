## 🎨 Phase 7: GNOME Theming
> **Goal:** Apply Dracula and TokyoNight themes for a consistent desktop look.

### 7.1 Install Dracula Theme

#### 7.1.1: Create Directory and Download the Assets
```bash
# Create download directory
mkdir -p ~/.config/themes/dracula/gtk
cd ~/.config/themes/dracula/gtk

# Download main GTK theme
wget https://github.com/dracula/gtk/releases/download/v4.0.0/Dracula.tar.xz

# Download GNOME Shell theme
wget https://github.com/dracula/gtk/releases/download/v4.0.0/Dracula-shell-v40.tar.xz

# Optional: Download cursor theme
wget https://github.com/dracula/gtk/releases/download/v4.0.0/Dracula-cursors.tar.xz
```

#### 7.1.2: Extract and Symlink GTK Theme
```bash
# Extract the GTK theme (creates Dracula directory)
tar -xf Dracula.tar.xz

# Create themes directory if needed
mkdir -p ~/.local/share/themes

# Create symlink
ln -s ~/.config/themes/dracula/gtk/Dracula ~/.local/share/themes/Dracula
```

#### 7.1.3: Extract and Symlink GNOME Shell Theme
```bash
# Extract the Shell theme (creates Dracula directory)
tar -xf Dracula-shell-v40.tar.xz

# Create gnome-shell themes directory if needed
mkdir -p ~/.local/share/gnome-shell/themes

# Create symlink (note: shell theme also extracts as "Dracula")
ln -s ~/.config/themes/dracula/gtk/Dracula ~/.local/share/gnome-shell/themes/Dracula
```

#### 7.1.4: Extract and Symlink Cursor Theme
```bash
# Extract cursors
tar -xf Dracula-cursors.tar.xz

# Create icons directory if needed
mkdir -p ~/.local/share/icons

# Create symlink
ln -s ~/.config/themes/dracula/gtk/Dracula-cursors ~/.local/share/icons/Dracula-cursors
```

#### 7.1.5: Apply GTK3/GTK2 Theme
```bash
# Apply GTK theme for GTK3/GTK2 applications
gsettings set org.gnome.desktop.interface gtk-theme "Dracula"
gsettings set org.gnome.desktop.wm.preferences theme "Dracula"
```

#### 7.1.6: Apply GTK4/Libadwaita Theme
```bash
# Create GTK4 config directory
mkdir -p ~/.config/gtk-4.0

# Copy GTK4 CSS files
cp ~/.local/share/themes/Dracula/gtk-4.0/gtk.css ~/.config/gtk-4.0/
cp ~/.local/share/themes/Dracula/gtk-4.0/gtk-dark.css ~/.config/gtk-4.0/

# Copy assets folder
cp -r ~/.local/share/themes/Dracula/assets ~/.config/
```

#### 7.1.7: Apply GNOME Shell Theme
```bash
# Install GNOME Shell extensions tool if not already installed
#pamac install gnome-shell-extensions

# Enable user themes extension
#gnome-extensions enable user-theme@gnome-shell-extensions.gcx.io

# Apply the Shell theme
gsettings set org.gnome.shell.extensions.user-theme name "Dracula"
```

#### 7.1.8: Apply Cursor Theme
```bash
gsettings set org.gnome.desktop.interface cursor-theme "Dracula-cursors"
```

#### 7.1.9: Restart GNOME Shell
```bash
# Press Alt+F2, type 'r' and press Enter
# Or log out and log back in
```

### 7.2 Install TokyoNight Theme (Optional)

- [Wallpapers](https://github.com/tokyo-night/wallpapers)
- [VSCode](https://github.com/tokyo-night/tokyo-night-vscode-theme)
- [NeoVim](https://github.com/folke/tokyonight.nvim)
- [GNOME Terminal](https://github.com/folke/tokyonight.nvim/tree/main/extras/gnome_terminal)
- [LazyGit](https://github.com/folke/tokyonight.nvim/tree/main/extras/lazygit)
- [Lua](https://github.com/folke/tokyonight.nvim/tree/main/extras/lua)
- [OpenCode](https://github.com/folke/tokyonight.nvim/tree/main/extras/opencode)
- [Terminator](https://github.com/folke/tokyonight.nvim/tree/main/extras/terminator)

> 🎨 Or use **GNOME Tweaks** for easier switching between themes.

---

