## 🧛 Phase 5: Terminal Setup and Theming
> **Goal:** Apply consistent Dracula theme across all terminals and editors.

### 5.1 Configure Terminator with Dracula Theme
```bash
git clone git@mefm.github.com:dracula/terminator.git ~/.config/themes/dracula/terminator
# Edit config/config: change 'Anonymous Pro 12' to 'Hack Nerd Font 14', then run
# the following command.
./install.sh
```
> 💡 **Repo:** https://github.com/fmconfig/terminator (placeholder)

### 5.2 Configure Neovim with Dracula Theme
```bash
# Neovim config should already be cloned from Phase 4
# If not, clone it now:
git clone git@mefm.github.com:fmconfig/nvim.git ~/.config/nvim
```
> 💡 **Repo:** https://github.com/fmconfig/nvim

### 5.3 Theme bat, lazygit, and lsd
```bash
# bat
git clone git@mefm.github.com:fmconfig/bat.git ~/.config/bat-repo
mkdir -p "$(bat --config-dir)/themes"
wget -P "$(bat --config-dir)/themes" https://github.com/dracula/sublime/raw/master/Dracula.tmTheme
bat cache --build
cp ~/.config/bat-repo/config ~/.config/bat/config

# lazygit
git clone git@mefm.github.com:fmconfig/lazygit.git ~/.config/lazygit

# lsd
git clone git@mefm.github.com:fmconfig/lsd.git ~/.config/lsd
```
> 💡 **Repos:**
> - https://github.com/fmconfig/bat
> - https://github.com/fmconfig/lazygit
> - https://github.com/fmconfig/lsd

### 5.4 Theme HTTPie with Dracula
```bash
git clone git@mefm.github.com:fmconfig/httpie.git ~/.config/httpie
```
> 💡 HTTPie includes Dracula styling by default.
> 💡 **Repo:** https://github.com/fmconfig/httpie

### 5.5 Theme Obsidian with Dracula

> TODO: Add instructions for theming Obsidian with Dracula

### 5.6 Theme Zed with Dracula

> TODO: Add instructions for theming Zed with Dracula

---
