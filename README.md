# Setting Up Neovim in Windows Subsystem for Linux (WSL)

## 1. Install Neovim

**Update package manager first:**
```bash
sudo apt update
sudo apt upgrade -y
```

**Install Neovim:**
```bash
sudo apt install neovim -y
```

**Verify installation:**
```bash
nvim --version
```

## 2. Clone your dotfiles repository

```bash
cd ~
git clone https://github.com/LukeElrod/nvim.git ~/.config/nvim
cd ~/.config/nvim
```

## 3. Install submodules (including neovim-java-config)

```bash
git submodule update --init --recursive
```

## 4. Install required dependencies

**Essential tools:**
```bash
sudo apt install -y \
  build-essential \
  curl \
  wget \
  git \
  ripgrep \
  fd-find \
  unzip
```

**Node.js (for LSP and plugins):**
```bash
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install nodejs -y
```

**Python (for Python LSP and plugins):**
```bash
sudo apt install python3-pip python3-venv -y
pip3 install --user pynvim
```

## 5. Install a Nerd Font (optional but recommended)

Download a Nerd Font and install it in Windows (not WSL), then set it in your terminal settings:
- Download from: https://www.nerdfonts.com/
- Extract and install the `.ttf` files
- Set in Windows Terminal settings

## 6. Set up lazy.nvim (if using it)

Your `init.lua` should auto-bootstrap lazy.nvim. On first launch:

```bash
nvim
```

This will automatically:
- Download lazy.nvim
- Install all plugins
- Run setup for each plugin

## 7. Verify Java setup (for neovim-java-config)

Install Java if you need Java LSP:

```bash
sudo apt install default-jdk -y
java -version
```

## 8. Fix Git SSL issues in WSL (if you still have them)

```bash
# For WSL, use the Windows certificate store
git config --global http.sslCAinfo /etc/ssl/certs/ca-certificates.crt

# Alternatively, disable verification temporarily (not recommended)
git config --global http.sslVerify false
```

## 9. Launch Neovim

```bash
nvim
```

On first launch, lazy.nvim will install all plugins. This may take a minute or two.

## Troubleshooting

**If plugins don't install:**
```bash
# Clear plugin cache and reinstall
rm -rf ~/.local/share/nvim/lazy
nvim  # Re-run to reinstall
```

**If you get permission errors:**
```bash
# Check your .config directory permissions
chmod -R 755 ~/.config/nvim
```

**If LSPs don't attach:**
```bash
# Check your LSP setup in Neovim
:LspInfo
```

**Access your WSL files from Windows:**
```
\\wsl$\Ubuntu\home\<username>\.config\nvim
```

(Replace "Ubuntu" with your WSL distro name if different)

---

## Bonus: Git Submodule Management

### Remove old submodule and add a new one

```bash
# Remove from staging
git rm --cached neovim-java-config

# Remove the submodule entry from .gitmodules
git config --file .gitmodules --remove-section submodule.neovim-java-config

# Stage the .gitmodules change
git add .gitmodules

# Delete the actual directory
rm -rf neovim-java-config

# Commit the removal
git commit -m "Remove old neovim-java-config submodule"

# Add the new submodule
git submodule add <new-repo-url> <new-directory-name>

# Commit the new submodule
git add .gitmodules <new-directory-name>
git commit -m "Add new submodule"
```

### Verify submodule status

```bash
# Check .gitmodules
cat .gitmodules

# Check the submodule status
git submodule status
```

## SSL Certificate Issues

If you encounter SSL certificate errors with Git:

**Point Git to system certificates:**
```bash
git config --global http.sslCAinfo /etc/ssl/certs/ca-certificates.crt
```

**Update system certificates (if needed):**
```bash
sudo apt update
sudo apt install ca-certificates
```

**Verify configuration:**
```bash
git config --global http.sslCAinfo
```
