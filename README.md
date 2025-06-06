## Essential Commands

<details>
<summary>System Update and Upgrade</summary>

#### Update package lists and upgrade all packages:
```
sudo pacman -Syyu --noconfirm
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

#### Remove a package, configuration files, and its dependencies:
```
sudo pacman -Rns <package-name>
```
- -R: Remove the package.
- -n: Remove the package's configuration files (if they aren't needed elsewhere).
- -s: Remove dependencies that were installed specifically for this package and are no longer needed by any other installed packages.

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
sudo pacman -S wpa_supplicant dialog --noconfirm
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

#### Enable and start the Bluetooth service:
```
sudo systemctl enable --now bluetooth.service
sudo systemctl restart bluetooth.service
```

#### Verify the service is running:
```
systemctl status bluetooth.service
```

#### Use bluetoothctl to manage Bluetooth devices:
```
bluetoothctl
```

#### Enable simultaneous output to multiple audio devices
```
pactl load-module module-combine-sink
```

#### Disable simultaneous output to multiple audio devices
```
pactl unload-module XXXXXXXXX
```

</details>

<details>
<summary>Automatic Mirrors</summary>

#### Install Package:
```
sudo pacman -S reflector --noconfirm
```

#### Generate and Save the Mirror List:
- [Country Codes Alpha-2 & Alpha-3](https://www.iban.com/country-codes)
```
sudo reflector --latest 10 --sort rate --protocol https --save /etc/pacman.d/mirrorlist
```

#### Update Package Databases:
```
sudo pacman -Syy
```

</details>

## Installing Software

<details>
<summary>macchanger</summary>

#### Install macchanger
```
sudo pacman -S macchanger --noconfirm
```

#### Spoof MAC Address of wlan0
```
sudo ip link set wlan0 down

sudo macchanger -r wlan0

sudo ip link set wlan0 up
```

#### Automated MAC address spoofing of active network interface
```
# Set the variable for the active network interface
INTERFACE=$(ip link show | awk '/state UP/ {print $2}' | sed 's/:$//')

# Now use the variable in your commands
sudo ip link set "$INTERFACE" down
sudo macchanger -r "$INTERFACE"
sudo ip link set "$INTERFACE" up
```

</details>



<details>
<summary>Mullvad VPN</summary>

#### Configure System Build Enviroment
```
sudo nano /etc/makepkg.conf
```
![config](https://github.com/user-attachments/assets/2cae9a80-db70-452a-ad00-5a863d42bdc3)

```
mkdir -p ~/build/{packages,sources,srcpackages,makepkglogs}
```

#### Install
```
# Clone into the Mullvad VPN binary repository
git clone https://aur.archlinux.org/mullvad-vpn-bin.git && cd mullvad-vpn-bin/

# Download the Mullvad code signing key
wget https://mullvad.net/media/mullvad-code-signing.asc

# Import the Mullvad code signing key into GPG
gpg --import mullvad-code-signing.asc

# Verify the fingerprint of the Mullvad signing key
gpg --fingerprint admin@mullvad.net

# Set the build directory and build the package
BUILDDIR=/tmp/makepkg makepkg -sirc

