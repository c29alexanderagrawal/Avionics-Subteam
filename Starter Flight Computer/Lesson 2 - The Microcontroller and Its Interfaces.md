# Lesson 2: The Microcontroller and Its Interfaces

## What you should be able to explain after this lesson

- What a microcontroller is and how it differs from the computer on your desk.
- What is inside the STM32F405 and why this chip suits a flight computer.
- What the chip needs around it to run at all: power, decoupling, a clock, reset and boot control.
- Why choosing which pin does what is a design decision, not an afterthought.
- The basic interfaces the MCU uses to talk to every other part on the board, and why each part uses the one it does.

## What a microcontroller is

A **microcontroller (MCU)** is a complete small computer on one chip: a processor core, program memory, working memory, and a set of built-in hardware blocks called **peripherals** that handle the outside world (talking to sensors, keeping time, measuring voltages, driving pins).

It differs from a laptop in a few ways that matter for a flight computer:

- **It runs one program, forever.** There is no operating system juggling apps. From power-on, the chip runs your firmware in a loop until the battery is pulled.
- **It is built for timing, not raw speed.** What matters is reading a sensor at exactly the right moment, every time, and reacting within microseconds. This is what people mean by **real-time**.
- **It talks directly to hardware.** Every peripheral is controlled by writing and reading **registers**: fixed memory addresses whose bits switch hardware on and off, set speeds, or hold incoming data. Firmware is, at its lowest level, reading and writing registers.
- **It is small, cheap and low-power.** It runs from a few milliamps to tens of milliamps on a single LiPo cell.

On this board the MCU is the hub. Every sensor, the memory, the buzzer and the USB port connect to it, and nothing on the board talks to anything else except through it.

## Inside the STM32F405

The STM32F405 is a member of ST's STM32F4 family, built around an **Arm Cortex-M4** core. The parts worth understanding:

- **The core.** A 32-bit processor running at up to 168 MHz. That is far more than a logger strictly needs, which is the point: it leaves headroom for filtering, flight-phase detection and future features without having to redesign the board.
- **The floating-point unit (FPU).** Hardware that does decimal math (multiplying, dividing, square roots on numbers like 1013.25) in a single step. Converting pressure to altitude, filtering sensor noise and combining sensors all lean on floating-point math. Without an FPU, the same math is done slowly in software.
- **Flash memory (1 MB).** Where the firmware lives. It survives power loss. Note that this is the MCU's own program memory, separate from the flash chip on the board that stores flight data.
- **SRAM (192 KB).** Fast working memory for variables and buffers. It is erased the moment power is lost, which is why flight data cannot just sit in RAM.
- **Peripherals.** Dedicated hardware blocks that do jobs on their own while the core does something else. The ones this board relies on:
  - **SPI, I²C and USART controllers** to talk to sensors, memory and the GPS.
  - **SDIO** to talk to the microSD card.
  - **USB** for connecting to a computer.
  - **Timers** for keeping time and generating signals such as the buzzer's tone.
  - **ADC** (analog-to-digital converter) to measure voltages such as the battery's.
  - **GPIO** (general-purpose input/output) for simple on/off pins such as LEDs.
- **DMA (direct memory access).** A helper that moves data between peripherals and memory without the core touching every byte. For example, DMA can pull a block of IMU readings into RAM, or push a block of log data out to the flash chip, while the core keeps running flight logic.

## What keeps the chip running

An MCU on its own does nothing. It needs a small set of supporting parts around it, and most first-board failures come from getting one of these wrong.

### Power and decoupling

- The chip runs from the board's **3.3 V rail** and has several power pins (VDD), each of which needs its own nearby **decoupling capacitor**.
- Why: when the core and peripherals switch, they pull short bursts of current. The trace back to the regulator is too long and slow to supply those bursts, so a small capacitor right at the pin acts as a local reservoir. Without it, the supply voltage dips and the chip can glitch or reset.
- **VDDA** powers the analog circuits, including the ADC. It is filtered separately so digital noise does not leak into voltage measurements.
- **VCAP pins.** The core runs at about 1.2 V from a regulator inside the chip. That internal regulator needs external capacitors on the VCAP pins to stay stable. Leave them off, or use the wrong value, and the chip will not run reliably. This is a classic "read the datasheet" requirement.

