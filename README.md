# Dilemma DIY 3x5+2 ZMK Configuration

A fully optimized ZMK firmware configuration for the Dilemma DIY split keyboard with ergonomic features, home row modifiers, and gaming compatibility.

## 🚀 Features

- **Home Row Modifiers**: Tap-preferred modifiers on ASDF and JKL; for enhanced ergonomics
- **6-Layer Design**: Comprehensive layout covering all use cases
- **Gaming Layer**: Pure QWERTY with dedicated gaming mode toggle
- **Combo Shortcuts**: Quick access to common actions (Undo, Cut, Copy, Paste)
- **Optimized Timing**: 175ms tap-preferred timing for reliable modifier operation
- **Bluetooth Support**: Full wireless capability with Nice!nano v2 controllers
- **Complete Documentation**: Printable layout guide with ASCII art representations

## 🔧 Hardware Overview

- **Keyboard**: Dilemma DIY 3x5+2 Split Keyboard
- **Controllers**: Nice!nano v2 (left and right)
- **Layout**: 34-key split with 3 rows × 5 columns + 2 thumb keys per side
- **Connectivity**: Wireless (Bluetooth) and USB-C
- **Optional**: Rotary encoder support

## 📋 Layer Overview

| Layer | Name | Purpose | Access |
|-------|------|---------|----------|
| 0 | Base | QWERTY with home row mods | Default |
| 1 | Symbols | Numbers, punctuation, symbols | Hold B or N |
| 2 | Function | F-keys and numpad | Hold G or H |
| 3 | Navigation | Arrows, mouse, navigation | Hold T or Y |
| 4 | System | Bluetooth, media, system | B+DEL or N+ENT combo |
| 5 | Gaming | Pure QWERTY for gaming | Q+P combo toggle |

## ⌨️ Key Features

### Home Row Modifiers
- **Left Hand**: CTRL(A), ALT(S), GUI(D), SHIFT(F)
- **Right Hand**: SHIFT(J), GUI(K), ALT(L), CTRL(;)
- **Timing**: 175ms tap-preferred for reliable operation

### Combo Shortcuts
- **Z + X**: Undo (CTRL+Z)
- **X + C**: Cut (CTRL+X) 
- **C + V**: Copy (CTRL+C)
- **V + B**: Paste (CTRL+V)

### System Access
- **B + DEL**: System layer (left hand)
- **N + ENT**: System layer (right hand)
- **Q + P**: Toggle gaming layer

## 🛠️ Build Instructions

### Using GitHub Actions (Recommended)

1. **Fork ZMK Config Template**:
```bash
git clone https://github.com/zmkfirmware/zmk-config.git your-keyboard-config
cd your-keyboard-config
```

2. **Copy Configuration Files**:
```bash
# Copy keymap and build config
cp /path/to/diy_dilemma_3x5_2.keymap config/
cp /path/to/build.yaml config/

# Create shield directory
mkdir -p config/boards/shields/diy_dilemma_3x5_2

# Copy shield files
cp /path/to/diy_dilemma_3x5_2.dtsi config/boards/shields/diy_dilemma_3x5_2/
cp /path/to/diy_dilemma_3x5_2_left.overlay config/boards/shields/diy_dilemma_3x5_2/
cp /path/to/diy_dilemma_3x5_2_right.overlay config/boards/shields/diy_dilemma_3x5_2/
cp /path/to/Kconfig.defconfig config/boards/shields/diy_dilemma_3x5_2/
cp /path/to/Kconfig.shield config/boards/shields/diy_dilemma_3x5_2/
```

3. **Push to GitHub**:
```bash
git add .
git commit -m "Add Dilemma DIY ZMK configuration"
git push
```

4. **Download Firmware**: Check GitHub Actions tab for built `.uf2` files

## 📖 Documentation

- **[Dilemma_Keyboard_Layout_Guide.md](Dilemma_Keyboard_Layout_Guide.md)**: Complete printable layout reference with ASCII diagrams
- Each layer documented with visual representations
- Feature explanations and quick reference tables

## 🎮 Gaming Mode

Gaming layer provides pure QWERTY without any layer taps or home row mods that could interfere with gaming:

- **Toggle**: Q+P combo to enable/disable
- **Features**: Standard QWERTY layout, dedicated SHIFT/CTRL thumb keys
- **Weapon Numbers**: LSHIFT+LCTRL combo for momentary access to Layer 1

