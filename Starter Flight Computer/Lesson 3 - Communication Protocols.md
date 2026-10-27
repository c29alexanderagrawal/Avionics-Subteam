# Lesson 3: Communication Protocols

*Template: headers and main concepts only. Each section will be written out in full later. Builds on Lesson 2's introduction to the MCU; Lessons 4 and 5 put this into practice in CubeMX.*

## What you should be able to explain after this lesson

- How SPI, I²C and UART move bits between the MCU and the board's parts.
- What each wire does, and what is happening electrically on the pins.
- The configuration options each protocol offers, and how to choose between them from a datasheet.
- Why the board uses the configurations it does, and what breaks when a setting is wrong.

## 1. Ideas shared by every protocol

- Controller and peripheral: who starts a conversation and who answers.
- Synchronous versus asynchronous: a shared clock wire, or an agreed speed.
- Serial bits on a wire: logic levels, bit order (MSB or LSB first), and what a clock edge means.
- Talking in registers: writing a setting, reading a result, burst reads of several bytes.
- Sharing a bus: selecting one device out of several.
- Reading a timing diagram in a datasheet.

## 2. How the pins work

- What a GPIO pin is inside the chip: input buffer, output driver, optional pull-up or pull-down.
- **Output types:** push-pull versus open-drain, and why each protocol needs the one it uses.
- **Pull-ups and pull-downs:** internal versus external resistors, and when a line must never float.
- **Output speed:** slew rate, edge ringing and noise, and why faster is not always better.
- **Alternate functions:** handing a pin to a peripheral instead of driving it from code.
- Logic levels: 3.3 V parts on a 3.3 V bus, and what happens if levels don't match.

## 3. SPI

- The four wires: clock (SCK), controller out (MOSI), controller in (MISO), chip select (CS).
- How a transfer works: a shift register on each side, full duplex, one bit per clock edge.
- **Clock polarity and phase (CPOL/CPHA):** the four SPI modes, and finding the right one in a sensor datasheet.
- Clock speed: each device's maximum, and the shared-bus rule that the slowest device sets the pace.
- Chip select: active low, hardware versus software control, and timing around a transfer.
- Several devices on one bus: shared clock and data lines, one CS each, and unselected devices releasing MISO.
- Pin configuration: push-pull outputs for SCK, MOSI and CS; CS idling high; MISO as input.
- **On this board:** the IMU and barometer sharing one bus, the data flash on its own bus, and why.
- Common failures: wrong mode, clock too fast, floating CS at power-up, two devices selected at once.

## 4. I²C

- The two wires: clock (SCL) and data (SDA).
- Open-drain lines: devices only pull low, resistors pull high, so nobody can short the bus.
- Pull-up resistor sizing: bus capacitance, rise time, and why the internal pull-ups are too weak.
- How a transaction works: start condition, 7-bit address, read/write bit, ACK/NACK, data bytes, stop.
- Register access: write the register address, then a repeated start to read.
- Speed modes: standard (100 kHz) and fast (400 kHz).
- Clock stretching, and a bus stuck low after a reset.
- Pin configuration: open-drain alternate function, external pull-ups.
- **On this board:** the magnetometer, and why I²C is fast enough for it.
- Common failures: missing pull-ups, wrong address (7-bit versus 8-bit notation), a hung bus.

## 5. UART

- The two wires: transmit (TX) and receive (RX), crossed between devices, plus a shared ground.
- How a frame works: idle high, start bit, data bits, optional parity, stop bit.
- Baud rate: both sides must agree, and how clock error turns into garbled bytes.
- Frame format options: 8N1 and the alternatives.
- Streaming data: the receiver must be ready for bytes that arrive on the sender's schedule.
- Pin configuration: TX as a push-pull alternate function, RX as an input with a pull-up.
- **On this board:** no part uses UART on v1. It is still worth knowing, because GPS modules and radios usually connect this way, and a spare UART makes a simple debug console.
- Common failures: TX/RX not crossed, mismatched baud rate, dropped bytes when the MCU is busy.

## 6. Comparing the three

- Speed, wire count, number of devices, and duplex side by side.
- Choosing a protocol for a new part.

## 7. Debugging communication

- Reading an ID register (WHO_AM_I) as the first test of every bus.
- What a logic analyzer shows, and decoding SPI, I²C and UART traffic.
- What an oscilloscope adds: rise times, ringing and noise.

## Discussion questions

- *To be written.*
