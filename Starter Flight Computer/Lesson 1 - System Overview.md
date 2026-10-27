# Lesson 1: System Overview

## Mission

### What you should be able to explain after this section

- What this flight computer is for, and what it deliberately does not do.
- The questions the board has to answer about every flight.
- The phases of a flight, and how each one looks to the sensors.
- How the mission turns into design requirements, and why every part on the board traces back to one.

### What the board is

The starter flight computer is a **flight data recorder**. It rides in the av bay and observes: it measures the rocket's motion, altitude and orientation, works out what phase of flight the rocket is in, and saves everything to memory so it can be studied after recovery.

It does **not** control the flight. It fires no ejection charges and steers nothing. Recovery is handled by the motor's delay charge, exactly as it would be with no electronics on board. This is on purpose: a board that only observes can fail without endangering the rocket, which makes it the right first project for learning the hardware.

### Why record a flight at all

- **Know what actually happened.** Without data, a flight is a story people tell afterward. With data, it is a measurement.
- **Check the simulation.** Comparing measured altitude, velocity and acceleration against the OpenRocket prediction shows how good the model was, and tunes it for the next flight.
- **Diagnose failures.** Early deployment, a late ejection charge, a motor that underperformed, or a rocket that weathercocked hard all leave clear fingerprints in the data.
- **Build toward active avionics.** Detecting launch and apogee reliably is exactly the logic a deployment controller needs. Getting it right on a board that fires nothing is how you earn trust in it before it fires something.

### The questions the board must answer

Every design choice should help answer at least one of these:

1. **Did the rocket launch, and exactly when?**
2. **How hard did it accelerate, and when did the motor burn out?**
3. **How high did it go, and when did it reach apogee?**
4. **How was it oriented and rotating throughout the flight?**
5. **When did the ejection charge fire, and how fast did it descend?**
6. **Was the board itself healthy the whole time?** (battery voltage, sensor dropouts, logging gaps)

### The phases of flight

The board sorts the flight into phases so it knows how fast to log and what to expect next. Each phase has a physical signature the sensors can see.

| Phase | What is physically happening | What the sensors see |
| --- | --- | --- |
| **Pad** | Rocket sits on the rail, possibly for a long time before launch | About 1 g on the vertical axis, near-zero rotation, steady pressure |
| **Boost** | Motor is burning; typically only a second or two for an L1 motor | Large sustained acceleration along the rocket's axis, pressure falling quickly |
| **Coast** | Motor has burned out; rocket is still climbing on momentum and slowing | Axial acceleration drops sharply to negative (drag plus gravity), pressure still falling but more slowly |
| **Apogee** | Vertical velocity reaches zero; the motor's ejection charge fires near here | Pressure at its minimum, near-weightless readings, then a sharp shock spike from the ejection charge |
| **Descent** | Rocket falls under its parachute | Pressure rising at a fairly steady rate, swinging and tumbling on the gyro |
| **Landed** | Rocket is on the ground | Steady pressure again, a steady gravity vector in some random orientation |

Two ideas matter here. First, **no single sensor sees everything well**: the accelerometer is best at catching launch and burnout, the barometer is best at apogee, and the gyro and magnetometer track orientation. Second, **the transitions are the hard part**. Deciding that launch happened, and not just that someone bumped the rail, is a real engineering problem the firmware lesson will return to.

### From mission to requirements

The mission turns into concrete requirements, and each requirement explains why a part is on the board:

- **Never miss the launch.** The board must be powered, sampling and ready on the pad for as long as a launch delay lasts. This drives the battery capacity and the power budget.
- **Capture boost in detail.** Boost is short and violent, so the board has to sample fast and handle high acceleration without saturating. This drives the IMU choice and its measurement range.
- **Measure altitude accurately.** Apogee is the headline number of every flight. This drives the barometer.
- **Know the orientation.** This drives the gyro and the magnetometer.
- **Help find the rocket.** This drives the buzzer that lets you hear it in tall grass.
- **Never lose the data.** A hard landing or a battery that pops loose must not wipe the flight. This drives writing continuously to onboard flash, with the microSD card as a second copy.
- **Survive the flight.** Vibration, shock and the ejection charge's jolt drive part selection, connectors and mounting.
- **Be easy to work with on the ground.** USB for downloading data and updating firmware, SWD for debugging, LEDs and the buzzer for status without a screen.

The rest of this lesson follows the same thread: every component exists because the mission demands it, and every connection exists so that component can do its part of the job.
