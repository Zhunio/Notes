# How to Build Raspberry Pi Image

Build a reusable Raspberry Pi OS image with SSH, username, and password pre-configured. Flash it onto new SD cards and add WiFi credentials from your Mac before inserting into any new Pi.

**Device:** Raspberry Pi 5
**OS:** Raspberry Pi OS (64-bit) — Debian 13 Trixie

---

## Part 1: Build the Image (on a Raspberry Pi)

### 1. Clone and install

```bash
git clone https://github.com/raspberrypi/rpi-image-gen.git
cd rpi-image-gen
sudo ./install_deps.sh
```

### 2. Build the image

```bash
./rpi-image-gen build -c ./config/trixie-minbase.yaml -- \
  IGconf_device_user1pass="Herbs143!" \
  IGconf_ssh_pubkey_user1="$(cat ~/.ssh/pi.pub)"
```

### 3. Copy image to Mac

```bash
scp itsupport@raspberrypi:~/rpi-image-gen/work/image-deb13-arm64-min/deb13-arm64-min.img ~/Desktop/
```

---

## Part 2: Flash a New Pi (from Mac)

```bash
diskutil list                    # find SD card e.g. disk4
diskutil unmountDisk /dev/disk4
sudo dd if=~/Desktop/*.img of=/dev/rdisk4 bs=4m status=progress
```

Don't eject yet — continue to Part 3.

---

## Part 3: Add WiFi (from Mac)

After flashing, the SD card mounts automatically. Edit the file at `/Volumes/bootfs/network-config`:

```yaml
network:
  version: 2
  wifis:
    wlan0:
      dhcp4: true
      access-points:
        "YourNetworkName":
          password: "YourWiFiPassword"
```

Eject:

```bash
diskutil eject /dev/disk4
```

Insert SD card into new Pi and power on. It connects to WiFi automatically on first boot.
