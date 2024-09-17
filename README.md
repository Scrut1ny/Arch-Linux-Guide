## Essential Commands

<details>
<summary>System Update and Upgrade</summary>

#### Update package lists and upgrade all packages:
```
sudo pacman -Syu --noconfirm
```

</details>


<details>
<summary>Package Management</summary>

#### Install a package:
```
sudo pacman -S <package-name>
```

#### Remove a package:
```
sudo pacman -R <package-name>
```

#### Remove a package and its dependencies:
```
sudo pacman -Rns <package-name>
```

#### Search for a package:
```
pacman -Ss <package-name>
```

#### List installed packages:
```
pacman -Q
```

#### Display information about a package:
```
pacman -Qi <package-name>
```

</details>


<details>
<summary>System Maintenance</summary>

#### Clean the package cache:
```
sudo pacman -Sc --noconfirm
```

#### Remove all unused packages and dependencies:
```
sudo pacman -Rns $(pacman -Qdtq) --noconfirm
```

</details>


<details>
<summary>User Management</summary>

#### Add a new user:
```
sudo useradd -m <username>
```

#### Set a password for a user:
```
sudo passwd <username>
```

#### Delete a user:
```
sudo userdel -r <username>
```

</details>


## Networking

<details>
<summary>Basic Network Commands</summary>

#### List network interfaces:
```
ip link show
```

#### Show IP addresses and network details:
```
ip addr show
```

#### Test connectivity to a host:
```
ping <host>
```

#### Trace the path to a host:
```
traceroute <host>
```

</details>

<details>
<summary>Configuration and Management</summary>

#### Bring an interface up:
```
sudo ip link set <interface> up
```

#### Bring an interface down:
```
sudo ip link set <interface> down
```

#### Add an IP address to an interface:
```
sudo ip addr add <IP>/<prefix> dev <interface>
```

#### Remove an IP address from an interface:
```
sudo ip addr del <IP>/<prefix> dev <interface>
```

#### Check NetworkManager status:
```
systemctl status NetworkManager.service
```

#### Restart NetworkManager:
```
sudo systemctl restart NetworkManager.service
```

</details>

<details>
<summary>Setup Wi-Fi</summary>

#### Install Necessary Packages:
```
sudo pacman -S iw wpa_supplicant dialog --noconfirm
```

#### Restart NetworkManager:
```
sudo systemctl restart NetworkManager.service
```

</details>


</details>

<details>
<summary>Setup Bluetooth</summary>

- [Arch Wiki](https://wiki.archlinux.org/title/Bluetooth)

#### Install the required packages:
```
sudo pacman -S bluez bluez-utils --noconfirm
```

#### Start and enable the Bluetooth service:
```
sudo systemctl start bluetooth.service
sudo systemctl enable bluetooth.service
```

#### Verify the service is running:
```
systemctl status bluetooth.service
```

#### Use bluetoothctl to manage Bluetooth devices:
```
bluetoothctl
```

</details>


## Installing Software

<details>
<summary>Mullvad VPN</summary>

- Configure System Build Enviroment
```
sudo nano /etc/makepkg.conf
mkdir -p ~/build/{packages,sources,srcpackages}
```

- Install
```
# Clone the Mullvad VPN binary repository
git clone https://aur.archlinux.org/mullvad-vpn-bin.git

# Navigate into the repository directory
cd mullvad-vpn-bin/

# Download the Mullvad code signing key
wget https://mullvad.net/media/mullvad-code-signing.asc

# Import the Mullvad code signing key into GPG
gpg --import mullvad-code-signing.asc

# Verify the fingerprint of the Mullvad signing key
gpg --fingerprint admin@mullvad.net

# Set the build directory and build the package
BUILDDIR=/tmp/makepkg makepkg -sirc

# Clean up by removing the repository directory
cd .. && rm -rf mullvad-vpn-bin/
```

</details>


<details>
<summary>Kitty (Terminal)</summary>

#### Install Kitty
```
sudo pacman -S kitty --noconfirm
```
#### Configure Kitty theme
```
kitty +kitten themes
```

</details>


<details>
<summary>Common Access Card (CAC) / Smartcard</summary>

- [Common Access Card](https://wiki.archlinux.org/title/Common_Access_Card)

#### Install required packages
```
sudo pacman -Sy ccid opensc --noconfirm
```

#### Start & Enable the PC/SC Smart Card Daemon
```
sudo systemctl start pcscd
sudo systemctl enable pcscd
```

#### Load security device
- Navigate to Settings > Privacy & Security > Security Devices and click "Load" to load a module using:
```
/usr/lib/opensc-pkcs11.so
```
- Flatpak Install
```
modutil -dbdir "$HOME/.var/app/io.gitlab.librewolf-community/.librewolf/*/cert9.db" -add "CAC Module" -libfile "/usr/lib/opensc-pkcs11.so"
```
- System Install
```
modutil -dbdir "$HOME/.mozilla/firefox/*/cert9.db" -add "CAC Module" -libfile "/usr/lib/opensc-pkcs11.so"
```

</details>


<details>
<summary>yt-dlp & ffmpeg</summary>

#### Install yt-dlp
```
sudo curl -fsSL https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp
sudo chmod +x /usr/local/bin/yt-dlp
```
#### Install ffmpeg
```
sudo pacman -S ffmpeg --noconfirm
```

</details>


<details>
<summary>ollama</summary>

#### Install ollama
```
curl -fsSL https://ollama.com/install.sh | sh
```

#### Install any llama instance

- [https://ollama.com/](https://ollama.com/)
```
ollama run taozhiyuai/llama-3-8b-lexi-uncensored:f16
```

</details>


## Troubleshooting

<details>
<summary>KDE Plasma</summary>

#### Missing Touchpad Settings Page
```
kcmshell6 kcm_touchpad
```

</details>

<details>
<summary>Display Manager</summary>

#### Display Manager Frozen

- Quick Way
```
for dm in gdm sddm lightdm xdm; do sudo systemctl restart ${dm}.service 2>/dev/null; done
```

- Fancy Way
```
#!/bin/bash

# Check for the active display manager and set the appropriate variable
for dm in gdm sddm lightdm xdm; do
    if systemctl is-active --quiet "${dm}.service"; then
        active_dm="${dm}"
        break
    fi
done

# Check if an active display manager was found
if [ -z "$active_dm" ]; then
    echo "No display manager is currently active."
    exit 1
fi

# Restart the detected display manager
echo "Restarting ${active_dm} service..."
systemctl restart "${active_dm}.service"
```

</details>


## Personal Commands

<details>
<summary>Install all Apps</summary>

```
sudo pacman -Syu --noconfirm # Update package lists and upgrade all packages

sudo pacman -Sc --noconfirm # Clean the package cache
sudo pacman -Rns $(pacman -Qdtq) --noconfirm # Remove all unused packages and dependencies

kitty +kitten themes "Everforest Dark Hard"

flatpak install -y io.gitlab.librewolf-community
flatpak install -y org.keepassxc.KeePassXC
flatpak install -y io.freetubeapp.FreeTube
flatpak install -y org.flameshot.Flameshot
flatpak install -y com.atlauncher.ATLauncher
flatpak install -y org.signal.Signal
flatpak install -y com.github.tchx84.Flatseal


```

</details>
