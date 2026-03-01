# ORVACT Tier 1 Specification — Atmospheric Vehicle

## 1. Overview
**Purpose:** Commercial atmospheric transport platform for cargo (100–500 kg) and/or passenger (2–4 persons) missions. Vertical takeoff/landing (VTOL), hover, and agile maneuvering in dense atmosphere (0–5 km altitude).

**Design Philosophy:** Modular, serviceable, safety-first architecture. Built on validated Tier 0 physics, scaled with industrial materials and manufacturing.

**Status:** Conceptual design. Requires Tier 0 validation before detailed engineering.

---

## 2. Physical Principles (Theory)

### 2.1. Core Propulsion Mechanisms
| Mechanism | Physical Basis | Role in Tier 1 |
|-----------|---------------|----------------|
| **MHD Thrust** | Lorentz force: $F = \int (J \times B) dV$, where $J = \sigma(E + v \times B)$ | Primary atmospheric thrust. Conductive fluid (Hg) + axial magnetic field + induced current → body force on fluid → momentum transfer to vehicle. |
| **Gyroscopic Precession** | Euler's equation: $\tau = dL/dt = \Omega \times L$ | Attitude control and vector thrust. Applied torque → orthogonal precession → vehicle reorientation without aerodynamic surfaces. |
| **Counter-Rotation** | Conservation of angular momentum: $L_{total} = L_1 + L_2 \approx 0$ | Cancels net reactive torque on chassis. Enables stable hover and eliminates need for tail rotor/anti-torque system. |
| **Tesla Valve Damping** | Passive flow rectification via asymmetric geometry | Suppresses secondary flows and fluid slosh during precession maneuvers. Improves control authority and reduces parasitic losses. |

### 2.2. Scaling Laws (Tier 0 → Tier 1)
| Parameter | Scaling Relationship | Implication |
|-----------|---------------------|-------------|
| Thrust (MHD) | $F \propto V_{fluid} \times B^2 \times \omega \times R$ | 560× volume increase (1L → 0.636 m³) enables ~100 kN thrust at $B=1 T$, $\omega=40 rad/s$ |
| Stored Energy | $E \propto m \times R^2 \times \omega^2$ | Kinetic energy scales with $R^5$ for geometric similarity → 362 MJ at Tier 1 (vs 0.18 MJ Tier 0) |
| Centrifugal Stress | $\sigma \propto \rho \times \omega^2 \times R^2$ | Stress increases with $R^2$ → requires high-strength materials (Haynes 230, CFRP) at Tier 1 |
| Thermal Load | $Q \propto I^2R + hysteresis + eddy$ | Absolute heat load increases, but surface-area-to-volume ratio decreases → active cooling required |

---

## 3. Key Specifications (Target)

| Category | Parameter | Target Value | Notes |
|----------|-----------|--------------|-------|
| **Geometry** | Rotor diameter | 10.0 m | Fixed for MHD efficiency |
| | Channel cross-section | 150 × 150 mm | Square, optimized for flow + structural integrity |
| | Fluid volume (per rotor) | 0.636 m³ | 92% Hg, 8% Ar buffer |
| **Mass** | Fluid mass (Hg, per rotor) | 8,610 kg | Density 13,534 kg/m³ |
| | Structural mass (per rotor) | ~8,500 kg | Ti-6Al-4V center + CFRP rim |
| | Total vehicle mass | 40–45 t | Includes power, payload, systems |
| **Performance** | Nominal rotation speed | 380 RPM (40 rad/s) | Balance of thrust vs stress |
| | Max MHD thrust (atmosphere) | ≤ 100 kN | Theoretical, requires validation |
| | Payload capacity | 100–500 kg | Configurable cargo/passenger module |
| | Endurance (hover) | 2–4 hours | Limited by fuel/energy storage |
| **Power** | Primary source | Gas turbine, 2 MWe | e.g., Capstone C2000 or equivalent |
| | Peak power (MHD mode) | 4–5 MW | Supercapacitor buffer for transients |
| | Stator field power | 500 kW – 2 MW | Copper coils (liquid cooled) or HTS option |
| **Control** | Attitude authority | ±30° precession cone | Via differential torque + field modulation |
| | Response time (maneuver) | < 2 s | FAST_LOOP 1 ms, SLOW_LOOP 100 ms |
| **Safety** | Containment design basis | 500 m free-fall impact | No fluid release |
| | Liner service life | 2,000 hours | Planned replacement, not run-to-failure |
| | Vibration shutdown threshold | > 5.0 mm/s RMS | Two-level: warning at 2.5 mm/s |

---

## 4. Architecture Summary

