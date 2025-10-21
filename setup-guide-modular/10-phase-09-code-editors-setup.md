## 🧠 Phase 9: Code Editors Setup
> **Goal:** Install and configure VS Code and essential extensions for all major languages.

### 9.1 Install Visual Studio Code
```bash
pamac install visual-studio-code-bin
```

### 9.2 Install VS Code Extensions
```bash
# Language support
code --install-extension ms-python.python
code --install-extension golang.go
code --install-extension rust-lang.rust-analyzer
code --install-extension phoenixframework.phoenix
code --install-extension haskell.haskell
code --install-extension bradlc.vscode-tailwindcss
code --install-extension esbenp.prettier-vscode
code --install-extension ms-vscode.vscode-typescript-next

# JVM languages
code --install-extension vscjava.vscode-java-pack
code --install-extension mathiasfrohlich.kotlin
code --install-extension scala-lang.scala

# Theming
code --install-extension dracula-theme.theme-dracula
```

> 💡 **Tip:** Launch VS Code and open the Command Palette → “Reload Window” after installing extensions to apply the Dracula theme.

---

