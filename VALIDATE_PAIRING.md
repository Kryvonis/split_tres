# How to Validate Split Keyboard Pairing

## Methods to Check if Right Half is Connected:

### 1. **ZMK Studio (if enabled)**
- Connect left half via USB
- Open ZMK Studio
- Check connection status indicators
- Look for split connection status

### 2. **USB Serial Logs**
First, check if USB device is detected:
```bash
# Check USB devices
lsusb | grep -i feed
# Should show: Bus XXX Device XXX: ID feed:0000 Kryvonis handwired/split_uno

# Check for serial devices
ls -la /dev/ttyACM* /dev/ttyUSB* 2>/dev/null

# Check system logs for device
dmesg | tail -20 | grep -i "tty\|usb\|cdc\|acm"
```

Connect left half via USB and monitor serial output:
```bash
# Find the device (usually /dev/ttyACM0 or /dev/ttyACM1)
ls /dev/ttyACM*

# Monitor logs (replace with your device)
screen /dev/ttyACM0 115200
# or
minicom -D /dev/ttyACM0 -b 115200
# or
picocom /dev/ttyACM0 -b 115200
```

**If no /dev/ttyACM* device appears:**
- USB CDC/ACM might not be enabled in firmware
- Rebuild firmware with USB enabled (I've added USB configs)
- Check permissions: `sudo chmod 666 /dev/ttyACM0` (if it exists)
- Check if you need udev rules

Look for messages like:
- "split: connected" 
- "ble: peripheral connected"
- Any errors about split connection

### 3. **Test Keys on Right Half**
- Press keys that should send characters from right half (N6-N0, Y-U-I-O-P, etc.)
- If keys work → connected
- If nothing happens → not connected

### 4. **Reset and Pair Process**
1. Power off both halves
2. Press reset buttons simultaneously on both halves
3. Wait 5-10 seconds
4. Try typing on right half keys

### 5. **Clear Bonds and Re-pair**
1. Flash settings_reset to both halves
2. Flash regular firmware to both halves
3. Reset both simultaneously
4. Wait for pairing (can take 30-60 seconds)

### 6. **Check Battery/Power**
- Right half needs adequate power to stay connected
- Try with USB power first to rule out battery issues

## Debugging Enabled
I've enabled debug logging. After rebuilding and flashing, check serial logs for detailed connection messages.

## Quick Test: Right Half Working?

**Right now, without USB serial:**
1. Device is detected: `lsusb` shows `SplitDos` (device 049)
2. Test right half keys:
   - Press keys on right half: `N6`, `N7`, `N8`, `N9`, `N0`, `Y`, `U`, `I`, `O`, `P`, etc.
   - If characters appear → **Right half is connected and working!**
   - If nothing happens → Right half needs pairing/config fix

**After rebuilding with USB config:**
- `/dev/ttyACM0` or `/dev/ttyACM1` should appear
- Monitor logs: `screen /dev/ttyACM0 115200`
- Look for split connection messages

