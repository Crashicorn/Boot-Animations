# Custom Boot Animations

Customized Plymouth boot‑splash themes for Ubuntu (22.04 → 25.04).  
Currently contains Crashicorn’s tweaked **Circle** animation plus room for more.

---

## One-Line Installation:

```bash
git clone --depth 1 https://github.com/Crashicorn/PlymouthThemes.git ~/PlymouthThemes && cd ~/PlymouthThemes && sudo ./install.sh
```


## Or for Manual Installation, copy‑paste this into your terminal:

```bash
# Clone the repo (depth 1 keeps it small)
git clone --depth 1 https://github.com/Crashicorn/PlymouthThemes.git ~/PlymouthThemes
cd ~/PlymouthThemes

# 1) Install the Plymouth packages Ubuntu actually provides
sudo apt update
sudo apt install -y plymouth plymouth-label plymouth-themes plymouth-x11

# 2) Copy the Circle theme into the system directory
sudo rm -rf /usr/share/plymouth/themes/circle
sudo cp -r pack_1/circle /usr/share/plymouth/themes/

# 3) Register & select “circle”
sudo update-alternatives --install \
  /usr/share/plymouth/themes/default.plymouth default.plymouth \
  /usr/share/plymouth/themes/circle/circle.plymouth 100
sudo update-alternatives --set default.plymouth \
  /usr/share/plymouth/themes/circle/circle.plymouth

# 4) Convert all frames to RGBA, mapping black (#000000) to full transparency
cd /usr/share/plymouth/themes/circle
sudo mogrify -alpha on -transparent '#000000' -type TrueColorAlpha progress-*.png

# 5) Rebuild initramfs
sudo update-initramfs -u

# 6) Update GRUB
# MANUALLY CHANGE THIS LINE if it already contains info between "")
sudo sed -i 's|^GRUB_CMDLINE_LINUX_DEFAULT=.*|GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"|' /etc/default/grub
sudo update-grub

# 7) Reboot for changes to take effect
sudo reboot
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
