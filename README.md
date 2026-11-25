# LWM2-62 LoRa End Device RF Module

## Introduction

![LINK LOGIX LWM2-62 LoRa end-device RF module](./assets/lwm2-62-product.png)

The LWM2-62 is a compact LoRa end-device RF module made by **LINK LOGIX** for
long-range, low-power wireless connectivity in embedded and industrial IoT
products. It integrates a Semtech SX1262 LoRa transceiver with an STM32
32-bit ARM Cortex-M0+ microcontroller, so a host device can use the module's
wireless features through its hardware interfaces and firmware.

The module is intended for operation in the 865–867 MHz ISM band. It supports
LoRaWAN Class A and Class C; LoRa point-to-point communication is also possible
with suitable firmware. Example application areas include smart metering,
industrial monitoring, building automation, and wireless security or
irrigation. The seller advertises a line-of-sight range of up to 5 km; actual
range depends on antenna, installation, terrain, and radio conditions.

Key features include:

- Compact 22 x 19 mm shielded package
- RF output power up to +22 dBm
- Receiver sensitivity down to -137 dBm
- 3.0 V to 3.6 V supply voltage
- UART and I2C interfaces
- Built-in EEPROM for non-volatile data storage
- Low-power operation, including a sleep current below 5 uA as listed by the
  seller

See the [LWM2-62 datasheet](./DS_LWM2-62_v1.0%20(1).pdf) for module and
electrical details.

## Manufacturer, purchase, and contact

- **Manufacturer:** [LINK LOGIX](https://link-logix.in/) — [official website](https://www.link-logix.in/)
- **Purchase:** [Buy the LWM2-62 from Quartz Components](https://quartzcomponents.com/products/lwm2-62-lorawan-rf-module-sx1262-long-range-wireless-transceiver)
- **LINK LOGIX phone:** +91 91160 72233 (shown in the LINK LOGIX product
  graphic provided with this project)
- **Quartz Components support:** [support@quartzcomponents.com](mailto:support@quartzcomponents.com)

## Folder overview

### `LOW LEVEL DOVELOPMENT`

Contains project documentation and firmware for a custom STM32L071KBU6 +
SX1262 design:

- [Folder README](./LOW%20LEVEL%20DOVELOPMENT/README.md) provides a detailed
  guide to the project documents, code layout, and UART-to-I2C migration.
- [Project PDF](./LOW%20LEVEL%20DOVELOPMENT/stm32_LoRa.pdf) and
  [Word document](./LOW%20LEVEL%20DOVELOPMENT/stm32_LoRa/STM32_LoRa.docx)
  describe the hardware and pin mapping, clocks, Semtech SWL2001 LoRa Basics
  Modem stack, board-support code, application modes and commands, low-power
  Stop mode, and EEPROM handling.
- `stm32_LoRa/stm32_LoRa/MDK-ARM/` contains the Keil uVision project and the
  application sources:
  - `main.c` starts the board and application.
  - `bsp_mcu.c/.h` configure MCU clocks and peripherals, the UART console,
    I2C bus, watchdog, and Stop mode.
  - `lora_app.c/.h` implement the application and LoRaWAN/P2P behavior.
  - `sensor_manager.c/.h` prepare sensor telemetry payloads.
  - `eeprom_manager.c/.h` manage the MCU EEPROM data.
  - `modem_pinout.h` maps MCU pins to the SX1262 radio.
- `Core/` and `Drivers/` contain MCU startup and STM32/CMSIS support files.
- `SWL2001-master/` contains Semtech modem, radio, HAL, and example code.



### `QUICK_START`

The `UART/` subfolder contains a ready-to-flash Intel HEX firmware image:

- [`LWM2-62-firmware.hex`](./QUICK_START/UART/LWM2-62-firmware.hex) — a
  prebuilt firmware image copied from the Keil project output. You do not need
  to install Keil or rebuild the source just to flash this image.

This image is the firmware build currently supplied with the project. Flashing
it does not configure the module for a different UART setup or replace UART
with I2C; those changes require firmware development and a new build.

## Exploring the module

The LWM2-62 combines a radio and a microcontroller in one small module. The
SX1262 handles LoRa radio communication, while the STM32 runs the application
that controls the radio and communicates with sensors or a host. It is designed
for low-power devices that send small amounts of data over long distances, such
as meters, industrial monitors, and remote sensors.

A practical way to explore it is to start with the datasheet for electrical
limits and pin connections, review the low-level project README to understand
the firmware, then flash the supplied image and connect through the configured
UART to try the firmware's commands. LoRaWAN operation also requires suitable
network coverage and valid device credentials; range and power use depend on
the antenna, environment, and how the application is configured.