```
[ Upper Rotor ]  ← Counter-rotating, fluid-filled, Tesla valve channels
       |
[ Upper Stator ] ← Axial field coils (vector controllable)
       |
[ Payload Module ] ← Removable cargo/passenger pod (static)
       |
[ Lower Stator ] ← Axial field coils (vector controllable)
       |
[ Lower Rotor ]  ← Counter-rotating, fluid-filled, Tesla valve channels
       |
[ Static Shaft ] ← Structural spine, houses cooling/power/data
       |
[ Power/Control Bay ] ← Turbine, converters, avionics, thermal system
```

**Key Subsystems:**
- **Propulsion:** Dual counter-rotating MHD rotors with independent speed control.
- **Attitude Control:** Precession via differential torque + stator field vectoring.
- **Thermal Management:** Liquid-cooled stator coils + passive rotor convection + optional heat pipes.
- **Power Architecture:** Turbine → DC bus → VESC motor drives + stator converters + supercapacitor buffer.
- **Containment:** Primary liner (Haynes 230 + SiC CVD) + secondary CFRP bandage + leak detection.
- **Service Model:** Modular rotor swap (4–8 hours), liner replacement at certified hub.

---

## 5. Development Vector (Roadmap)

### 5.1. Prerequisites (from Tier 0)
- [ ] Validate MHD thrust scaling (force vs B, ω, fluid properties)
- [ ] Confirm precession control authority at scale
- [ ] Demonstrate Tesla valve damping effect on fluid dynamics
- [ ] Verify containment integrity under dynamic loads

### 5.2. Tier 1 Development Phases
| Phase | Focus | Deliverable | Timeline (est.) |
|-------|-------|-------------|-----------------|
| **A. Detailed Design** | FEA/CFD, material selection, subsystem specs | Frozen CAD, BOM, safety case | 12–18 months |
| **B. Component Prototyping** | Liner fabrication, coil winding, AMB testing | Qualified subsystems | 12 months |
| **C. Integrated Ground Test** | Full-scale rotor spin, MHD thrust stand, thermal validation | Performance data, safety certification basis | 12–18 months |
| **D. Flight Prototype** | Tethered hover, free flight envelope expansion | Airworthy vehicle, operational procedures | 12 months |
| **E. Commercial Deployment** | Service hub setup, maintenance training, regulatory approval | Revenue operations | Ongoing |

### 5.3. Critical Technology Risks
| Risk | Mitigation |
|------|------------|
| MHD efficiency lower than predicted | Design margin on thrust; hybrid aerodynamic assist option |
| Liner fatigue under cyclic stress | Conservative S-N curves, NDT inspection protocol, modular replacement |
| Thermal runaway in stator coils | Redundant cooling loops, real-time temperature monitoring, derating logic |
| Control instability at high precession rates | Gain-scheduled controllers, hardware-in-loop simulation, envelope protection |

### 5.4. Long-Term Evolution (Tier 1 → Tier 2+)
- **Propulsion:** Add vacuum-capable mode (gravitomagnetic hypothesis validation) → enable Tier 2 lunar missions.
- **Power:** Integrate compact nuclear source (Kilopower-class) → extend endurance, enable high-altitude/long-duration.
- **Materials:** Transition to monolithic SiC liners → higher temperature tolerance, longer life.
- **Autonomy:** Full AI flight control + self-diagnosis → reduce crew requirements, enable remote operations.
- **Scalability:** Modular "cluster" architecture (multiple ORVACT units) → heavy-lift capability.

---

## 6. Open Challenges (Research Questions)

1. **MHD Efficiency in Turbulent Flow:** How does channel wall roughness, fluid compressibility, and secondary flow affect net thrust coefficient?
2. **Precession Control Bandwidth:** What is the maximum usable precession rate before fluid slosh or structural modes limit authority?
3. **Thermal-Electromagnetic Coupling:** How does coil temperature rise affect field strength and control linearity?
4. **Containment Probabilistic Risk:** What is the quantitative failure probability of the dual-containment system under realistic operational spectra?
5. **Scalability Limits:** At what radius does centrifugal stress force a transition to alternative rotor architectures (e.g., segmented, active support)?

---

## 7. License & Attribution
This specification is part of the ORVACT Project.  
Licensed under [CC-BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) (documentation), [CERN-OHL-S-2.0](https://ohl.cern.ch/) (hardware), [GPL-3.0](https://www.gnu.org/licenses/gpl-3.0.html) (software), and **ORVACT Open Humanity License v1.0** (anti-patent terms).  

© 2026 ORVACT Collective | Author: dz9ikx | Repository: [https://github.com/dz9ikx/ORVACT](https://github.com/dz9ikx/ORVACT)

---

**"From Garage to Stars"**
