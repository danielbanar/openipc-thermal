# Thermal Throttle Service

This service dynamically adjusts the camera framerate based on CPU temperature to prevent overheating.  

It is intended for **OpenIPC cameras** and has only been tested on **Sigmastar SoCs**.

---

## Overview
- Reads CPU temperature from:  
  `/sys/devices/virtual/mstar/msys/TEMP_R`
- Adjusts camera framerate via:  
  `/proc/mi_modules/mi_sensor/mi_sensor0`
- Uses a **configuration file** to define temperature → framerate rules.

---

## Configuration

Configuration file path:  
```
/etc/temp_profile.conf
```

### Format
```
# <range> <fps> <scale> <update>
```

- **min-max** → Temperature range (°C)  
  - Example: `60-70`
- **fps** → Fixed FPS value (use `-` if not set)  
- **scale** → Fraction of base FPS (use `-` if not set)  
- **update** → Update interval in seconds

### Example
```conf
# <range> <fps> <scale> <update>
0-60       -     1       5
60-70      -     0.75    5
70-80      -     0.5     5
80-90      15    -       5
90-999     5     -       5
```

---

## Installation

This script is distributed as part of a custom package in the **OpenIPC firmware build system**.  

1. Clone and use my OpenIPC firmware repo that contains this package.  
2. Enable the package in your `defconfig`:  
   ```
   BR2_PACKAGE_THERMAL_THROTTLE_DB=y
   ```
3. Build the firmware and flash it to your camera.  

The service will then be available inside the firmware.

---
