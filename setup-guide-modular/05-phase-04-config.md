## ⚙️ Phase 4: Base Configuration
> **Goal:** Clone your configuration repositories and apply system dotfiles.

### 4.1 Clone Configuration Repositories
```bash
git clone git@mefm.github.com:fmconfig/bash.git ~/.config/bash
git clone git@mefm.github.com:fmconfig/bat.git ~/.config/bat
git clone git@mefm.github.com:fmconfig/dotfiles.git ~/.config/dotfiles
git clone git@mefm.github.com:fmconfig/dph.git ~/.config/dph
git clone git@mefm.github.com:fmconfig/fonts.git ~/.config/fonts
git clone git@mefm.github.com:fmconfig/httpie.git ~/.config/httpie
git clone git@mefm.github.com:fmconfig/lazygit.git ~/.config/lazygit
git clone git@mefm.github.com:fmconfig/lsd.git ~/.config/lsd
git clone git@mefm.github.com:fmconfig/nvim.git ~/.config/nvim
git clone git@mefm.github.com:fmconfig/terminator.git ~/.config/terminator
git clone git@mefm.github.com:fmconfig/themes.git ~/.config/themes
git clone git@mefm.github.com:fmconfig/zed.git ~/.config/zed
git clone git@mefm.github.com:fmconfig/zsh.git ~/.config/zsh
```

### 4.2 Install Nerd Fonts
```bash
pushd ~/.config/fonts
    ./install.sh
popd
```

### 4.3 Setup Home Directory Links
```bash
pushd ~/.config/dotfiles
    ./remove_links
    ./setup_links
popd
```

### 4.4 Restart Shell
```bash
exec $SHELL
```

### 4.5 Install Oh My Zsh
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
exec $SHELL
```

---
