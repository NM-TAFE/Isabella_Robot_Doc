# Upgrade Guide: ARI 23.12 → 25.01

Step-by-step instructions for upgrading Isabella from ARI OS 23.12 to 25.01.

## Prerequisites

✅ Complete [Pre-Upgrade Checklist](./pre-upgrade-checklist.md) before proceeding.

## Step 1: Obtain Upgrade Materials

### Download from PAL Robotics

1. **Access PAL Download Portal**
   - Contact PAL Robotics support or access your account
   - Request: ARI 25.01 OS image and upgrade tools
   - You should receive:
     - ISO disk image (`ari-25.01-*.iso`)
     - Build docker script (`<user-id>-build-docker.sh`)
     - Credentials/keys folder

2. **Verify Files**
   ```bash
   # Check file sizes and dates
   ls -lh ari-25.01-*.iso
   ls -lh *-build-docker.sh
   
   # Verify checksums (if provided by PAL)
   sha256sum -c ari-25.01-checksums.txt
   ```

3. **Store Safely**
   - Keep original files on external drive
   - Create working copies for upgrade
   - Document file paths and checksums

## Step 2: Prepare Bootable USB

### Requirements

- USB drive: 8GB+ (USB 3.0 recommended)
- Linux machine or tool like Etcher (cross-platform)
- 15-20 minutes

### Create USB Bootable Image

**Option A: Using `dd` (Linux)**

```bash
# Find USB device
lsblk
# Should show something like /dev/sdb or /dev/sdc

# Unmount if mounted
sudo umount /media/user/USBDRIVE

# Write ISO (CAUTION: specify correct device!)
sudo dd if=ari-25.01-*.iso of=/dev/sdX bs=4M status=progress
sudo sync

# Verify write
sudo eject /dev/sdX
```

**Option B: Using Etcher (GUI, all platforms)**

- Download: https://www.balena.io/etcher/
- Insert USB drive
- Select ISO file
- Select USB device
- Click Flash
- Wait for completion

### Verify USB

```bash
# Re-insert USB and verify files
lsblk -o NAME,SIZE,TYPE
# Should show USB device

# Mount and check
sudo mkdir -p /mnt/usb-check
sudo mount /dev/sdXX /mnt/usb-check
ls -la /mnt/usb-check/
sudo umount /mnt/usb-check
```

## Step 3: Prepare Robot for Upgrade

### Robot Setup

1. **Ensure Robot is Powered Off**
   - Shut down gracefully: `sudo poweroff`
   - Wait 30 seconds for complete shutdown
   - Disconnect power cable

2. **Connect USB to Robot**
   - Insert USB bootable drive into robot's USB port
   - Use USB 3.0 port if available (faster)
   - Secure cable to prevent disconnection

3. **Prepare for Boot**
   - Keep monitor and keyboard ready
   - Or prepare for SSH access after boot
   - Note: First boot may take 10-15 minutes

## Step 4: Boot from USB and Install

### Boot Process

1. **Power On Robot**
   - Connect power cable
   - Press power button
   - Robot should boot from USB

2. **Monitor Boot Sequence**
   - Watch for PAL boot logo
   - System may show installation prompts
   - **DO NOT interrupt if process begins**

3. **Follow On-Screen Prompts**
   - Accept license terms (if shown)
   - Confirm installation target (should be main drive)
   - Confirm data will be erased (this is expected)
   - Allow installation to proceed (typically 30-60 minutes)

### Typical Installation Timeline

| Phase | Duration | Status |
|-------|----------|--------|
| BIOS/Boot | 1-2 min | Checking hardware |
| Installer Load | 1-2 min | Loading installation files |
| Pre-installation Checks | 1-2 min | Verifying hardware |
| System Installation | 20-40 min | Writing files to disk |
| Package Installation | 10-30 min | Installing software |
| Configuration | 5-10 min | Setting up system |
| First Boot | 5-10 min | Starting new OS |
| **Total** | **45-90 min** | ⏱️ |

### Monitor Installation

