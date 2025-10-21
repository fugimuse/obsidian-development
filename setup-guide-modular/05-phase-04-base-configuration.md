## ⚙️ Phase 4: Base Configuration
> **Goal:** Clone your configuration repositories and apply system dotfiles.

### 4.1 Clone Configuration Repositories
```bash
git clone git@mefm.github.com:fmconfig/bash.git ~/.config/bash
git clone git@mefm.github.com:fmconfig/dotfiles.git ~/.config/dotfiles
git clone git@mefm.github.com:fmconfig/nvim.git ~/.config/nvim
git clone git@mefm.github.com:fmconfig/zsh.git ~/.config/zsh
```

### 4.2 Setup Home Directory Links
```bash
pushd ~/.config/dotfiles
./remove_links
./setup_links
popd
```

### 4.3 Restart Shell
```bash
exec $SHELL
```

### 4.4 Install Oh My Zsh
```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
exec $SHELL
```

---

