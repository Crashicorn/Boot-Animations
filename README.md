# PlymouthThemes

Customized Plymouth boot‑splash themes for Ubuntu (22.04 → 25.04).  
Currently contains Crashicorn’s tweaked **Circle** animation plus room for more.

---

## Installation (manual, copy‑paste)

```bash
# Clone the repo (depth 1 keeps it small)
git clone --depth 1 https://github.com/Crashicorn/PlymouthThemes.git ~/PlymouthThemes
cd ~/PlymouthThemes

# 1) Install the Plymouth packages Ubuntu actually provides
sudo apt update
sudo apt install -y plymouth plymouth-label plymouth-themes

# 2) Copy the Circle theme into the system directory
sudo rm -rf /usr/share/plymouth/themes/circle
sudo cp -r circle /usr/share/plymouth/themes/

# 3) Register & select “circle”
sudo update-alternatives --install \
  /usr/share/plymouth/themes/default.plymouth default.plymouth \
  /usr/share/plymouth/themes/circle/circle.plymouth 100
sudo update-alternatives --set default.plymouth \
  /usr/share/plymouth/themes/circle/circle.plymouth

# 4) Rebuild initramfs
sudo update-initramfs -u

# 5) Preview (optional) then reboot
sudo plymouthd && sudo plymouth --show-splash; sleep 5; sudo plymouth quit
sudo reboot
```

### Check GRUB once (only if you don’t already see a splash)

Make sure `/etc/default/grub` contains **quiet splash**:

```bash
grep GRUB_CMDLINE_LINUX_DEFAULT /etc/default/grub
# If missing:
# sudo sed -i 's|^GRUB_CMDLINE_LINUX_DEFAULT=.*|GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"|' /etc/default/grub
# sudo update-grub
```

---

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| Boots to text / no splash | Confirm `quiet splash` is in `/etc/default/grub`; proprietary GPU driver must support KMS. |
| Still shows Ubuntu spinner | Redo **step 3** then **step 4**. |
| `label-pango.so missing` warning | `sudo apt install plymouth-label plymouth-themes` then rebuild. |
| Want to tweak speed / color | Edit `circle/circle.script` and PNGs, then `sudo update-initramfs -u`. |

---