**Option A: Local Console**
- Connect monitor via HDMI
- Connect keyboard via USB
- Watch progress on screen

**Option B: Remote SSH (after ~10 minutes)**
```bash
# After installation begins, try SSH
ssh ari@ari-<id>
# Should eventually work once system boots
```

## Step 5: Post-Installation Configuration

### First Boot After Upgrade

The system will boot into new ARI 25.01 OS. You may see:

1. **Welcome Screen** - PAL robot setup wizard
2. **Language Selection** - Choose your language
3. **Network Setup** - Configure WiFi
4. **System Setup** - Set hostname, admin PIN, etc.

### Complete Initial Setup

Follow on-screen prompts to:
- [ ] Select language
- [ ] Accept license terms
- [ ] Configure network (WiFi or Ethernet)
- [ ] Set admin password
- [ ] Set robot name
- [ ] Confirm timezone

### Verify System Booted

```bash
# SSH into robot (from development machine)
ssh ari@ari-<id>

# Check OS version
cat /etc/os-release
# Should show: Ubuntu 22.04 or 24.04 with ARI 25.01

# Check ROS installation
ros2 --version
# Should show: ROS 2 Humble

# Check system is healthy
ros2 topic list
# Should list robot topics
```

## Step 6: Remove USB and Shutdown

### Safe Removal

```bash
# From robot console/SSH
sudo eject /dev/sdb  # Or appropriate USB device
# Or physically remove USB

# Shut down gracefully
sudo shutdown -h now
```

### Power Down

- Robot will shut down
- Wait 30 seconds
- Disconnect power cable (optional, depends on your setup)

## Step 7: First Boot with Upgraded OS

### Power On

- Connect power cable
- Press power button
- Wait for boot (3-5 minutes typical)
- First boot may be slower (optimizing)

### Verify Upgrade Successful

```bash
# SSH to robot
ssh ari@ari-<id>

# Verify OS version
cat /etc/os-release
# Should show ARI 25.01

# Verify ROS 2
ros2 --version
# Should show ROS 2 Humble

# Check basic subsystems
ros2 topic list | head -20

# Access Web Dashboard
# Open browser: https://ari-<id>
# Login with admin credentials
```

## Troubleshooting During Upgrade

### USB Not Booting

**Symptom:** Robot boots from internal drive instead of USB

**Solutions:**
- Verify USB is bootable: `isohybrid ari-25.01-*.iso`
- Try different USB port (preferably USB 3.0)
- Try different USB drive
- Check BIOS boot order (may need F2/DEL during startup)

### Installation Hangs

**Symptom:** Installation stuck at certain percentage or step

**Solutions:**
- Wait 10-15 minutes (sometimes just slow)
- If still stuck: force shutdown (hold power 10 seconds)
- Remove USB, boot from internal drive
- Retry from backup recovery using recovery USB

### Network Not Available After Boot

**Symptom:** Cannot SSH or ping robot after upgrade

**Solutions:**
- Check WiFi is configured (see initial setup screen)
- Verify network name and password
- Try Ethernet cable if available
- Use local console to troubleshoot

## Next Steps

After successful upgrade and first boot:

1. **Proceed to [Post-Upgrade Validation](./post-upgrade-validation.md)**
   - Run diagnostics
   - Test all Isabella features
   - Verify functionality

2. **Review [Component Updates](./component-updates.md)**
   - Check Isabella customizations
   - Update any affected code

3. **Monitor System**
   - Watch diagnostics for 24 hours
   - Monitor temperature and resource usage
   - Report any issues to PAL support

---

**Estimated Total Time:** 2-4 hours  
**Point of No Return:** When installation starts  
**Recovery Option:** Restore from backup or PAL support  

## Support & References

- **PAL Robotics Upgrade Docs:** https://docs.pal-robotics.com/25.01/management/
- **PAL Robot Management:** https://docs.pal-robotics.com/25.01/management/
- **ROS 2 Humble Docs:** https://docs.ros.org/en/humble/
- **Technical Support:** https://docs.pal-robotics.com/25.01/general/getting-support.html
