# Starship Prompt Setup Guide

## Overview

Starship is a fast, minimal, and highly customizable cross-shell prompt written in Rust. This guide replaces the Powerlevel10k setup with Starship.

## Why Starship?

- **Actively maintained** with regular updates
- **Cross-shell compatible** (works with Zsh, Bash, Fish, PowerShell)
- **Fast** - written in Rust, minimal startup time
- **Highly customizable** via TOML configuration
- **Context-aware** - shows git status, language versions, package info, etc.
- **Beautiful defaults** that work out of the box

---

## Installation

### Step 1: Install Starship

```bash
# Install via pamac
pamac install starship

# Or install via cargo (if you prefer)
cargo install starship --locked
```

### Step 2: Add to Zsh Configuration

```bash
# Add to the end of your ~/.zshrc (or ~/.config/zsh/.zshrc)
eval "$(starship init zsh)"
```

### Step 3: Reload Shell

```bash
exec $SHELL
```

At this point, Starship is installed and working with default settings!

---

## Configuration

Starship is configured via a TOML file at `~/.config/starship.toml`

### Basic Dracula-Themed Configuration

Create a configuration file that matches your Dracula theme:

```bash
# Create starship config directory
mkdir -p ~/.config

# Create basic Dracula-themed configuration
cat > ~/.config/starship.toml << 'EOF'
# Starship Configuration with Dracula Colors
# Get editor completions based on the config schema
"$schema" = 'https://starship.rs/config-schema.json'

# Inserts a blank line between shell prompts
add_newline = true

# Format of the prompt
format = """
[┌───────────────────────────────────────────────────>](bold purple)
[│](bold purple)$directory$git_branch$git_status$git_state
[│](bold purple)$python$nodejs$deno$bun$golang$rust$haskell$elixir$erlang$ruby$perl
[└─>](bold purple) """

# Timeout for commands (in milliseconds)
command_timeout = 500

# Right prompt (optional)
right_format = """$cmd_duration"""

[character]
success_symbol = "[➜](bold green)"
error_symbol = "[➜](bold red)"

[directory]
style = "bold cyan"
truncation_length = 3
truncate_to_repo = true
format = "[$path]($style)[$read_only]($read_only_style) "

[git_branch]
symbol = " "
style = "bold purple"
format = "on [$symbol$branch]($style) "

[git_status]
style = "bold yellow"
format = '([$all_status$ahead_behind]($style) )'
conflicted = "🏳"
ahead = "⇡${count}"
behind = "⇣${count}"
diverged = "⇕⇡${ahead_count}⇣${behind_count}"
up_to_date = "✓"
untracked = "?"
stashed = "$"
modified = "!"
staged = '[++\($count\)](green)'
renamed = "»"
deleted = "✘"

[git_state]
format = '[\($state( $progress_current of $progress_total)\)]($style) '
cherry_pick = "[🍒 PICKING](bold red)"

[cmd_duration]
min_time = 500
format = "took [$duration](bold yellow)"

# Language-specific modules
[python]
symbol = " "
style = "bold yellow"
format = 'via [$symbol$pyenv_prefix($version )(\($virtualenv\) )]($style)'

[nodejs]
symbol = " "
style = "bold green"
format = "via [$symbol($version )]($style)"

[deno]
symbol = "🦕 "
style = "bold cyan"
format = "via [$symbol($version )]($style)"

[bun]
symbol = "🍞 "
style = "bold red"
format = "via [$symbol($version )]($style)"

[golang]
symbol = " "
style = "bold cyan"
format = "via [$symbol($version )]($style)"

[rust]
symbol = " "
style = "bold red"
format = "via [$symbol($version )]($style)"

[haskell]
symbol = " "
style = "bold purple"
format = "via [$symbol($version )]($style)"

[elixir]
symbol = " "
style = "bold purple"
format = 'via [$symbol($version \(OTP $otp_version\) )]($style)'

[erlang]
symbol = " "
style = "bold red"
format = "via [$symbol($version )]($style)"

[ruby]
symbol = " "
style = "bold red"
format = "via [$symbol($version )]($style)"

[perl]
symbol = "🐪 "
style = "bold cyan"
format = "via [$symbol($version )]($style)"

# Package version
[package]
symbol = "📦 "
style = "bold cyan"
format = "via [$symbol$version]($style) "

# Container
[docker_context]
symbol = " "
style = "bold blue"
format = "via [$symbol$context]($style) "

# Memory usage
[memory_usage]
disabled = false
threshold = 75
symbol = " "
style = "bold dimmed red"
format = "via $symbol[$ram( | $swap)]($style) "

# Time
[time]
disabled = false
format = '🕙[\[ $time \]]($style) '
time_format = "%T"
style = "bold cyan"

# Username
[username]
style_user = "bold cyan"
style_root = "bold red"
format = "[$user]($style) "
disabled = false
show_always = false

# Hostname
[hostname]
ssh_only = true
format = "on [$hostname](bold red) "
disabled = false

# Line break
[line_break]
disabled = false

# Status
[status]
style = "bold red"
symbol = "✖"
format = '[\[$symbol $common_meaning$signal_name$maybe_int\]]($style) '
map_symbol = true
disabled = false
EOF

echo "Starship configuration created at ~/.config/starship.toml"
```

