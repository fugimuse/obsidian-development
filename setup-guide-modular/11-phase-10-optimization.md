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

