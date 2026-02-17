# Testing Guide for WackyMXter Firmware

## Background
This guide helps diagnose and fix issues where one side of the split keyboard may not work after flashing new firmware.

## Common Issue: "Left Keyboard is Dead"

### Symptoms
- Right side works fine
- Left side doesn't respond to any key presses
- Keys on left side feel mechanical but don't register

### Most Common Causes

#### 1. Firmware Version Mismatch
**Problem**: Left and right sides have different firmware versions, breaking communication.

**Solution**:
```bash
# Always flash BOTH sides when updating firmware
1. Download firmware from GitHub Actions (both left and right .uf2 files)
2. Flash left microcontroller with *-left.uf2
3. Flash right microcontroller with *-right.uf2  
4. Power cycle both sides
```

#### 2. GitHub Actions Build Cache
**Problem**: Old cached artifacts may be reused instead of building fresh firmware.

**Solution**:
```bash
# Trigger a clean build
1. Go to Actions tab: https://github.com/YOUR_USERNAME/WackyMXter-zmk-config/actions
2. Click "Build" workflow
3. Click "Run workflow" → "Run workflow" 
4. Wait for build to complete (usually 5-10 minutes)
5. Download artifacts and flash both sides
```

#### 3. Incomplete Flash
**Problem**: Flashing was interrupted or failed silently.

**Solution**:
```bash
# Proper flashing procedure
1. Double-click reset button on microcontroller
2. Wait for storage device to appear (e.g., "XIAO-SENSE")
3. Drag and drop .uf2 file
4. Wait for device to disconnect/reconnect automatically
5. Do NOT unplug during flashing!
```

#### 4. Wrong Firmware File
**Problem**: Flashed right firmware to left side or vice versa.

**Solution**:
```bash
# Check filenames carefully
wackymxter_left-seeeduino_xiao_ble-zmk.uf2  → LEFT side
wackymxter_right-seeeduino_xiao_ble-zmk.uf2 → RIGHT side
```

## Diagnostic Steps

### Step 1: Verify Both Sides Power On
- Do both microcontrollers have power LEDs lit?
- If not, check USB cable and power

### Step 2: Check Bluetooth Pairing
- The right side (central) should show up in Bluetooth devices
- The left side (peripheral) does NOT show up separately
- Left side connects to right side, not directly to computer

### Step 3: Test Matrix Scanning
- Connect right side to computer via USB
- Does right side work? If yes, firmware is basically OK
- If left side doesn't work, it's likely a communication issue

### Step 4: Reset Both Sides
```bash
# Hard reset procedure
1. Disconnect USB from right side
2. Hold reset button on left side for 3 seconds
3. Hold reset button on right side for 3 seconds
4. Reconnect right side to computer
5. Wait 30 seconds for pairing
```

### Step 5: Check Serial Console (Advanced)
```bash
# If you have serial console access
1. Connect via serial at 115200 baud
2. Look for error messages during boot
3. Check for "peripheral" and "central" messages
```

## Key Code Reference

### Standard Keys Used in WackyMXter
- `MINUS`: The minus/underscore key (-/\_)
- `EQUAL`: The equals/plus key (=/+)
- `KP_MINUS`: Keypad minus
- `GRAVE`: Backtick/tilde key (\`/~)

All key codes are documented in ZMK here:
https://zmk.dev/docs/codes

## If Issues Persist

If the left keyboard is still dead after trying all the above:

1. **Check hardware**: 
   - Inspect solder joints on left microcontroller
   - Test with a different USB cable
   - Try different USB ports

2. **Test with original firmware**:
   - Revert to a known working commit
   - Flash that version to both sides
   - If it works, the issue is in the keymap changes

3. **File an issue**:
   - Include the exact commit SHA that doesn't work
   - Include build logs from GitHub Actions
   - Include any serial console output
   - Describe which specific keys don't work

## Useful Links
- [ZMK Documentation](https://zmk.dev/)
- [ZMK Split Keyboard Setup](https://zmk.dev/docs/features/split-keyboards)
- [GitHub Actions Logs](https://github.com/vivianyyd/WackyMXter-zmk-config/actions)
