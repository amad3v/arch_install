# Arch Linux Installation Notes

## List of packages

The can be found in the [packages](docs/packages) file or a [script](docs/packages.sh) that can be executed.

## Foreign packages

### Oh My Zsh!

```bash
sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```

#### Plugins

- `sudo`: Add `sudo` plugins in `.zshrc`
- Syntax Highlighting:
```bash
echo "source /usr/share/zsh/plugins/zsh-syntax-highlighting/zsh-syntax-highlighting.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
```
- Autosuggestions:
```bash
echo "source /usr/share/zsh/plugins/zsh-autosuggestions/zsh-autosuggestions.zsh" >> ${ZDOTDIR:-$HOME}/.zshrc
```
### Shell Prompt

Comment `ZSH_THEME` in `$HOME/.zshrc`.

```bash
echo 'eval "$(starship init zsh)"' >> ${ZDOTDIR:-$HOME}/.zshrc
starship preset no-nerd-font -o ~/.config/starship.toml
```
### AUR Helper

```bash
cd /tmp
git clone https://aur.archlinux.org/paru-bin.git
cd paru-bin
makepkg -si
```

### AUR Packages
```bash
paru -S brave-beta-bin google-chrome-dev pamac-aur volantes-cursors-git woeusb vscodium-bin vscodium-bin-features drawio
```

## Services

### SDDM

```bash
systemctl enable sddm
```

### Network Manager

```bash
systemctl enable NetworkManager
```

### QEMU

```bash
systemctl enable libvirtd.socket
systemctl enable virtlockd
```

### Pipewire Pulse

This to replace `pulseaudio` and `pulseaudio-bluetooth`

```bash
systemctl enable pipewire-pulse
```

## Aliases

Accessible from this [file](docs/alias). Copy it to `$HOME`.

```bash
echo 'source "$HOME/.alias"' >> ${ZDOTDIR:-$HOME}/.zshrc
```

## Git/GitHub

### Setup Git

```bash
git config --global user.name <USERNAME>
git config --global user.email <EMAIL>
git config --global core.editor vim
git config --global init.defaultBranch main
```

### Setup SSH

```bash
ssh-keygen -t ed25519 -C <EMAIL>
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

### Add SSH to GitHub

Copy the content of `~/.ssh/id_ed25519.pub`

In GitHub, go to `Settings` > `Access` > `SSH and GPG keys`.

Click `New SSH key`.

Add a `Title`.

Select the type of key: `authentication`.

In the `Key` field, paste the public key.

Click `Add SSH key`.

## Applications

### Dolphin

Setup of the toolbar layout by copying the [config](docs/dolphinui.rc) file to `$HOME/.local/share/kxmlgui5/dolphin`.

Make sure `$HOME/.local/share/kxmlgui5/dolphin/` exists:

```bash
mkdir -p $HOME/.local/share/kxmlgui5/dolphin/
```

### Kate

Setup of the toolbar layout by copying the [config](docs/katepart5ui.rc) file to `$HOME/.local/share/kxmlgui5/katepart/`.

Make sure `$HOME/.local/share/kxmlgui5/katepart/` exists:

```bash
mkdir -p $HOME/.local/share/kxmlgui5/katepart/
```

## Misc

### Passless `sudo`

To avoid typing the password for `sudo`, delete files from `/etc/sudoers.d/`.

```bash
sudo rm -rf /etc/sudoers.d/*
```

### visudo

Run `sudo visudo` (vim must be installed):

```bash
VISUAL=$(which vim) sudo visudo
```

Add:

```
<USER_NAME> ALL=(ALL:ALL) NOPASSWD: ALL

# Set default EDITOR to restricted version of vim, and do not allow visudo to use EDITOR/VISUAL.
Defaults editor=/usr/bin/vim, !env_editor
```

### Local `bin`

```bash
mkdir "$HOME/.local/bin"

# Update .zshrc
echo 'export PATH=$PATH:"$HOME/.local/bin"' >> ${ZDOTDIR:-$HOME}/.zshrc
```

### Uniform cursors

Open `/usr/share/icons/default/index.theme`

Change `Inherits=Adwaita` to `Inherits=<cursor's name>`
