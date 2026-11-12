# Starter Flight Computer — Parts List

Last updated: 12 Nov 2026

## Parts list

| Subsystem | Interface to MCU | Key parts |
| --- | --- | --- |
| [Power](#power) | ADC (battery voltage) | 1S LiPo, FDN302P, AP2112K LDO |
| [MCU core](#mcu-core) | — | STM32F405, 8 MHz crystal, reset/BOOT0 buttons |
| [USB](#usb) | USB | USB-C receptacle, USBLC6-2SC6 |
| [Debug and programming](#debug-and-programming) | SWD | 2×5 header, STLINK-V3MINIE |
| [Sensors](#sensors) | SPI1, I²C | LSM6DSV32X, BMP585, MMC5983MA |
| [Data logging](#data-logging) | SPI2, SDIO | W25Q128JV, microSD socket |
| [Buzzer](#buzzer) | GPIO | CSS-J4B20 buzzer, AO3400A |
| [Pyro](#pyro) | GPIO | 2S connector, AO3400A ×3, AO3401A |
| [Passives](#passives) | — | All resistor and capacitor models |

### Changes on 12 Nov 2026

Three parts were out of stock and were swapped. The board is still being designed, so the new footprints go into the schematic and layout from the start.

| Was | Now | What changes |
| --- | --- | --- |
| LSM6DSO32TR (IMU) | LSM6DSV32XTR | Same ±32 g range and 2.5 × 3 mm LGA-14 package, but a different register map, so it needs its own driver. Check its pinout against the footprint. |
| KYOCERA AVX KGM21AR51E106KU (10 µF, 0805, 25 V) | Taiyo Yuden EMK107BBJ106MA-T (10 µF, 0603, 16 V) | 0603 footprint instead of 0805. 16 V is plenty for a 1S cell. |
| Yageo RC0603FR-07100KL (100 kΩ, 0603) | Panasonic ERJ-2RKF1003X (100 kΩ, 0402) | 0402 footprint, the only 0402 part on the board; every other passive is 0603. |

### Power

The 1S cell powers all logic through a reverse-polarity FET and a 3.3 V LDO; the ADC reads battery voltage through the divider.

| Part type | Part number |
| --- | --- |
| Logic battery (1S LiPo) | [Adafruit 1578](https://www.digikey.com/en/products/detail/adafruit-industries-llc/1578/5054539) 3.7 V 500 mAh, JST-PH |
| Battery connector | [JST S2B-PH-SM4-TB](https://www.digikey.com/en/products/detail/jst-sales-america-inc/S2B-PH-SM4-TB/926655) |
| Reverse-polarity FET | [FDN302P](https://www.digikey.com/en/products/detail/onsemi/FDN302P/413230) P-channel |
| 3.3 V LDO | [AP2112K-3.3TRG1](https://www.digikey.com/en/products/detail/diodes-incorporated/AP2112K-3-3TRG1/4470746) |
| Power LED | [Lite-On LTST-C191KRKT](https://www.digikey.com/en/products/detail/liteon/LTST-C191KRKT/386837) red, 0603 |

### MCU core

Everything the STM32F405 needs to boot and run: its clock, boot and reset controls, and a status LED.

| Part type | Part number |
| --- | --- |
| MCU | [STM32F405RGT6W](https://www.digikey.com/en/products/detail/stmicroelectronics/STM32F405RGT6W/4357233) (LQFP-64) |
| 8 MHz crystal (HSE) | [Abracon ABM3B-8.000MHZ-B2-T](https://www.digikey.com/en/products/detail/abracon-llc/ABM3B-8-000MHZ-B2-T/1873166) |
| Reset and BOOT0 buttons | [C&K PTS810 SJM 250 SMTR LFS](https://www.digikey.com/en/products/detail/c-k/PTS810SJM250SMTR-LFS/4176610) |
| Status LED | [Lite-On LTST-C191KGKT](https://www.digikey.com/en/products/detail/liteon/LTST-C191KGKT/386835) green, 0603 |

### USB

USB-C carries data only: DFU flashing and log download. It does not power the board; the 1S LiPo powers all logic, even when USB is plugged in.

| Part type | Part number |
| --- | --- |
| USB-C receptacle | [GCT USB4105-GF-A](https://www.digikey.com/en/products/detail/gct/USB4105-GF-A/11198441) |
| USB ESD protection | [USBLC6-2SC6](https://www.digikey.com/en/products/detail/stmicroelectronics/USBLC6-2SC6/1040559) |

### Debug and programming

Only the header goes on the board; the probe and cable are shared team tools.

| Part type | Part number |
| --- | --- |
| SWD debug header | [Samtec FTSH-105-01-F-DV-K](https://www.digikey.com/en/products/detail/samtec-inc/FTSH-105-01-F-DV-K/2649974) 2×5 1.27 mm |
| Debug probe (off-board) | [STLINK-V3MINIE](https://www.digikey.com/en/products/detail/stmicroelectronics/STLINK-V3MINIE/16284301) |
| SWD cable (off-board) | [Adafruit 1675](https://www.digikey.com/en/products/detail/adafruit-industries-llc/1675/6827142) 10-pin 1.27 mm |

### Sensors

The IMU and barometer share SPI1 with separate chip selects; the magnetometer sits alone on I²C.

| Part type | Part number |
| --- | --- |
| IMU (accel + gyro) | [LSM6DSV32XTR](https://www.digikey.com/en/products/detail/stmicroelectronics/LSM6DSV32XTR/21700730) (replaces LSM6DSO32TR; different register map) |
| Barometer | [BMP585](https://www.digikey.com/en/products/detail/bosch-sensortec/BMP585/19522589) |
| Magnetometer | [MMC5983MA](https://www.digikey.com/en/products/detail/memsic-inc/MMC5983MA/10452795) |

### Data logging

The microSD card (SDIO, 4-bit) is the main log; the SPI2 flash is the backup.

| Part type | Part number |
| --- | --- |
| Flash (backup log) | [W25Q128JVSIQ](https://www.digikey.com/en/products/detail/winbond-electronics/W25Q128JVSIQ/5803943) 16 MB SPI NOR |
| microSD socket | [Molex 1040310811](https://www.digikey.com/en/products/detail/molex/1040310811/2370379) |

### Buzzer

A GPIO drives the magnetic buzzer through a low-side FET, with a flyback diode across the coil.

| Part type | Part number |
| --- | --- |
| Buzzer | [Same Sky CSS-J4B20-SMT-TR](https://www.digikey.com/en/products/detail/same-sky-formerly-cui-devices/CSS-J4B20-SMT-TR/1003797) |
| Buzzer driver FET | [AO3400A](https://www.digikey.com/en/products/detail/alpha-omega-semiconductor-inc/AO3400A/1855772) N-channel |
| Flyback diode | [Diodes Inc 1N4148W-7-F](https://www.digikey.com/en/products/detail/diodes-incorporated/1N4148W-7-F/814371) |

### Pyro

A separate 2S pack fires the drogue and main channels through a high-side arm FET and one low-side fire FET per channel.

| Part type | Part number |
| --- | --- |
| Pyro battery connector | [JST S2B-XH-A](https://www.digikey.com/en/products/detail/jst-sales-america-inc/S2B-XH-A/1651055) |
| Pyro battery plug housing (off-board) | [JST XHP-2](https://www.digikey.com/en/products/result?keywords=XHP-2) |
| Pyro battery plug contacts (off-board) | [JST SXH-001T-P0.6](https://www.digikey.com/en/products/detail/jst-sales-america-inc/SXH-001T-P0-6/527371) |
| Fire FETs (drogue, main, arm-gate driver) | [AO3400A](https://www.digikey.com/en/products/detail/alpha-omega-semiconductor-inc/AO3400A/1855772) N-channel |
| Arm FET | [AO3401A](https://www.digikey.com/en/products/detail/alpha-omega-semiconductor-inc/AO3401A/1855773) P-channel |

### Passives

Every resistor and capacitor on the board comes from this list. When your schematic needs one of these values, use this exact part so everyone builds from the same models. All are 0603 except the 10 µF capacitor (0603, 16 V) and the 100 kΩ resistor (0402), which changed on 12 Nov 2026.

| Part type | Part number |
| --- | --- |
| Capacitor, 27 pF, 0603, C0G, 50 V | [Samsung CL10C270JB8NNNC](https://www.digikey.com/en/products/detail/samsung-electro-mechanics/CL10C270JB8NNNC/3886744) |
| Capacitor, 100 nF, 0603, X7R, 50 V | [KEMET C0603C104K5RACTU](https://www.digikey.com/en/products/detail/kemet/C0603C104K5RACTU/1465594) |
| Capacitor, 1 µF, 0603, X5R, 50 V | [Yageo CC0603KRX5R9BB105](https://www.digikey.com/en/products/detail/yageo/CC0603KRX5R9BB105/5195195) |
| Capacitor, 2.2 µF, 0603, X5R, 16 V | [Samsung CL10A225KO8NNNC](https://www.digikey.com/en/products/detail/samsung-electro-mechanics/CL10A225KO8NNNC/3886698) |
| Capacitor, 10 µF, **0603**, X5R, 16 V | [Taiyo Yuden EMK107BBJ106MA-T](https://www.digikey.com/en/products/detail/taiyo-yuden/EMK107BBJ106MA-T/3660079) (replaces KGM21AR51E106KU, 0805) |
| Resistor, 1 kΩ, 0603, 1% | [Yageo RC0603FR-071KL](https://www.digikey.com/en/products/detail/yageo/RC0603FR-071KL/726843) |
| Resistor, 4.7 kΩ, 0603, 1% | [Yageo RC0603FR-074K7L](https://www.digikey.com/en/products/detail/yageo/RC0603FR-074K7L/727212) |
| Resistor, 5.1 kΩ, 0603, 1% | [Yageo AF0603FR-075K1L](https://www.digikey.com/en/products/detail/yageo/AF0603FR-075K1L/5901075) |
| Resistor, 10 kΩ, 0603, 1% | [Yageo AR0603FR-0710KL](https://www.digikey.com/en/products/detail/yageo/AR0603FR-0710KL/5904597) |
| Resistor, 47 kΩ, 0603, 1% | [Yageo RC0603FR-0747KL](https://www.digikey.com/en/products/detail/yageo/RC0603FR-0747KL/727253) |
| Resistor, 100 kΩ, **0402**, 1% | [Panasonic ERJ-2RKF1003X](https://www.digikey.com/en/products/detail/panasonic-industry/ERJ-2RKF1003X/192081) (replaces RC0603FR-07100KL, 0603) |
