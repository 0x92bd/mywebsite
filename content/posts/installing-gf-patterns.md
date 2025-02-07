---
date: '2025-01-19T23:19:45+06:00'
draft: false
title: 'Installing Gf Patterns on Kali Linux'
author: 'rakib'
tags:
    - linux
    - gf_patterns
    - recon
image:
description:
toc: true
---

The `waybackurls` and `gf` are excellent for bug bounty hunters and pentesters. This script will automate their installation and configuration, ensuring a smooth setup experience. Below is a step-by-step guide, including a script to handle everything for you.

---

## Prerequisites

Before you proceed, ensure that:

1. **Go** is installed on your system. If not, you can install it by following [Go's official documentation](https://golang.org/doc/install).
2. You know whether you’re using Bash or Zsh as your shell.

---

## The Script

The following script will:

1. Check if you’re using Bash or Zsh and configure completion scripts accordingly.
2. Ensure that your `GOPATH` is set.
3. Install the necessary tools (`waybackurls` and `gf`).
4. Set up GF patterns.

Save this script as `setup_gf_patterns.sh` and run it to automate the process.

```bash
#!/bin/bash

# Function to check if a command exists
command_exists() {
    command -v "$1" &>/dev/null
}

# Check if Go is installed
if ! command_exists go; then
    echo "Go is not installed. Please install Go and re-run this script."
    exit 1
fi

# Detect shell type
SHELL_TYPE="$(basename $SHELL)"
echo "Detected shell: $SHELL_TYPE"

# Set GOPATH if not set
if [ -z "$GOPATH" ]; then
    export GOPATH="$HOME/go"
    echo "GOPATH is not set. Using default: $GOPATH"
    if [ "$SHELL_TYPE" = "bash" ]; then
        echo 'export GOPATH="$HOME/go"' >> "$HOME/.bashrc"
    elif [ "$SHELL_TYPE" = "zsh" ]; then
        echo 'export GOPATH="$HOME/go"' >> "$HOME/.zshrc"
    fi
else
    echo "GOPATH is set to: $GOPATH"
fi

# Ensure GOPATH/bin is in PATH
if [[ ":$PATH:" != *":$GOPATH/bin:"* ]]; then
    export PATH="$PATH:$GOPATH/bin"
    if [ "$SHELL_TYPE" = "bash" ]; then
        echo 'export PATH="$PATH:$GOPATH/bin"' >> "$HOME/.bashrc"
    elif [ "$SHELL_TYPE" = "zsh" ]; then
        echo 'export PATH="$PATH:$GOPATH/bin"' >> "$HOME/.zshrc"
    fi
fi

# Install waybackurls and gf
echo "Installing waybackurls and gf..."
go install github.com/tomnomnom/waybackurls@latest
if [ $? -ne 0 ]; then
    echo "Failed to install waybackurls. Exiting."
    exit 1
fi
go install github.com/tomnomnom/gf@latest
if [ $? -ne 0 ]; then
    echo "Failed to install gf. Exiting."
    exit 1
fi

# Configure GF completion
if [ "$SHELL_TYPE" = "bash" ]; then
    COMPLETION_SCRIPT="$GOPATH/pkg/mod/github.com/tomnomnom/gf*/gf-completion.bash"
    if [ -f "$COMPLETION_SCRIPT" ]; then
        echo "source $COMPLETION_SCRIPT" >> "$HOME/.bashrc"
        echo "GF completion for Bash added to .bashrc."
    else
        echo "GF completion script for Bash not found."
    fi
elif [ "$SHELL_TYPE" = "zsh" ]; then
    COMPLETION_SCRIPT="$GOPATH/pkg/mod/github.com/tomnomnom/gf*/gf-completion.zsh"
    if [ -f "$COMPLETION_SCRIPT" ]; then
        echo "source $COMPLETION_SCRIPT" >> "$HOME/.zshrc"
        echo "GF completion for Zsh added to .zshrc."
    else
        echo "GF completion script for Zsh not found."
    fi
else
    echo "Unsupported shell: $SHELL_TYPE"
    exit 1
fi

# Set up GF patterns
echo "Setting up GF patterns..."
mkdir -p "$HOME/.gf"
TEMP_DIR=$(mktemp -d)
cd "$TEMP_DIR" || exit

git clone https://github.com/1ndianl33t/Gf-Patterns
if [ $? -ne 0 ]; then
    echo "Failed to clone GF patterns repository. Exiting."
    exit 1
fi

mv ./Gf-Patterns/*.json "$HOME/.gf/"
if [ $? -eq 0 ]; then
    echo "GF patterns successfully set up in $HOME/.gf."
else
    echo "Failed to move GF patterns to $HOME/.gf."
    exit 1
fi

# Clean up
rm -rf "$TEMP_DIR"
echo "Temporary files cleaned up."

# Reload shell configuration
if [ "$SHELL_TYPE" = "bash" ]; then
    source "$HOME/.bashrc"
elif [ "$SHELL_TYPE" = "zsh" ]; then
    source "$HOME/.zshrc"
fi

echo "Setup complete! You can now use waybackurls and gf."
```

---

## How to Use the Script
1. Save the script to a file:
```bash
vim setup_gf_patterns.sh
```
2. Make the script executable:
```bash
chmod +x setup_gf_patterns.sh
```
3. Run the script:
```bash
./setup_gf_patterns.sh
```

---

## Verifying the Setup

Once the script completes, verify the installation:

- **Waybackurls**:
```bash
waybackurls -h
```

- **GF**:
```bash
gf -h
```

- **GF Patterns**:
```bash
ls ~/.gf
```

---

## Conclusion

With this script, setting up `waybackurls`, `gf`, and GF patterns becomes a breeze. Whether you're a pentester or bug bounty hunter, these tools are now ready to use. Happy hacking!

