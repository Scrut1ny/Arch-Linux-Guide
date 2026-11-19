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
#### File Transfer
- *“Failed to start org.bluez.obex service: The name is not activatable”*
```bash
sudo pacman -S bluez-obex --noconfirm
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
module_id=$(pactl load-module module-combine-sink)
```

#### Disable simultaneous output to multiple audio devices
```bash
pactl unload-module "$module_id"
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

## Software

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


## Troubleshooting

<details>
<summary>Windows overwrites Linux EFI Boot entry - Fix</summary>

1. Boot into live Linux enviroment
- [Download `archlinux-XXXX.XX.XX-x86_64.iso`](https://archlinux.org/download/)
- [Download `Ventoy` (*optional*)](https://github.com/ventoy/Ventoy)

2. Recreate missing NVRAM entry
```
efibootmgr --create --disk /dev/nvmeXnX --part 1 --label "Linux Boot Manager" --loader '\EFI\systemd\systemd-bootx64.efi'
```
- *Note: Set `/dev/nvmeXnX` to your ROOT partition, NOT your EFI.*

3. Reorder NVRAM entries
```
efibootmgr -o XXXX,YYYY
```

- Note: `BootXXXX` entries are located and accessable via `/sys/firmware/efi/efivars`.

</details>

<details>
<summary>Missing Windows `EFI System Partition` - Fix</summary>

- NOTE: Make sure to change the disk # if needed!

```cmd
(echo select volume C&echo shrink desired=100&echo select disk 0&echo create partition efi size=100&echo format quick fs=fat32 label=ESP&echo assign letter=S&echo exit) | diskpart && bcdboot C:\Windows /s S: /f UEFI
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
