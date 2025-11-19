## 🧰 Phase 8: Development Tools Installation
> **Goal:** Install programming language managers and development toolchains.

### 8.1 Install Python (via pamac)
```bash
pamac install python
pamac install python314
```

### 8.2 Install NodeJS
```bash
pamac install nodejs
```

### 8.3 Install Java, Kotlin, and Scala (using SDKMAN!)
```bash
# SDKMAN!
curl -s "https://get.sdkman.io" | bash

# Java
sdk install java 25.0.1-tem

# Kotlin
sdk install kotlin 2.2.21

# Scala
sdk install scala 3.7.4
```

### 8.4 Install Go (using standard method from go.dev)
```bash
pushd ~/Downloads
wget https://go.dev/dl/go1.25.4.linux-amd64.tar.gz
sha256sum go1.25.4.linux-amd64.tar.gz # Compare to appropriate checksum at https://go.dev/dl/
sudo rm -rf /usr/local/go
sudo tar -C /usr/local -xzf go1.25.4.linux-amd64.tar.gz
# Make sure you're sourcing `~/.config/dph/golang` in `~/.config/dph/dphrc`
exec $SHELL
go version
```

### 8.5 Install Rust (using standard method from rust-lang.org)
```bash
# Install rustup
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
# Choose option 2, and do not modify path variable.
# Make sure you're sourcing `~/.config/rust` in `~/.config/dph/dphrc`
exec $SHELL
cargo --version
```

> 🧩 Go and Rust are installed using their native tools for best compatibility.

---

