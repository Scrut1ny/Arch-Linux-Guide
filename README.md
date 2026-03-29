## Essential Commands

<details>
<summary>System Update and Upgrade</summary>

#### Update package lists and upgrade all packages:
```bash
sudo pacman -Syyu --noconfirm
```

</details>


<details>
<summary>Package Management</summary>

#### Install a package:
```bash
sudo pacman -S <package-name>
```

#### Remove a package:
```bash
sudo pacman -R <package-name>
```

#### Remove a package, configuration files, and its dependencies:
```bash
sudo pacman -Rns <package-name>
```
- -R: Remove the package.
- -n: Remove the package's configuration files (if they aren't needed elsewhere).
- -s: Remove dependencies that were installed specifically for this package and are no longer needed by any other installed packages.

#### Search for a package:
```bash
pacman -Ss <package-name>
```

#### List installed packages:
```bash
pacman -Q
```

#### Display information about a package:
```bash
pacman -Qi <package-name>
```

</details>


<details>
<summary>System Maintenance</summary>

#### Clean the package cache:
```bash
sudo pacman -Sc --noconfirm
```

#### Remove all unused packages and dependencies:
```bash
sudo pacman -Rns $(pacman -Qdtq) --noconfirm
```

</details>


<details>
<summary>User Management</summary>

#### Add a new user:
```bash
sudo useradd -m <username>
```

#### Set a password for a user:
```bash
sudo passwd <username>
```

#### Delete a user:
```bash
sudo userdel -r <username>
```

</details>


## Networking

<details>
<summary>Basic Network Commands</summary>

#### List network interfaces:
```bash
ip link show
```

#### Show IP addresses and network details:
```bash
ip addr show
```

#### Test connectivity to a host:
```bash
ping <host>
```

#### Trace the path to a host:
```bash
traceroute <host>
```

</details>

<details>
<summary>Configuration and Management</summary>

#### Bring an interface up:
```bash
sudo ip link set <interface> up
```

#### Bring an interface down:
```bash
sudo ip link set <interface> down
```

#### Add an IP address to an interface:
```bash
sudo ip addr add <IP>/<prefix> dev <interface>
```

#### Remove an IP address from an interface:
```bash
sudo ip addr del <IP>/<prefix> dev <interface>
```

#### Check NetworkManager status:
```bash
systemctl status NetworkManager.service
```

#### Restart NetworkManager:
```bash
sudo systemctl restart NetworkManager.service
```

</details>

<details>
<summary>Setup Wi-Fi</summary>

#### Install Necessary Packages:
```bash
sudo pacman -S wpa_supplicant dialog --noconfirm
```

#### Restart NetworkManager:
```bash
sudo systemctl restart NetworkManager.service
```

</details>


</details>

<details>
<summary>Setup Bluetooth</summary>

