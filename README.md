#### System Update and Upgrade
<details>
<summary>Expand...</summary>

### Update package lists and upgrade all packages:
```
sudo pacman -Syu
```

</details>


#### Package Management
<details>
<summary>Expand...</summary>

### Install a package:
```
sudo pacman -S <package-name>
```

### Remove a package:
```
sudo pacman -R <package-name>
```

### Remove a package and its dependencies:
```
sudo pacman -Rns <package-name>
```

### Search for a package:
```
pacman -Ss <package-name>
```

### List installed packages:
```
pacman -Q
```

### Display information about a package:
```
pacman -Qi <package-name>
```

</details>


#### System Maintenance
<details>
<summary>Expand...</summary>

#### Clean the package cache:
```
sudo pacman -Sc
```

#### Remove all unused packages and dependencies:
```
sudo pacman -Rns $(pacman -Qdtq)
```

</details>


#### Install Mullvad VPN
<details>
<summary>Expand...</summary>

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
