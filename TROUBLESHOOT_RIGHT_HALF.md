# Troubleshooting Right Half Not Working

## Steps to Fix Right Half Issues

### 1. **Clear Bonds and Re-pair (Most Important!)**
The most common issue is corrupted pairing information:

```bash
# Flash settings_reset to BOTH halves first
# Then flash regular firmware to both
# Then reset both simultaneously
```

**Process:**
1. Flash `settings_reset` firmware to **both halves**
2. Flash regular `split_dos_left` to left half
3. Flash regular `split_dos_right` to right half  
4. **Power off both halves** (unplug USB/battery)
5. **Power on both halves** (or plug USB to left)
6. Press **reset buttons simultaneously** on both halves
7. Wait 30-60 seconds for pairing
8. Test right half keys

### 2. **Verify Right Half Has Power**
- Right half MUST have power (battery or USB)
- LED on right half should light up if it has one
- If battery-powered, try USB power to test

### 3. **Check Right Half is Actually Flashed**
- Connect right half via USB
- Check if it shows up: `lsusb | grep SplitDos`
- If you see two "SplitDos" devices, both halves are connected
- The right half should appear even when not paired (as separate device)

### 4. **Test Right Half Independently**
- Connect right half directly to USB
- Flash right half firmware
- It should appear as keyboard device
- Press keys - do ANY keys work? (Even wrong characters?)

### 5. **Hardware Check**
- Verify right half wiring matches left half
- Check diodes are oriented correctly
- Verify all row/column connections
- Test continuity with multimeter if possible

### 6. **LED/Status Indicators**
- Does right half have status LED?
- Does it blink/flash when powered?
- Different blink patterns indicate connection status

### 7. **Serial Logs (if USB serial works after rebuild)**
```bash
# Connect left half via USB
screen /dev/ttyACM0 115200

# Look for:
# - "split: peripheral connected"
# - "split: ble connected"
# - Any error messages
```

## Quick Validation Test

**Right now:**
1. Unplug both halves
2. Plug ONLY right half to USB
3. Does it appear in `lsusb`? → Hardware works
4. Does it show as keyboard? → Firmware works
5. Press keys - any response? → Matrix works

**Then:**
1. Unplug right half
2. Plug left half to USB
3. Wait 10 seconds
4. Power on right half (battery or separate USB)
5. Press reset on BOTH simultaneously
6. Wait 30 seconds
7. Test right half keys

## Expected Behavior

- **Left half (central)**: Always works, connects to computer
- **Right half (peripheral)**: Should connect to left via BLE after pairing
- **Pairing**: Takes 10-60 seconds after simultaneous reset
- **If paired**: Right half keys should work immediately
- **If not paired**: Right half does nothing, left half works alone

