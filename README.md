# PlymouthThemes

Customized Plymouth boot‐splash themes for Ubuntu (tested on 22.04 LTS → 24.04).  
Includes the **Circle** animation (tweaked by Crashicorn) and any future additions.

---

## Quick-start (one-liner)

```bash
curl -sL https://raw.githubusercontent.com/Crashicorn/PlymouthThemes/main/install_circle.sh | bash
```

> **Heads-up:** the script checks for `quiet splash` in `/etc/default/grub`, installs the necessary packages, copies the theme, registers it, rebuilds the initramfs, and prompts you to reboot. Read it first if you want to see exactly what happens.

---

## Manual install

### 1. Clone this repo

```bash
git clone https://github.com/Crashicorn/PlymouthThemes.git ~/PlymouthThemes
cd ~/PlymouthThemes
```

### 2. Install Plymouth & plugins

```bash
sudo apt update
sudo apt install -y plymouth plymouth-label plymouth-themes
```

### 3. Ensure GRUB enables Plymouth

Open `/etc/default/grub` and confirm **`quiet splash`** is present:

```bash
grep GRUB_CMDLINE_LINUX_DEFAULT /etc/default/grub
```

If it isn’t, edit the file, add `quiet splash`, then run:

```bash
sudo update-grub
```

### 4. Copy the theme

```bash
sudo cp -r circle /usr/share/plymouth/themes/
```

### 5. Register & select it

```bash
sudo update-alternatives --install \
  /usr/share/plymouth/themes/default.plymouth default.plymouth \
  /usr/share/plymouth/themes/circle/circle.plymouth 100

sudo update-alternatives --set default.plymouth \
  /usr/share/plymouth/themes/circle/circle.plymouth
```

### 6. Rebuild the initramfs

```bash
sudo update-initramfs -u
```

### 7. Preview (optional)

```bash
sudo plymouthd
sudo plymouth --show-splash
sleep 5
sudo plymouth quit
```

### 8. Reboot 🚀

```bash
sudo reboot
```

---

## Troubleshooting

| Symptom | Fix |
|---------|-----|
| Boots to text / no splash | Verify `quiet splash` in `/etc/default/grub`; make sure proprietary GPU driver has KMS enabled. |
| Default spinner still shows | Re-run **step 5** to set the alternatives link, then **step 6** to rebuild the initramfs. |
| Warning: `label-pango.so missing` | `sudo apt install plymouth-label plymouth-themes` and rebuild. |
| Need to tweak speed / colors | Edit `circle/circle.script` (and PNG assets), then `sudo update-initramfs -u`. |
