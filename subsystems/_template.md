# [Subsystem name]: Requirements

**Owner(s):** -\
**Status:** Draft\
**Last updated:** YYYY-MM-DD

<!--
How to use this template:
- Copy this file into your subsystem folder as requirements.md, or fill in the one already there.
- Keep every heading below, in this order, so all subsystem files read the same way.
- "TBD" is a fine answer. Every TBD is also a question: add it to the Questions list.
- Keep this file to conclusions. Long research, datasheets and comparisons go in a notes/ folder
  next to this file, linked from here.
- Status is one of: Draft, In review, Agreed.
-->

## 1. Scope

In a few sentences, describe what this subsystem is responsible for and what it is not. Name anything that might overlap with another subsystem.

-

## 2. FAR-OUT requirements

List every rule in the FAR-OUT Rules & Requirements document that affects this subsystem.

| Rule # | Requirement (quote or summary) | How it affects this subsystem |
|---|---|---|
| | | |

## 3. Questions

Write down as many questions as you can about what you need to know. No question is too basic. Keep adding to this list while you work on the other sections.

Example: *How many IMUs will we need?*

1.

## 4. Objectives

What this subsystem must do (not how it will do it). Note where each one comes from: a FAR-OUT rule number, or "team goal".

| # | Objective | Source (rule # or team goal) |
|---|---|---|
| 1 | | |

## 5. Interfaces

One block per connection to another subsystem. If the other subsystem owns the interface, link to its file instead of copying the details.

### Interface: [this subsystem] ↔ [other subsystem]

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
