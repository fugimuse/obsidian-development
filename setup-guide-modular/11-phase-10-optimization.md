## 🚀 Phase 10: Final Configuration and Optimization
> **Goal:** Finalize system defaults, create project workspaces, and optimize GNOME for performance.

### 10.1 Set Default Applications
```bash
# Set Terminator as the default terminal
gsettings set org.gnome.desktop.default-applications.terminal exec 'terminator'
```

### 10.2 Create Development Workspaces
```bash
# Organize projects by language
mkdir -p ~/proj/{go,rust,python,elixir,javascript,haskell,java,kotlin,scala}
```

### 10.3 Optimize GNOME for Performance (Mac Mini 2012)
- Disable unnecessary GNOME animations in **Tweaks → Appearance → Animations**  
- Use lightweight extensions (disable weather, media players, etc.)  
- Limit background services  
- Prefer **X11** over Wayland for smoother GPU handling  
- Monitor CPU and RAM usage with the **System Monitor** GNOME extension

---

