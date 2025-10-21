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