---

## Alternative: Minimal Configuration

If you prefer a simpler, more minimal look:

```bash
cat > ~/.config/starship.toml << 'EOF'
# Minimal Starship Configuration

add_newline = true

format = """
$directory\
$git_branch\
$git_status\
$python\
$nodejs\
$golang\
$rust\
$line_break\
$character"""

[character]
success_symbol = "[➜](bold green)"
error_symbol = "[➜](bold red)"

[directory]
style = "bold cyan"
truncation_length = 3
truncate_to_repo = true

[git_branch]
symbol = " "
style = "bold purple"

[git_status]
style = "bold yellow"

[python]
symbol = " "
style = "bold yellow"

[nodejs]
symbol = " "
style = "bold green"

[golang]
symbol = " "
style = "bold cyan"

[rust]
symbol = " "
style = "bold red"
EOF
```

---

## Customization Tips

### 1. Use a Preset

Starship comes with several presets. To use one:

```bash
# List available presets
starship preset --help

# Apply a preset (examples)
starship preset nerd-font-symbols -o ~/.config/starship.toml
starship preset bracketed-segments -o ~/.config/starship.toml
starship preset pure-preset -o ~/.config/starship.toml
```

### 2. Test Configuration Changes

```bash
# After editing ~/.config/starship.toml, reload your shell:
exec $SHELL

# Or test without reloading:
starship prompt
```

### 3. Add More Languages

To add support for other languages, check the [Starship documentation](https://starship.rs/config/) for available modules. Common ones include:

- `java`
- `kotlin`
- `lua`
- `php`
- `swift`
- `terraform`
- `vagrant`
- And many more!

### 4. Customize Colors

Starship supports various color formats:

- Named colors: `red`, `green`, `blue`, `cyan`, `yellow`, `purple`, `black`, `white`
- Hex colors: `#ff79c6` (Dracula pink), `#bd93f9` (Dracula purple)
- 256-color palette: `141`, `212`, etc.

Example with Dracula hex colors:

```toml
[git_branch]
style = "bold #bd93f9"  # Dracula purple

[directory]
style = "bold #8be9fd"  # Dracula cyan
```

---

## Integration with Your Setup

### Update Section 6.6 of the Main Guide

Replace the Powerlevel10k section (6.6) with:

```bash
### 6.6 Configure Starship Prompt

# Install Starship
pamac install starship

# Add to your .zshrc (this should be in your zsh config repo)
# Add this line near the end of your .zshrc:
# eval "$(starship init zsh)"

# Create Dracula-themed configuration
mkdir -p ~/.config
cat > ~/.config/starship.toml << 'EOF'
# ... (copy the Dracula configuration from above)
EOF

# Reload shell to activate
exec $SHELL

echo "NOTE: Add 'eval \"\$(starship init zsh)\"' to your .zshrc"
echo "Configuration file is at ~/.config/starship.toml"
```

---

## Verification

Test that Starship is working:

```bash
# Check version
starship --version

# Test prompt rendering
starship prompt

# Navigate to a git repository to see git integration
cd ~/Projects
mkdir test-repo && cd test-repo
git init
# You should see git branch info in your prompt

# Test language detection
cd ~/Projects/python
# You should see Python version in prompt

cd ~/Projects/go
# You should see Go version in prompt
```

---

## Troubleshooting

### Prompt not showing icons correctly

- Ensure Nerd Fonts are installed (you already have Hack Nerd Font from Phase 5)
- Verify your terminal is using the Nerd Font

### Prompt is slow

- Check `command_timeout` setting in starship.toml
- Disable modules you don't need
- Use `starship timings` to see which modules are slow

### Configuration not loading

- Check file location: `~/.config/starship.toml`
- Verify TOML syntax: `starship config`
- Check for errors: `starship explain`

---

## Resources

- **Official Documentation**: https://starship.rs/
- **Configuration Reference**: https://starship.rs/config/
- **Presets Gallery**: https://starship.rs/presets/
- **GitHub Repository**: https://github.com/starship/starship

---

## Comparison: Powerlevel10k vs Starship

|Feature|Powerlevel10k|Starship|
|---|---|---|
|**Maintenance**|Limited support|Active development|
|**Speed**|Very fast|Very fast|
|**Configuration**|Zsh-specific, wizard-based|TOML file, cross-shell|
|**Shell Support**|Zsh only|Zsh, Bash, Fish, PowerShell, etc.|
|**Customization**|Extensive|Extensive|
|**Setup**|Interactive wizard|File-based config|
|**Learning Curve**|Easy (wizard)|Medium (TOML syntax)|

---

## Summary

Starship provides a modern, actively maintained alternative to Powerlevel10k with these benefits:

✅ **Active development** - regular updates and new features ✅ **Cross-shell** - not locked into Zsh ✅ **Fast** - minimal performance impact ✅ **Flexible** - TOML configuration is powerful and portable ✅ **Beautiful** - great defaults with Nerd Font integration ✅ **Context-aware** - shows all your language versions automatically

The Dracula-themed configuration provided matches your existing color scheme and will integrate seamlessly with your development environment.