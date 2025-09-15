# 720x720 DSI Display (FL7707N Controller)

## Overview

This directory contains documentation and configuration files for supporting a 720x720 DSI display with FL7707N controller.

**Product Link**: [AliExpress - 720x720 DSI Display](https://www.aliexpress.us/item/3256807565141061.html)

**IMPORTANT**: This specific display module does NOT include touch hardware. While GT911 touch controller documentation is provided for reference, the current implementation focuses solely on display output. The PicoCalc hardware would require modifications to support touchscreen functionality.

## Specifications

- **Resolution**: 720x720 pixels
- **Panel Type**: IPS
- **Interface**: 2-lane MIPI DSI
- **Display Controller**: FL7707N
- **Touch Controller**: None (display-only module)

**Note**: While GT911 touch controller configuration is available in the documentation, this specific display module does not include touch hardware.

## Display Timing Parameters

```
Width:  720
Height: 720

Vertical Timing:
- VFP: 20 (Vertical Front Porch)
- VBP: 20 (Vertical Back Porch) 
- VSA: 4  (Vertical Sync Active)

Horizontal Timing:
- HFP: 106 (Horizontal Front Porch)
- HBP: 120 (Horizontal Back Porch)
- HSA: 60  (Horizontal Sync Active)
```

## Files

### Display Controller Documentation
- `2_FL7707N_QV040YNQ-N80_IPS_Code_2Power_V5.5_20230810.txt` - Display initialization sequence and timing configuration
- `FL7707N_DS_V0.2_20230324_Customer.pdf` - FL7707N display controller datasheet and specifications

### Display Module Documentation  
- `HD395003C30-V2 规格书.pdf` - Display module specifications
- `HD395003C30工程图20231023.dwg` - Engineering drawings and mechanical specifications (AutoCAD format)

### Touch Controller Configuration (Reference Only)
- `../../touch/FT040037_GT911_Config_20231103.cfg` - GT911 touch controller configuration (not applicable to current display samples - no touch panel)

## DSI Configuration

- **DSI Mode**: 2-lane (0x31 in register 0xBA[1])
- **Power Mode**: 2-power configuration
- **Color Format**: RGB888

## Implementation Status

- [x] Device tree overlay creation
- [x] Display driver implementation
- [x] Timing parameter configuration
- [x] Power sequence setup
- [ ] Testing and validation

## Notes

The initialization sequence includes:
- Power management configuration (B8 register for 2-power mode)
- DSI lane configuration (BA register)
- Gamma correction settings (E0 register)
- GIP (Gate In Panel) timing configuration (E9/EA registers)
- Touch controller I2C configuration

## Adapter

In order to connect the display to the Lyra, you need will to construct an adapter with the following pinout:
| Lyra Pin # | Display Pin # | Signal Name      | Description                       | Connection               |
|------------|---------------|------------------|-----------------------------------|--------------------------|
| 4          | 1             | LEDK             | Backlight Cathode                 | (See circuit)            |
| 4          | 2             | LEDK             | Backlight Cathode                 | (See circuit)            |
| 6          | 3             | LEDA             | Backlight Anode                   | (See circuit)            |
| 4          | 4             | GND              | Ground                            | GND                      |
| 5          | 5             | RST              | Reset Input Pin                   | GPIO1_C4                 |
| 7          | 6             | GND              | Ground                            | GND                      |
| 1          | 7             | VCC              | Power Supply 2.8V-3.3V            | VCC                      |
| 7          | 8             | GND              | Ground                            | GND                      |
| 1          | 9             | IOVCC            | Power Supply 1.8V-3.3V            | VCC                      |
| 10         | 10            | GND              | Ground                            | GND                      |
| 20         | 11            | D0P              | MIPI DSI Data Lane 0 Positive     | DSI_TX_D0_P              |
| 21         | 12            | D0N              | MIPI DSI Data Lane 0 Negative     | DSI_TX_D0_N              |
| 13         | 13            | GND              | Ground                            | GND                      |
| 17         | 14            | D1P              | MIPI DSI Data Lane 1 Positive     | DSI_TX_D1_P              |
| 18         | 15            | D1N              | MIPI DSI Data Lane 1 Negative     | DSI_TX_D1_N              |
| 16         | 16            | GND              | Ground                            | GND                      |
| 14         | 17            | CLKP             | MIPI DSI Clock Lane Positive      | DSI_TX_CLK_P             |
| 15         | 18            | CLKN             | MIPI DSI Clock Lane Negative      | DSI_TX_CLK_N             |
| 19         | 19            | GND              | Ground                            | GND                      |
| 19         | 20            | GND              | Ground                            | GND                      |
| 19         | 21            | GND              | Ground                            | GND                      |
| 22         | 22            | GND              | Ground                            | GND                      |
| 22         | 23            | GND              | Ground                            | GND                      |
| 22         | 24            | GND              | Ground                            | GND                      |
| NC         | 25            | TP_INT           | Touchpad                          | N.C.                     |
| 2          | 26            | TP_SDA           | Touchpad / BL I2C                 | GPIO0_A0 RMIO_0 I2C2_SDA |
| 3          | 27            | TP_SCL           | Touchpad / BL I2C                 | GPIO0_A1 RMIO_1 I2C2_SCL |
| NC         | 28            | TP_RST           | Touchpad                          | GPIO1_C4                 |
| NC         | 29            | TP_VCI           | Touchpad                          | VCC                      |
| NC         | 30            | GND              | Ground                            |                          |
