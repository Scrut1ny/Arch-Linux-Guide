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
systemctl status NetworkManager
```

#### Restart NetworkManager:
```
sudo systemctl restart NetworkManager
```

</details>

<details>
<summary>Setup Wi-Fi</summary>

#### Install Necessary Packages
```
sudo pacman -S iw wpa_supplicant dialog --noconfirm
```

#### Restart NetworkManager
```
sudo systemctl restart NetworkManager
```

</details>


## Installing Software

<details>
<summary>Mullvad VPN</summary>

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

# Navigate out of the repository directory
cd ..

# Clean up by removing the repository directory
rm -rf mullvad-vpn-bin/
```

</details>


<details>
<summary>Kitty (Terminal)</summary>

#### Install Kitty
```
sudo pacman -S kitty
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
#### Load security device
- Navigate to Settings > Privacy & Security > Security Devices and click "Load" to load a module using:
```
/usr/lib/opensc-pkcs11.so
```
```
modutil -dbdir "$HOME/.var/app/io.gitlab.librewolf-community/.librewolf/*.default/cert9.db" -add "CAC Module" -libfile "/usr/lib/opensc-pkcs11.so"
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
sudo pacman -S ffmpeg
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


## Personal Commands

<details>
<summary>Install all Apps</summary>

```
sudo pacman -Syu # Update package lists and upgrade all packages

sudo pacman -Sc # Clean the package cache
sudo pacman -Rns $(pacman -Qdtq) # Remove all unused packages and dependencies

kitty +kitten themes "Everforest Dark Hard"

flapak install -y io.gitlab.librewolf-community
flapak install -y org.keepassxc.KeePassXC
flapak install -y io.freetubeapp.FreeTube
flapak install -y org.flameshot.Flameshot
flapak install -y com.atlauncher.ATLauncher
flapak install -y org.signal.Signal
flapak install -y com.github.tchx84.Flatseal


```

</details>