## 📱 Flashing Instructions

1. **Enter Bootloader Mode**:
   - Double-press the reset button on your Nice!nano
   - Or use the BOOTLOADER key in Layer 4

2. **Flash Firmware**:
   - Copy `zmk.uf2` file to the mounted drive
   - Repeat for both left and right controllers

3. **Pairing**:
   - Flash left side first, then right side
   - Controllers will automatically pair

## 🔧 Technical Specifications

### Pin Configuration

### Row Pins (both halves)
- Row 0: P0.31 (D4)
- Row 1: P0.29 (D5) 
- Row 2: P0.02 (D6)
- Row 3: P1.15 (D7)

### Column Pins
#### Left Half
- Col 0: P0.10 (D19)
- Col 1: P0.09 (D18) 
- Col 2: P1.06 (D15)
- Col 3: P0.08 (D14)
- Col 4: P0.06 (D16)

#### Right Half  
- Col 0: P0.06 (D16)
- Col 1: P0.08 (D14)
- Col 2: P1.06 (D15) 
- Col 3: P0.09 (D18)
- Col 4: P0.10 (D19)

### Rotary Encoder (optional)
- A Pin: P0.24 (D20)
- B Pin: P1.00 (D21)

## ZMK Configuration Files

### Required Files Structure

```
config/
├── diy_dilemma_3x5_2.keymap          # Main keymap configuration
├── build.yaml                         # Build configuration
└── boards/shields/diy_dilemma_3x5_2/
    ├── Kconfig.shield                 # Shield configuration
    ├── Kconfig.defconfig              # Default configuration
    ├── diy_dilemma_3x5_2.dtsi        # Main device tree
    ├── diy_dilemma_3x5_2_left.overlay # Left half overlay
    └── diy_dilemma_3x5_2_right.overlay# Right half overlay
```

### Build Configuration

Your `build.yaml` should include:

```yaml
---
include:
  - board: nice_nano_v2
    shield: diy_dilemma_3x5_2_left
  - board: nice_nano_v2  
    shield: diy_dilemma_3x5_2_right
```

## 🔧 Customization

### Timing Adjustments
```c
// In keymap, adjust tapping term
&mt {
    flavor = "tap-preferred";
    tapping_term_ms = <175>;  // Adjust this value
};
```

### Adding Combos
```c
// Example combo definition
combo_new_shortcut {
    timeout-ms = <50>;
    key-positions = <0 1>;  // Key positions to trigger
    bindings = <&kp ESC>;   // Action to perform
};
```

## 🐛 Troubleshooting

### Home Row Mods Triggering Accidentally
- Increase `tapping_term_ms` value (try 200-250ms)
- Ensure you're tapping quickly, not holding

### Bluetooth Connectivity Issues
- Use Layer 4 to clear Bluetooth profiles (`BT_CLR` or `BT_CLR_ALL`)
- Try selecting specific profile (BT_0-4)

### Combos Not Working
- Check `timeout-ms` setting (try increasing to 50-100ms)
- Verify key positions match your intended keys

### Build Failures
- Ensure all required files are in correct directories
- Check shield names match exactly in all files

## Legacy Features (from original configuration)

### Original Layer Access
- Layer 1 (Lower): Numbers, symbols, media keys - Access via B/N keys
- Layer 2 (Raise): Function keys, numpad - Access via G/H keys  
- Layer 3 (Adjust): System functions, reset - Access via T/Y keys

### Original Combos
- Q+W = ESC
- A+S = TAB
- S+D = LGUI
- D+F = LALT  
- J+K = RALT
- K+L = RGUI

---

**Last Updated**: March 18, 2026  
**Keymap Version**: Home Row Mods + Combo Optimized  
**ZMK Version**: Latest (as of build date)
3. Copy the respective `.uf2` file to each controller
4. The halves should automatically pair via Bluetooth

## Troubleshooting

- Ensure the pin assignments match your actual wiring
- Check that diodes are oriented correctly (cathode toward MCU)
- Verify that the Nice!Nanos are properly seated in their sockets
- Use a multimeter to check for shorts or broken connections

## Customization

The keymap can be customized by modifying the `diy_dilemma_3x5_2.keymap` file. Refer to the ZMK documentation for available keycodes and behaviors.