# Clean up by removing the repository directory
cd .. && rm -rf mullvad-vpn-bin/ ~/build/{packages,sources}/*
```

</details>


<details>
<summary>Tor Service</summary>

#### Install the Tor Service
```
sudo pacman -S tor --noconfirm
```

#### Enable and Start the Tor Service (Optional)
```
sudo systemctl enable tor.service
sudo systemctl start tor.service
```

#### Check the Service Status (Optional)
```
sudo systemctl status tor.service
```

#### Configure Tor (Optional)
```
sudo nano /etc/tor/torrc
```

#### Restart Tor (Optional)
```
sudo systemctl restart tor
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

#### 1. Install required packages
```
sudo pacman -Sy ccid opensc --noconfirm
```

#### 2. Enable & start the PC/SC Smart Card Daemon
```
sudo systemctl enable --now pcscd.socket
```

#### 3. Load security device
- Navigate to Settings > Privacy & Security > Security Devices and click "Load" to load a module using:
```
/usr/lib/opensc-pkcs11.so
```
![image](https://github.com/user-attachments/assets/fefb339c-7ff5-49f8-bfee-8e0908f2cbf1)

---

#### Automated CLI - Load security device
- Flatpak Install
```
modutil -dbdir "$HOME/.var/app/io.gitlab.librewolf-community/.librewolf/*/cert9.db" -add "CAC Module" -libfile "/usr/lib/opensc-pkcs11.so"
```
- System Install
```
modutil -dbdir "$HOME/.mozilla/firefox/*/cert9.db" -add "CAC Module" -libfile "/usr/lib/opensc-pkcs11.so"
```

#### List available PKCS #11 Modules
```
modutil -dbdir sql:.pki/nssdb/ -list
```

#### Add custom "CAC Module" to PKCS #11 Module
```
modutil -dbdir sql:.pki/nssdb/ -add "CAC Module" -libfile /usr/lib/opensc-pkcs11.so
```

#### References:
- [Common Access Card](https://wiki.archlinux.org/title/Common_Access_Card)
- [militarycac.com](https://militarycac.com/linux.htm)
- [dod-cac-ubuntu-linuxmint](https://cubiclenate.com/linux/applications/utilities/dod-cac-ubuntu-linuxmint/)
- [cac-scripts](https://github.com/csmig/cac-scripts)
- [linux_cac](https://github.com/jdjaxon/linux_cac)

</details>


<details>
<summary>yt-dlp & ffmpeg</summary>

#### Install yt-dlp
```
sudo curl -fsSL https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp && sudo chmod +x /usr/local/bin/yt-dlp
```

#### Install ffmpeg (optional)
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

#### Missing Brightness Adjustment Bar (laptop)
```
sudo pacman -S powerdevil --noconfirm
```
- *Now restart your DM for changes to take effect.*

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

<details>
<summary>error: failed to synchronize all databases (unable to lock database)</summary>

#### Check for Running Processes:
- First, check if another instance of pacman or pamac is running.
```
ps aux | grep pacman
```
- If you see any processes, wait for them to complete or terminate them if you're sure they are stuck.

#### Remove Lock Files:
- If no other processes are running, you may need to remove the lock file manually. Run:
```
sudo rm /var/lib/pacman/db.lck
```

#### Update the System Again:
- After removing the lock file, try updating the package database again:
```
sudo pacman -Syu
```

</details>


## Miscellaneous

<details>
<summary>Screen Mirroring</summary>

#### Install xrander
```
sudo pacman -S xorg-xrandr --noconfirm
```

#### Check Connected Displays
```
xrandr
```

#### Auto-Detect and Enable HDMI Output
```
xrandr --output HDMI-1 --auto --same-as eDP-1
```
- HDMI-1: This is your projector. Replace it if your output is different.
- eDP-1: This usually represents your laptop's internal display. Check your xrandr output to confirm the correct identifier.

#### Adjust Display Settings (if necessary)
```
xrandr --output HDMI-1 --auto --same-as eDP-1 --mode 1920x1080
```

</details>

<details>
<summary>Partitioning</summary>

#### Tools:
- [gparted](https://archlinux.org/packages/extra/x86_64/gparted/)

#### Create a Manual Boot Entry (Windows):
```
sudo efibootmgr -c -d /dev/nvme0n1 -p 1 -L "Windows" -l "\EFI\Microsoft\Boot\bootmgfw.efi"
```

#### Correct Boot Loader reference:
```
Available Boot Loaders on ESP:
          ESP: /boot (/dev/disk/by-partuuid/80b88c64-c32b-46a3-887f-8c2944f2d03a)
         File: ├─/EFI/systemd/systemd-bootx64.efi (systemd-boot 257.5-1-arch)
               └─/EFI/BOOT/BOOTX64.EFI (systemd-boot 257.5-1-arch)

Boot Loaders Listed in EFI Variables:
        Title: Linux Boot Manager
           ID: 0x0001
       Status: active, boot-order
    Partition: /dev/disk/by-partuuid/80b88c64-c32b-46a3-887f-8c2944f2d03a
         File: └─/EFI/systemd/systemd-bootx64.efi

        Title: Windows Boot Manager
           ID: 0x0002
       Status: active, boot-order
    Partition: /dev/disk/by-partuuid/80b88c64-c32b-46a3-887f-8c2944f2d03a
         File: └─/EFI/Microsoft/Boot/bootmgfw.efi
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
flatpak install -y org.libreoffice.LibreOffice
flatpak install -y org.keepassxc.KeePassXC
flatpak install -y io.freetubeapp.FreeTube
flatpak install -y org.flameshot.Flameshot
flatpak install -y com.atlauncher.ATLauncher
flatpak install -y org.signal.Signal
flatpak install -y com.github.tchx84.Flatseal
```

</details>
