# ARI OS 25.01 Upgrade Overview

## Current State

Isabella is currently running on **ARI OS 23.12** (PAL Robotics SDK version `pal-sdk-23.12`).

## Target State

Upgrade to **ARI OS 25.01** with latest PAL Robotics features and improvements.

## What's New in ARI 25.01

### Key Improvements

- **ROS 2 Humble** - Continued support and improvements
- **Web User Interface** - Enhanced diagnostics panel
- **Docker Development** - Streamlined developer workflow with official Docker images
- **System Recovery** - ISO image available for factory reset
- **Hardware Support** - Expanded sensors and capabilities
- **Performance** - Various optimizations

### New Features by Category

| Category | 23.12 | 25.01 | Notes |
|----------|-------|-------|-------|
| **Navigation** | ✅ | ✅ Enhanced | Improved path planning |
| **Gestures & Motions** | ✅ | ✅ Enhanced | Additional motion primitives |
| **Social Perception** | ✅ | ✅ Enhanced | Better person tracking |
| **Communication** | ✅ | ✅ Enhanced | Speech improvements |
| **Interaction** | ✅ | ✅ Enhanced | Better interaction flows |
| **Web Dashboard** | Limited | ✅ Full | Comprehensive diagnostics |
| **Simulation** | ✅ | ✅ | Gazebo/RViz support |

## Impact on Isabella Customizations

### Affected Components

#### ✅ Live Mode (STT → LLM → TTS)
- **Impact:** Minor
- **Status:** Compatible with ROS 2 Humble
- **Action:** No code changes required, re-test after upgrade

#### ✅ LLM Server & Ollama Integration
- **Impact:** Minimal
- **Status:** Ollama runs independently, not affected by OS
- **Action:** Verify network connectivity post-upgrade

#### ✅ Social Perception
- **Impact:** Potentially improved
- **Status:** OS 25.01 has enhanced person tracking
- **Action:** Test and benchmark improvements

#### ✅ ROS Nodes & Services
- **Impact:** Low
- **Status:** ROS 2 Humble compatibility maintained
- **Action:** Verify topic subscriptions and publishers

#### ✅ Hardware Integration
- **Impact:** Low to Medium
- **Status:** Same hardware (depth camera, LiDAR, Respeaker)
- **Action:** Re-run hardware diagnostics

### Component Compatibility Matrix

```
Component              | Compatibility | Action Required
-----------------------|---------------|------------------
Live Mode              | ✅ Compatible | Test only
Ollama/LLM             | ✅ Compatible | Network verification
Social Perception      | ✅ Compatible | Re-test (improved)
ROS Nodes              | ✅ Compatible | Verify connections
Hardware (Cameras)     | ✅ Compatible | Run diagnostics
Hardware (LiDAR)       | ✅ Compatible | Run diagnostics
Hardware (Respeaker)   | ✅ Compatible | Run diagnostics
Docker Environment     | ✅ Compatible | Rebuild image
Network Configuration  | ✅ Compatible | Re-configure if needed
```

## Upgrade Path

### Recommended Approach

1. **Backup** - Full system backup before upgrade
2. **Test** - Upgrade non-production instance first (if available)
3. **Upgrade** - Follow [Upgrade Guide](./upgrade-guide.md)
4. **Validate** - Run [Post-Upgrade Validation](./post-upgrade-validation.md)
5. **Monitor** - Watch diagnostics for 24 hours

### Timeline

- **Pre-Upgrade Prep:** 30-60 minutes
- **Upgrade Process:** 45-90 minutes
- **Post-Upgrade Testing:** 1-2 hours
- **Total:** 2-4 hours

## Official PAL Documentation

- **ARI 25.01 Manual:** https://docs.pal-robotics.com/25.01/ari.html
- **ARI 23.12 Manual:** https://docs.pal-robotics.com/sdk/23.12/
- **Upgrade Documentation:** https://docs.pal-robotics.com/25.01/management/

## Known Issues & Considerations

### From PAL Robotics

- **Diagnostics Bug:** Higher-level diagnostics may show errors, but lower-level diagnostics will appear green. This is a known PAL issue (see [GitHub issue](https://github.com/ros/diagnostics/issues/297)).
- **Node Discovery:** First ROS 2 discovery can take up to 1 minute after boot.

### For Isabella

- Custom ROS nodes may require recompilation
- Docker image rebuild required for development environment
- Network configuration should be re-verified

## Quick Checklist

Before upgrading, verify:
- [ ] Full system backup exists
- [ ] All Isabella features documented and tested
- [ ] Network configuration documented
- [ ] Custom ROS packages backed up
- [ ] Team notified of upgrade window
- [ ] Post-upgrade testing plan ready

## Next Steps

1. Review [Pre-Upgrade Checklist](./pre-upgrade-checklist.md)
2. Follow [Upgrade Guide](./upgrade-guide.md)
3. Complete [Post-Upgrade Validation](./post-upgrade-validation.md)
4. Review [Component Updates](./component-updates.md) for Isabella specifics

## Support

- **PAL Support:** https://docs.pal-robotics.com/25.01/general/getting-support.html
- **Isabella Docs:** See related documentation pages
- **Troubleshooting:** Check [Post-Upgrade Validation](./post-upgrade-validation.md) troubleshooting section