- [Arch Wiki](https://wiki.archlinux.org/title/Bluetooth)

#### Install the required packages:
```bash
sudo pacman -S bluez bluez-utils --noconfirm
```

#### Enable and start the Bluetooth service:
```bash
sudo systemctl enable --now bluetooth.service
sudo systemctl restart bluetooth.service
```

#### Verify the service is running:
```bash
systemctl status bluetooth.service
```

#### Use bluetoothctl to manage Bluetooth devices:
```bash
bluetoothctl
```

#### Enable simultaneous output to multiple audio devices
```bash
pactl load-module module-combine-sink
```

#### Disable simultaneous output to multiple audio devices
```bash
pactl unload-module XXXXXXXXX
```

</details>

<details>
<summary>Automatic Mirrors</summary>

#### Install Package:
```bash
sudo pacman -S reflector --noconfirm
```

#### Generate and Save the Mirror List:
- [Country Codes Alpha-2 & Alpha-3](https://www.iban.com/country-codes)
```
sudo reflector --latest 10 --sort rate --protocol https --save /etc/pacman.d/mirrorlist
```

#### Update Package Databases:
```bash
sudo pacman -Syy
```

</details>

## Installing Software

<details>
<summary>macchanger</summary>

#### Install macchanger
```bash
sudo pacman -S macchanger --noconfirm
```

#### Spoof MAC Address of wlan0
```bash
sudo ip link set wlan0 down

sudo macchanger -r wlan0

sudo ip link set wlan0 up
```

#### Automated MAC address spoofing of active network interface
```bash
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

#### Install
```bash
sudo pacman -S mullvad-vpn --noconfirm
```

</details>


<details>
<summary>Tor Service</summary>

#### Install the Tor Service
```bash
sudo pacman -S tor --noconfirm
```

#### Enable and Start the Tor Service (Optional)
```bash
sudo systemctl enable tor.service
sudo systemctl start tor.service
```

#### Check the Service Status (Optional)
```bash
sudo systemctl status tor.service
```

#### Configure Tor (Optional)
```bash
sudo nano /etc/tor/torrc
```

#### Restart Tor (Optional)
```bash
sudo systemctl restart tor
```

</details>


<details>
<summary>Kitty (Terminal)</summary>

#### Install Kitty
```bash
sudo pacman -S kitty --noconfirm
```
#### Configure Kitty theme
```bash
kitty +kitten themes
```

</details>


<details>
<summary>Common Access Card (CAC) / Smartcard</summary>

#### 1. Install required packages
```bash
sudo pacman -Sy ccid opensc --noconfirm
```

#### 2. Enable & start the PC/SC Smart Card Daemon
```bash
sudo systemctl enable --now pcscd.socket
```

#### 3. Load security device
- Navigate to Settings > Privacy & Security > Security Devices and click "Load" to load a module using:
```bash
/usr/lib/opensc-pkcs11.so
```
![image](https://github.com/user-attachments/assets/fefb339c-7ff5-49f8-bfee-8e0908f2cbf1)

---

#### Automated CLI - Load security device
- Flatpak Install
```bash
modutil -dbdir "$HOME/.var/app/io.gitlab.librewolf-community/.librewolf/*/cert9.db" -add "CAC Module" -libfile "/usr/lib/opensc-pkcs11.so"
```
- System Install
```bash
modutil -dbdir "$HOME/.mozilla/firefox/*/cert9.db" -add "CAC Module" -libfile "/usr/lib/opensc-pkcs11.so"
```

#### List available PKCS #11 Modules
```bash
modutil -dbdir sql:.pki/nssdb/ -list
```

#### Add custom "CAC Module" to PKCS #11 Module
```bash
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
```bash
sudo curl -fsSL https://github.com/yt-dlp/yt-dlp/releases/latest/download/yt-dlp -o /usr/local/bin/yt-dlp && sudo chmod +x /usr/local/bin/yt-dlp
```

#### Install ffmpeg (optional)
```bash
sudo pacman -S ffmpeg --noconfirm
```

</details>


<details>
<summary>ollama</summary>

#### Install ollama
```bash
curl -fsSL https://ollama.com/install.sh | sh
```

#### Install any llama instance

- [https://ollama.com/](https://ollama.com/)
```bash
ollama run taozhiyuai/llama-3-8b-lexi-uncensored:f16
```

</details>


## Troubleshooting

<details>
<summary>KDE Plasma</summary>

#### Missing Touchpad Settings Page
```bash
kcmshell6 kcm_touchpad
```

#### Missing Brightness Adjustment Bar (laptop)
```bash
sudo pacman -S powerdevil --noconfirm
```
- *Now restart your DM for changes to take effect.*

</details>

<details>
<summary>Display Manager</summary>

#### Display Manager not loading
```bash
sudo systemctl restart display-manager
```

</details>

<details>
<summary>error: failed to synchronize all databases (unable to lock database)</summary>

#### Check for Running Processes:
- First, check if another instance of pacman or pamac is running.
```bash
ps aux | grep pacman
```
- If you see any processes, wait for them to complete or terminate them if you're sure they are stuck.

#### Remove Lock Files:
- If no other processes are running, you may need to remove the lock file manually. Run:
```bash
sudo rm /var/lib/pacman/db.lck
```

#### Update the System Again:
- After removing the lock file, try updating the package database again:
```bash
sudo pacman -Syu
```

</details>


## Miscellaneous

<details>
<summary>Screen Mirroring</summary>

#### Install xrander
```bash
sudo pacman -S xorg-xrandr --noconfirm
```

#### Check Connected Displays
```bash
xrandr
```

#### Auto-Detect and Enable HDMI Output
```bash
xrandr --output HDMI-1 --auto --same-as eDP-1
```
- HDMI-1: This is your projector. Replace it if your output is different.
- eDP-1: This usually represents your laptop's internal display. Check your xrandr output to confirm the correct identifier.

#### Adjust Display Settings (if necessary)
```bash
xrandr --output HDMI-1 --auto --same-as eDP-1 --mode 1920x1080
```

</details>

<details>
<summary>Partitioning</summary>

#### Tools:
- [gparted](https://archlinux.org/packages/extra/x86_64/gparted/)

#### Create a Manual Boot Entry (Windows):
```bash
sudo efibootmgr -c -d /dev/nvme0n1 -p 1 -L "Windows" -l "\EFI\Microsoft\Boot\bootmgfw.efi"
```

#### Create Systemd-boot Manual Boot Entry (Windows):
```bash
echo -e "title   Windows\nefi     /EFI/Microsoft/Boot/bootmgfw.efi" | sudo tee /boot/loader/entries/windows.conf > /dev/null
```
```bash
sudo bootctl update
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

```bash
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
