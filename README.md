# ORVACT Project

**Orbital Rotational Vector Axial Counter-rotating Torque**

*Open-source research platform for gyroscopic propulsion and vector thrust control.*

[![License: CC-BY-SA 4.0](https://img.shields.io/badge/Docs-CC--BY--SA--4.0-blue.svg)](https://creativecommons.org/licenses/by-sa/4.0/)
[![License: CERN-OHL-S-2.0](https://img.shields.io/badge/Hardware-CERN--OHL--S--2.0-orange.svg)](https://ohl.cern.ch/)
[![License: GPL-3.0](https://img.shields.io/badge/Software-GPL--3.0-green.svg)](https://www.gnu.org/licenses/gpl-3.0.html)
![Status: Experimental](https://img.shields.io/badge/Status-Experimental-yellow)

---

##  Abstract

ORVACT explores the integration of **counter-rotating fluid-filled rotors**, **magnetohydrodynamic (MHD) interaction**, and **gyroscopic precession** for propellantless attitude control and translational thrust. The system utilizes conductive fluids (mercury or simulants) in Tesla valve channels, energized by axial magnetic fields, to generate force without expelling mass.

This is an **open-source research platform** — from garage-scale prototypes to interstellar probe concepts — built on reproducible experimentation and shared knowledge.

---

## Key Features

- **Counter-rotating fluid rotors** — Reactive torque cancellation via dual rotors with mercury/conductive fluid
- **MHD thrust generation** — Lorentz force from induced currents in axial magnetic field
- **Vector control** — Gyroscopic precession enables maneuvering without aerodynamic surfaces
- **Tesla valve channels** — Passive flow rectification and slosh damping (3D-printable inserts)
- **Modular architecture** — Static central shaft with integrated cooling, designed for incremental validation
- **Open Humanity licensing** — No patents on core architecture; all improvements remain open

---

##  Development Tiers

| Tier | Name | Purpose | Status |
|------|------|---------|--------|
| **Tier 0** | Garage Prototype | Proof of physics (precession, MHD in atmosphere) | 🟢 **Ready to build** |
| **Tier 1** | Atmospheric Vehicle | Commercial transport, VTOL, hover (10 m diameter) | 📐 Conceptual |
| **Tier 2** | Lunar Mission | Piloted lunar landing/return (vacuum validation) | 🔭 Horizon |
| **Tier 3** | Interstellar Probe | Autonomous probe to Proxima Centauri | ✨ Conditional |

> **Note:** Tier 3 is contingent on experimental validation of gravitomagnetic coupling in Tiers 0–2. If unconfirmed, architecture defaults to conventional ion propulsion.

---

##  Architecture Overview

```
[ Upper Rotor ]  ← Counter-rotating, fluid-filled, Tesla valve channels
       |
[ Upper Stator ] ← Axial field coils (vector controllable)
       |
[ Static Shaft ] ← Structural spine, houses cooling/power/data
       |
[ Lower Stator ] ← Axial field coils (vector controllable)
       |
[ Lower Rotor ]  ← Counter-rotating, fluid-filled, Tesla valve channels
       |
[ Power/Control Bay ] ← Turbine, converters, avionics, thermal system (Tier 1+)
```

- **Configuration:** Dual counter-rotating disks, axial stator(s), static central shaft
- **Working Fluid:** Mercury (industrial), Galinstan or conductive simulants (prototype)
- **Channel Design:** Tesla valve geometry for passive flow rectification
- **Control:** Vector thrust via magnetic field modulation and gyroscopic precession
- **Cooling:** Passive convection (atmosphere) / Active radiator (vacuum)

---

##  Repository Structure

```
##  Project Files

| File | Description |
|------|-------------|
| `SPECIFICATION.md` | Complete technical spec (Tiers 0–3) |
| `BUILD_GUIDE_TIER0.md` | Step-by-step garage build |
| `ORVACT_Tier1_Spec_v1.md` | Atmospheric vehicle concept |
| `ORVACT_Tier2_Spec_v1.md` | Lunar mission concept |
| `ORVACT_Tier3_Investor_Prospectus.md` | Interstellar probe (investor focus) |
| `CONTRIBUTING.md` | How to help |
| `LICENSE` | Open Humanity License terms |
```

---

##  Getting Started

### Build the Tier 0 Prototype

1. Read the [ORVACT_Tier0_BUILD_GUIDE](./ORVACT_Tier0_BUILD_GUIDE.md)
2. Follow safety protocols for high-speed rotation and conductive fluids
3. Run the test sequence and share your data

### Contribute

We welcome contributions from engineers, physicists, makers, and dreamers:

-  **Experimental data** — Share your test results (even null results!)
-  **Hardware improvements** — Submit CAD modifications via Pull Request
-  **Firmware** — Improve control algorithms or add sensors
-  **Documentation** — Fix errors, add translations, write tutorials
-  **Ideas** — Open a Discussion for theoretical questions

See [CONTRIBUTING.md](./CONTRIBUTING.md) for full guidelines.

---

##  License & Philosophy

This project is released under an **Open Humanity** model:

| Component | License |
|-----------|---------|
| Documentation | [CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) |
| Hardware Designs | [CERN-OHL-S-2.0](https://ohl.cern.ch/) |
| Software/Firmware | [GPL-3.0](https://www.gnu.org/licenses/gpl-3.0.html) |
| **Additional Terms** | **[ORVACT Open Humanity License v1.0](./LICENSE#additional-terms)** |

**Core commitments:**
-  No patenting of the core architecture
-  All improvements must remain open
-  Knowledge should serve all of humanity

*We believe propulsion technology that could enable interstellar exploration should not be locked behind patents.*

---

##  Disclaimer

This project is for **research and educational purposes only**. The authors provide no warranty, express or implied. Users are solely responsible for:

- Safety compliance when handling hazardous materials (mercury, high voltage)
- Proper containment of high-speed rotating machinery
- Adherence to local laws and regulations

See [SAFETY.md](./docs/SAFETY.md) for detailed risk assessment and mitigation protocols.

---

##  Authors & Acknowledgments

**Author:** [@dz9ikx](https://github.com/dz9ikx) and the ORVACT Collective

This project exists because of open-source hardware, open science, and the belief that the future should be built together.

---

##  Contact & Community

- **GitHub Issues:** Bug reports, feature requests
- **GitHub Discussions:** Theory, ideas, collaboration
- **Pull Requests:** Your contributions here

---

***"From Garage to Stars — One Validated Step at a Time"** *

© 2026 ORVACT Collective
