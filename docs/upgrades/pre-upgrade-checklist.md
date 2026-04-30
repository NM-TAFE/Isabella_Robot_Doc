# Pre-Upgrade Checklist

Complete this checklist before upgrading from ARI OS 23.12 to 25.01.

## System Preparation

### Backups

- [ ] **Full System Backup**
  - Back up entire robot filesystem
  - Use: `sudo tar -czf ari-backup-23.12.tar.gz /home /etc`
  - Store on external drive or network location
  - Verify backup integrity: `tar -tzf ari-backup-23.12.tar.gz | head`

- [ ] **ISO Recovery Image**
  - Obtain ISO image from PAL Robotics (shipped with purchase)
  - Store on external drive
  - Verify checksum matches PAL documentation

- [ ] **Custom Code Backup**
  - Back up all custom ROS packages: `git clone --mirror <repo> backup/`
  - Back up Isabella configurations
  - Back up Docker build scripts
  - Back up any local modifications to robot software

- [ ] **Network Configuration Backup**
  - Document current WiFi networks
  - Document static IP configurations
  - Document custom DNS settings
  - Document VPN configurations (if used)
  - File: `/etc/netplan/` configurations

- [ ] **Docker Environment Backup**
  - Export current Docker images: `docker save -o backup-images.tar <image-names>`
  - Back up docker-compose files
  - Back up build scripts and Dockerfiles

### Hardware & Network Verification

- [ ] **Network Connectivity**
  - Ping robot: `ping ari-<id>`
  - SSH connection works: `ssh ari@ari-<id>`
  - Network latency acceptable (< 50ms typical)
  - WiFi signal strength adequate (RSSI > -70 dBm)

- [ ] **Robot Charging**
  - Fully charge battery before upgrade
  - Verify charging indicator shows full
  - Robot will be powered down during upgrade

- [ ] **Hardware Health Check**
  - Access Web UI: `https://ari-<id>`
  - Open Diagnostics panel
  - Verify all green (or expected states)
  - Note any warnings or errors

- [ ] **Subsystems Status**
  - Motors respond to commands
  - Cameras stream video
  - LiDAR returns distance data
  - Microphone array functions
  - Speakers output audio

### Software Requirements

- [ ] **Development Environment**
  - Docker installed and tested: `docker run hello-world`
  - Docker compose available: `docker-compose --version`
  - Sufficient disk space (> 50GB free)
  - Ubuntu 22.04 or 24.04 (development machine)

- [ ] **PAL Tools**
  - Download official upgrade tools from PAL
  - Verify digital signatures if provided
  - Extract to known location
  - Check permissions are correct

- [ ] **SSH Keys**
  - Verify SSH access to robot works
  - Test: `ssh ari@ari-<id> "echo OK"`
  - Default credentials working or documented

### Isabella-Specific Checks

- [ ] **Live Mode Verification**
  - Live Mode starts successfully: `npm start` → "Start Live Mode"
  - WebSocket connection working
  - LLM model loads
  - STT server accessible
  - TTS endpoint responsive
  - Document current configuration

- [ ] **Custom ROS Nodes**
  - All custom nodes compile: `colcon build`
  - All nodes start without errors
  - All topic subscriptions work
  - All services callable
  - Document node list: `ros2 node list`

- [ ] **Social Perception**
  - Person detection working
  - Face recognition enabled/disabled (as configured)
  - Performance metrics noted
  - Document current settings

- [ ] **LLM Integration**
  - Ollama running and responsive
  - Models loaded and tested
  - Network connectivity to external LLM (if used)
  - Test inference: `ollama run llama2 "test prompt"`

- [ ] **Custom Integrations**
  - Any external service integrations working
  - API credentials/tokens verified
  - Network endpoints accessible
  - List all dependencies

## Pre-Upgrade Shutdown

### 12 Hours Before Upgrade

- [ ] **Announce Maintenance Window**
  - Notify team of upgrade time
  - Document estimated downtime (2-4 hours)
  - Disable any external monitoring/automation

- [ ] **Graceful Shutdown**
  - Stop all running applications
  - Stop Live Mode and custom services
  - Shut down robot cleanly: `sudo poweroff`
  - Wait for shutdown to complete
  - Power down robot completely

### Create Recovery USB

- [ ] **Bootable USB Creation**
  - Prepare USB drive (8GB+ recommended)
  - Follow PAL instructions to create bootable USB
  - Test USB boot (optional, only if have spare robot)
  - Label USB with "ARI 25.01 Recovery - [date]"
  - Store in safe location

## Documentation

### Pre-Upgrade Records

- [ ] **Capture System State**
  ```bash
  # Log current system information
  uname -a > pre-upgrade-system-info.txt
  ros2 --version >> pre-upgrade-system-info.txt
  docker --version >> pre-upgrade-system-info.txt
  ```

- [ ] **Document Configuration**
  ```bash
  # Back up ROS configuration
  tar -czf pre-upgrade-ros-config.tar.gz ~/.ros ~/.config
  
  # Back up network configuration
  tar -czf pre-upgrade-network-config.tar.gz /etc/netplan
  ```

- [ ] **Create Upgrade Log Template**
  - File: `upgrade-log-25.01.txt`
  - Template sections:
    - Pre-upgrade state
    - Upgrade start time
    - Major steps and timestamps
    - Any errors or issues
    - Upgrade completion time
    - Post-upgrade observations

## Verification Checklist Summary

| Item | Status | Notes |
|------|--------|-------|
| Full system backup | ☐ | Path: __________ |
| ISO recovery image | ☐ | Verified: ☐ |
| Custom code backup | ☐ | Count: __________ |
| Network config backup | ☐ | Files backed up |
| Docker backup | ☐ | Images saved |
| Network connectivity | ☐ | Latency: __________ |
| Robot fully charged | ☐ | Status verified |
| Hardware diagnostics | ☐ | All green: ☐ |
| Live Mode working | ☐ | Tested: __________ |
| ROS nodes verified | ☐ | Count: __________ |
| Custom services tested | ☐ | All working: ☐ |
| PAL tools downloaded | ☐ | Version: __________ |
| Team notified | ☐ | Downtime OK: ☐ |
| Robot powered down | ☐ | Time: __________ |
| Recovery USB created | ☐ | Tested: ☐ |

## Next Step

Once all items are checked, proceed to [Upgrade Guide](./upgrade-guide.md).

---

**Estimated Time:** 1-2 hours  
**Point of No Return:** When backup USB is created and robot is powered off  
**Last Abort Point:** Before booting new ISO image
