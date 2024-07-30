#### Install Mullvad VPN
<details>
<summary>Expand...</summary>

```
git clone https://aur.archlinux.org/mullvad-vpn-bin.git \
  cd mullvad-vpn-bin/ \
  wget https://mullvad.net/media/mullvad-code-signing.asc \
  gpg --import mullvad-code-signing.asc \
  gpg --fingerprint admin@mullvad.net \
  BUILDDIR=/tmp/makepkg makepkg -sirc
  cd .. \
  rm -rf mullvad-vpn-bin/
```

</details>
