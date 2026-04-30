# Filesystem and Persistence

## Overview

This document explains which directories and files on the ARI robot are persistent across reboots, which should be backed up, and how to change non-persistent configurations.

## Persistent Directories

| Dir location        | Description                                      | Make copy | Justification                                      |
|---------------------|--------------------------------------------------|-----------|----------------------------------------------------|
| /media/root-ro      | Main filesystem (read-only)                      | N         | Unique data removed on reinstall                   |
| /boot/efi           | Bootloader filesystem                            | N         | Same for each install                              |
| /etc/calibration    | PAL-specific calibrations and user settings      | Y         | Needed for robot operation, unique since delivery   |
| /home               | User home dir, includes .ros and calibration     | Y         | Apps use calibration data from here                |
| /var/log            | Log files                                        | N~        | Usually not unique, too large to copy              |

**Also back up `/etc/fstab`** (contains boot mounting and settings).

## Checking Mounts and Partitions

Use the following commands to inspect disk partitions and mounts:

```bash
lsblk
mount
df
```

output as seen on robot:
```bash
pal@ari-20c:~$ df
Filesystem     1K-blocks     Used Available Use% Mounted on
udev             7847008        0   7847008   0% /dev
tmpfs            1574044     1856   1572188   1% /run
/dev/nvme0n1p3 146658236 16825092 122310484  13% /media/root-ro
tmpfs-root       7870212    24892   7845320   1% /media/root-rw
overlayroot      7870212    24892   7845320   1% /
tmpfs            7870212     8192   7862020   1% /dev/shm
tmpfs               5120        4      5116   1% /run/lock
tmpfs            7870212        0   7870212   0% /sys/fs/cgroup
/dev/nvme0n1p2    122990     8810    114180   8% /boot/efi
/dev/nvme0n1p1     58085      189     53310   1% /etc/calibration
/dev/nvme0n1p5 266329820  9510416 243217412   4% /home
/dev/nvme0n1p6  76270372  6587712  65762584  10% /var/log
tmpfs            1574040        8   1574032   1% /run/user/1000
```

## Changing Non-Persistent Configurations

To edit configuration files (e.g., PID settings):

1. go into admin mode:
   ```bash
   su
   ```

2. Remount filesystem as read-write and enter it:
   ```bash
   sudo rw
   chroot /ro
   ```
3. Edit the file or install pkg (using `vi` if `nano` is not available):
   ```bash
   sudo vi /opt/pal/gallium/share/ari_controller_configuration/config/pids_v2.yaml
   ```
4. when done exit the filesystem:
   ```bash
   exit
   ```
5. Remount as read-only:
   ```bash
   sudo ro
   ```
6. then exit admin mode:
   ```bash
   exit
   ```

## Troubleshooting
- If unable to remount `/ro` as read-only, ensure no processes are using it.
- Always back up important configuration files before making changes. 
