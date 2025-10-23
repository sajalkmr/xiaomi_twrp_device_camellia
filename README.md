# TWRP Device Tree for Xiaomi/Redmi/POCO (camellia)


> **Forked from:** [OrangeFoxRecovery/device_xiaomi_camellia](https://github.com/OrangeFoxRecovery/device_xiaomi_camellia)  
> **Repository:** [sajalkmr/xiaomi_twrp_device_camellia](https://github.com/sajalkmr/xiaomi_twrp_device_camellia)

**Compatible Devices:**
- 📱 **Redmi Note 10 5G** (Global/India)
- 📱 **Redmi Note 11SE** (India) 
- 📱 **Redmi Note 10T 5G** (India)
- 📱 **POCO M3 Pro 5G** (Global)


## Device Information

| Device | Xiaomi/Redmi/POCO Devices |
| --- | --- |
| **Device Names** | • Redmi Note 10 5G<br>• Redmi Note 11SE<br>• Redmi Note 10T 5G<br>• POCO M3 Pro 5G |
| **Codename** | camellia |
| **Model Numbers** | M2103K19C, M2103K19G, M2103K19PG, M2103K19PI |
| **Chipset** | MediaTek MT6833 (Dimensity 700) |
| **Architecture** | ARM64 |
| **Android Version** | Android 11+ |
| **TWRP Version** | 3.7.x |

## Features

🎯 **Device Compatibility:** Works on all Xiaomi/Redmi/POCO devices with "camellia" codename

✅ **Working Features:**
- [x] ADB & MTP
- [x] Storage (Internal/External)
- [x] USB OTG
- [x] Vibration
- [x] Brightness Control
- [x] A/B Partition Support
- [x] Decryption Support
- [x] Backup & Restore
- [x] Flash ZIP files
- [x] Reboot options


## Requirements

- **Android Build Environment** (Android 11+)
- **TWRP Minimal Manifest**
- **4GB+ RAM** for building
- **100GB+ Storage** for sources

## Building TWRP

### 1. Initialize Build Environment

```bash
# Install dependencies (Ubuntu/Debian)
sudo apt update
sudo apt install bc bison build-essential ccache curl flex g++-multilib gcc-multilib git gnupg gperf imagemagick lib32ncurses5-dev lib32readline-dev lib32z1-dev liblz4-tool libncurses5 libncurses5-dev libsdl1.2-dev libssl-dev libxml2 libxml2-utils lzop pngcrush rsync schedtool squashfs-tools xsltproc zip zlib1g-dev

# Create working directory
mkdir ~/twrp && cd ~/twrp
```

### 2. Initialize Repo

```bash
# Initialize TWRP minimal manifest
repo init --depth=1 -u https://github.com/minimal-manifest-twrp/platform_manifest_twrp_aosp.git -b twrp-12.1

# Sync sources
repo sync -c -j$(nproc --all) --force-sync --no-clone-bundle --no-tags
```

### 3. Clone Device Tree

```bash
# Clone this device tree
git clone https://github.com/sajalkmr/xiaomi_twrp_device_camellia.git device/xiaomi/camellia
```

### 4. Build TWRP

```bash
# Set up environment
source build/envsetup.sh

# Choose build target
lunch omni_camellia-eng

# Build recovery
make recoveryimage -j$(nproc)
```

## Installation


### Method 1: Fastboot (Recommended)

```bash
# Boot to fastboot mode
adb reboot bootloader

# Flash TWRP
fastboot flash recovery out/target/product/camellia/recovery.img

# Boot to recovery
fastboot reboot recovery
```

### Method 2: SP Flash Tool
1. Download SP Flash Tool
2. Load scatter file from device
3. Select recovery.img file
4. Flash recovery partition

## File Structure

```
xiaomi_twrp_device_camellia/
├── Android.mk              # Android makefile
├── AndroidProducts.mk      # Product configuration
├── BoardConfig.mk          # Board configuration (Main TWRP configs)
├── device.mk              # Device-specific packages
├── omni_camellia.mk       # Device inheritance
├── recovery.fstab         # Partition mount points
├── vendorsetup.sh         # Build targets
├── prebuilt/              # Prebuilt kernel & DTB
│   ├── dtb.img
│   └── Image.gz
└── recovery/              # Recovery-specific files
    └── root/              # Recovery ramdisk files
        ├── init.recovery.mt6833.rc
        ├── load_vendor_modules.rc
        └── [various .rc files]
```

## Partition Layout

| Partition | Type | Description |
| --- | --- | --- |
| **system** | logical | System partition |
| **vendor** | logical | Vendor partition |
| **product** | logical | Product partition |
| **boot** | physical | Boot partition |
| **recovery** | physical | Recovery partition |
| **userdata** | physical | User data (/data) |

## Known Issues & Solutions

### Issue: Touch not working in recovery

### Issue: Decryption not working
**Solution:** Ensure proper vendor files are present in `/vendor` partition

### Issue: MTP not working
**Solution:** Try different USB cables and enable USB debugging before booting recovery


## Disclaimer

- **Use at your own risk!** Flashing custom recovery may void your warranty
- Always backup your original recovery before flashing
- This is for educational and development purposes
- We are not responsible for bricked devices

