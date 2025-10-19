## 🔐 Phase 3: Setup SSH
> **Goal:** Create secure SSH access for GitHub and other repositories.

### 3.1 Generate SSH Key
```bash
ssh-keygen -t ed25519 -f ~/.ssh/mefm_ed25519 -C "fugimuse@gmail.com"
```

### 3.2 Copy Public Key
```bash
cat ~/.ssh/mefm_ed25519.pub
```
Copy the displayed public key to your GitHub, GitLab, and Bitbucket SSH-key settings.

### 3.3 Configure SSH for Named Host
```bash
cat >> ~/.ssh/config << "EOF"
Host mefm.github.com
    HostName github.com
    User fugimuse
    IdentityFile ~/.ssh/mefm_ed25519
    IdentitiesOnly yes
EOF
```

---

