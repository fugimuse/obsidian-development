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