### Clocks

Everything inside the MCU happens in step with a clock signal. The chip has two main sources:

- **The internal oscillator (HSI):** built in and needs no parts, but its frequency drifts with temperature and from chip to chip.
- **The external crystal (HSE):** an 8 MHz crystal on the board, which is far more accurate.

A block called the **PLL** multiplies the base clock up to the fast frequencies the core and peripherals use. The reason the board carries an external crystal is **USB**: it needs a 48 MHz clock that is accurate to a fraction of a percent, which the internal oscillator cannot guarantee. An accurate clock also makes flight timestamps trustworthy.

### Reset and boot

- **NRST (reset):** pulling this pin low restarts the chip. It is also used by the debugger.
- **BOOT0:** read once at startup to decide where the chip starts running from.
  - **BOOT0 low:** runs your firmware from flash. This is normal operation and the state for flight.
  - **BOOT0 high:** runs a bootloader that ST built into the chip, which can load new firmware over USB (**DFU**, Device Firmware Upgrade). This is a safety net: even with broken firmware, the board can always be reprogrammed.
- A pin that decides how the chip boots must never float. It is held at a defined level so that vibration or noise can never send the rocket into the bootloader mid-flight.

### Pins and alternate functions

The STM32F405 has far more peripherals than it has pins, so each pin can be connected to one of several internal functions. This is called **pin multiplexing**, and each choice is an **alternate function**. A given pin might be usable as a plain GPIO, an SPI clock, a UART transmit line or a timer output, but only one at a time.

This makes **pin assignment a design decision**. Each peripheral is only available on certain pins, so the choices interact: putting the GPS UART on one set of pins might take away the only pins that could have carried a second SPI bus. Pin assignments also shape the PCB layout, because they decide which side of the chip each signal leaves from. Pins are planned against the datasheet's pin table, and checked with ST's pin-planning tool, before the schematic is finished.

## How the MCU talks to the rest of the board

Every other part on the board connects to the MCU through an interface: a set of agreed rules about which wires carry what, who speaks when, and how fast. This lesson only names them and explains why each part uses the one it does. **Lesson 3: Communication Protocols** covers how SPI, I²C and UART actually work, down to the pins.

| Interface | Used on this board for |
| --- | --- |
| **SPI** | IMU and barometer sharing one bus; the data flash on its own bus |
| **I²C** | Magnetometer |
| **UART** | GPS module |
| **SDIO** | microSD card |
| **USB** | Downloading data, power and firmware updates (including the DFU bootloader) |
| **SWD** | Programming and debugging during development |
| **GPIO and PWM** | Status LEDs; the buzzer tone, via a MOSFET switch |
| **ADC** | Battery voltage, through a voltage divider |

### Why each part uses the interface it does

- The **IMU and barometer** produce data the flight logic needs quickly and often, so they go on fast SPI.
- The **data flash** gets its own SPI bus because erasing and writing can keep the bus busy for a long time. If it shared a bus with the sensors, a slow write could delay a sensor reading at exactly the wrong moment.
- The **magnetometer** is read less often and its data is small, so slower two-wire I²C is enough. That saves pins.
- The **GPS** sends position updates on its own schedule, so a UART stream suits it.
- The **microSD card** holds a full copy of the flight log, so it gets SDIO's high throughput.

### How data gets into the MCU

The firmware has three ways to collect data from a peripheral:

- **Polling:** the core keeps asking "is there new data yet?" This is simple, but it wastes time and can miss the exact moment the data is ready.
- **Interrupts:** the peripheral or sensor signals the MCU on a pin the moment data is ready, and the core drops what it is doing to handle it. This gives precise timing.
- **DMA:** the transfer itself happens in hardware, and the core is only told when it is finished.

A good flight computer combines all three. For example, a sensor's data-ready interrupt can start a DMA transfer, so readings arrive on time without the core babysitting the bus.
