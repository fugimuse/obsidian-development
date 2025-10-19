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

