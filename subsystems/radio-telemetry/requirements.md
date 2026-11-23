# Radio and Telemetry: Requirements

**Owner(s):** Lucie\
**Status:** Draft\
**Last updated:** 2026-11-23

## 1. Scope

In a few sentences, describe what this subsystem is responsible for and what it is not. Name anything that might overlap with another subsystem.

-

## 2. FAR-OUT requirements

List every rule in the FAR-OUT Rules & Requirements document that affects this subsystem.

Source: FAR-OUT 2026-27 Rules & Requirements, Rev. 2026-08-31. AV-xxx IDs match the team's [FAR-OUT Rules Applicable to Avionics](https://claude.ai/artifact/2U62yFiEvesXc2QKdn2qTq) doc.

| Rule # | Requirement (quote or summary) | How it affects this subsystem |
|---|---|---|
| 5.5 (AV-204) | No more than two GPS systems operating in the 915 MHz or 440 MHz bands per stage. | Caps how many trackers can share those bands; a telemetry radio on 915/440 may count against the limit (open question for FAR-OUT). |
| 5.5.1 (AV-205) | Identify every transmitter frequency. | Keep a frequency list for every onboard and ground transmitter. |
| 5.5.1, 3.2.2 (AV-206) | Frequencies needing an FCC (HAM) license require a licensed team member at the launch site; otherwise the team cannot transmit on its assigned frequency. | Using 70 cm / 2 m HAM gear means a licensed member must attend competition. |
| 5.5.1 (AV-207) | Without a licensed member, use an ISM-band GPS, preferably 900 MHz. | Default to 900 MHz ISM unless the team commits to having a licensee on site. |
| 5.5.2 (AV-208) | Specify tracker frequencies after the final progress update; FAR-OUT may reassign channels to avoid overlap. | Radios must be retunable to another channel at short notice. |
| 5.5.1, 5.7.5 (AV-209) | Practice pulling telemetry off the rocket at ranges at least 40% beyond expected apogee. | Link budget, antennas and ground station must close at 1.4× apogee; the range test must be done and recorded. |
| 5.4.12, 5.7.5 (AV-212) | Practice controlling wireless GSE at 2,000 ft or more; wired GSE must be tested beyond 1,000 ft. Wireless GSE is strongly recommended. | The bunker-to-pad launch-control link needs a ≥2,000 ft range test. |
| 5.4.12, 5.7.5 (AV-213) | Mount GSE radio antennas on masts to clear shrubbery and ground reflections (20 ft in 5.4.12, 10 ft in 5.7.5). | Plan a ground antenna mast; mast height is an open question for FAR-OUT. |
| 5.4.12 (AV-214) | Prefer 440 MHz (70 cm) or 915 MHz for GSE radio; 2.4 GHz and 5.8 GHz channels will be shared under a band-sharing scheme. | Choose GSE and flight radio bands so they don't overlap each other or the GPS trackers. |

## 3. Questions

Write down as many questions as you can about what you need to know. No question is too basic. Keep adding to this list while you work on the other sections.

Example: *How many IMUs will we need?*

1.

## 4. Objectives

What this subsystem must do (not how it will do it). Note where each one comes from: a FAR-OUT rule number, or "team goal".
- State data 
	- calculated attitude 
	- rates 
- Meta data 
	- Voltage 
	- State (with timestamps)
	- Tank pressures 
	- Tank valve states 
- As much raw data as possible 
	- 2-3 IMUs (what freq?)
	- 1-2 Barometers (what freq?)
	- 1 GPS (what freq?)

| #   | Objective | Source (rule # or team goal) |
| --- | --------- | ---------------------------- |
| 1   |           |                              |

## 5. Interfaces

One block per connection to another subsystem. If the other subsystem owns the interface, link to its file instead of copying the details.

### Interface: Radio and Telemetry ↔ [other subsystem]

- **Connects to:** [other subsystem](../other-subsystem/requirements.md)
- **Direction:** what flows from whom to whom
- **What crosses:** power / data / signal / mechanical / RF / procedure
- **Details:** TBD
- **Owner:** TBD
- **Status:** TBD / Proposed / Agreed

<details>
<summary>Details to think about, by interface type</summary>

- **Electrical power:** voltage and tolerance, peak and average current, connector, switched or always on, battery life
- **Data:** protocol (I²C, SPI, UART, CAN), data rate, message content, update frequency, which side is the master
- **Discrete signals:** arm/fire lines, interrupts, logic level, active high or low, timing
- **Mechanical:** mounting points, available volume, mass, board size, vibration and shock
- **RF:** frequency band, antenna location, data rate, range
- **Software:** shared data structures, timing, who calls whom
- **Operational:** arming sequence, pad procedures, ground station connection, before launch and after recovery

</details>

## 6. Prior art and notes

What other teams, commercial flight computers, or past Blue Horizon rockets did for this subsystem. Add links.

-
