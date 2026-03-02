# ORVACT Technical Specification v1.7

## 1. System Overview
ORVACT is a modular gyroscopic propulsion platform. It consists of two counter-rotating fluid rotors with Tesla valve channels, an axial electromagnetic stator, and a static structural shaft.

## 2. Physical Principles
2.1. **Gyroscopic Precession**: Torque applied to the spin axis results in orthogonal motion (Euler's equations: τ = dL/dt = Ω × L).
2.2. **MHD Interaction**: Lorentz force (F = ∫(J × B) dV, where J = σ(E + v × B)) acts on conductive fluid in a magnetic field.
2.3. **Tesla Valve Effect**: Passive flow rectification via asymmetric geometry creates anisotropy in hydraulic resistance (diodicity target 2.0-3.0), damping secondary flows during precession.
2.4. **Momentum Conservation**: Counter-rotation cancels net reactive torque on the chassis (L_total ≈ 0).

## 3. Tier 0 Prototype Specifications (Garage)

### 3.1. Geometry
- Rotor Diameter: 2.0 m
- Rotor Thickness: 40 mm (center), 20 mm (periphery)
- Channel Configuration: Single toroidal loop
- Channel Radius: 0.9 m (from center)
- Channel Cross-section: 20 mm OD PVC, 16 mm ID → Area = 201 mm²
- Channel Length: 5.65 m (circumference at R=0.9m)
- Fluid Volume: 1.136 L (calculated: 5.65 × 0.201)
- Fill Ratio: 88% fluid (1.00 L), 12% gas buffer (0.14 L)
  - Note: Mercury mass at 1.00 L = 13.53 kg (density 13,534 kg/m³)

### 3.2. Tesla Valve Design (Tier 0 Simplified)
- Configuration: 3D-printed inserts (PETG/POM/polyurethane 70D) with asymmetric geometry
- Insert Length: 100 mm per segment
- Number of Segments: 4 (90 degree spacing)
- Geometry: Curved loop with sharp reverse turn (Tesla patent US1329559)
- Forward Pressure Drop: Minimal (gradual curves)
- Reverse Pressure Drop: 2-3× forward (flow separation)
- Installation: Slide inserts into PVC pipe before sealing ends
- **Note**: Tesla valves affect hydraulic resistance, not electrical conductivity.

### 3.3. Mass Properties
- Fluid Mass: 
  - Mercury: 13.53 kg (density 13,534 kg/m³, at 1.00 L fill)
  - Galinstan: 6.44 kg (density 6,440 kg/m³, at 1.00 L fill) — **Recommended for Tier 0**
  - Salt solution: 1.20 kg (density 1,200 kg/m³, at 1.00 L fill)
- Rotor Disk Mass (Aluminum 6061-T6 or FR4, 20mm): ~17 kg per disk
- Magnet Mass (N52, 50×20mm, 32 pcs): ~9.6 kg per rotor
- Total Rotating Mass: ~40 kg per rotor (with fluid and magnets)
- Stator Mass: ~5 kg
- Total System Mass: ~85 kg (with frame, both rotors)

### 3.4. Material Specifications
- Rotor Disk: 
  - Option A: Aluminum 6061-T6, 20mm plate — **requires radial slots to break eddy current loops**
  - Option B: Glass-epoxy (FR4), 20mm plate — non-conductive, eliminates eddy losses
  - WARNING: Aluminum amalgamates with mercury. If using mercury:
    - Use stainless steel 304 for rotor construction, OR
    - Coat all aluminum surfaces with epoxy barrier + rigorous leak testing
- Channel: PVC Schedule 40 or HDPE, 20mm OD, 16mm ID
  - Sealing: Metal end caps with O-ring compression (NOT epoxy for mercury)
- Shaft: Steel 1045, 25mm diameter (upgraded from 20mm for magnet load)
- Bearings: 
  - Minimum: 6202-2RS deep groove ball bearings (radial capacity 5.8 kN)
  - Recommended: 7202 angular contact pair (handles combined radial + axial precession loads)
- Fasteners: M6-M12 stainless steel (grade 304)

### 3.5. Operational Limits
- Max RPM: 600 (63 rad/s)
- Nominal RPM: 300 (31.4 rad/s)
- Run Time: 10-15 minutes continuous (thermal limit)
- Max Temperature: 80°C (bearing limit)
- Vibration Limit: 0.5 mm/s RMS (balanced)

### 3.6. Power and Control

#### 3.6.1. Motor Configuration
- TWO independent BLDC motors (one per rotor) for counter-rotation
- Motor: MXUS 3K, 1.5 kW peak, 48V (each)
- Controller: TWO VESC 6 Plus, 120A (one per motor), CAN-synchronized
- Gear Ratio: 3:1 (60T rotor pulley / 20T motor pulley)
- Output torque at rotor: ~14 N·m (at 300 RPM, 1.5 kW)

#### 3.6.2. Stator System
- Coils: 4-8 coils, 50-100 turns each, 1.5mm enamel copper wire
- Field Strength: 0.1-0.3 T (peak, air-core solenoid)
- Power: 500 W - 2 kW (field coils, copper)
  - Note: Copper coils limited to ~500 W continuous without aggressive cooling
  - 2 kW requires liquid cooling or short-duty operation
- Control: Independent polarity reversal for vector field modulation

#### 3.6.3. Sensors and DAQ
- MPU6050 (IMU, gyro+accel) — precession angle measurement
- ACS712 30A (current sensing, per motor)
- Optical tachometer (RPM feedback, per rotor)
- BMP280 (temperature/pressure, optional)
- Thrust sensor: Torsion pendulum or load cell, resolution ≤ 0.01 N
- Sampling: 100 Hz minimum for control, 1 kHz for vibration FFT

#### 3.6.4. Control Loops
- FAST_LOOP: 1 kHz (motor commutation, field PWM)
- SLOW_LOOP: 100 Hz (precession feedback, maneuver sequencing)
- MISSION_LOOP: 1 Hz (data logging, sweep progression)

#### 3.6.5. Discrete Control Protocol (Quantized Maneuvers)
To isolate signal from noise and account for fluid inertia, control inputs are discretized into fixed "Maneuver Quanta".

**Maneuver Quantum Structure:**
| Phase | Duration | Purpose |
|-------|----------|---------|
| Ramp Up | 200-500 ms | Smooth transition to target delta, prevent mechanical shock |
| Hold | 500-2000 ms | Allow fluid to traverse Tesla valve, build pressure differential |
| Ramp Down | 200-500 ms | Smooth return to nominal state |
| Settle | 1000 ms | Wait for fluid stabilization before next quantum |
| **Total Cycle** | **~2.5 s minimum** | Time per control quantum |

**Quantum Parameters (Tier 0):**
| Parameter | Minimum Step | Maximum Range | Increment Strategy |
|-----------|-------------|---------------|-------------------|
| Rotor ΔRPM | 0.1% (≈0.3 RPM) | ±10% (≈30 RPM) | Double step if no response after 3 quanta |
| Stator Field | 0.01 T | 0.3 T | Linear sweep: 0.01 T per test cycle |
| Precession Angle | 0.1° | 5.0° | Logarithmic: 0.1°, 0.2°, 0.5°, 1°, 2°, 5° |
| Phase Offset (between rotors) | 0.5° | 30° | Fixed increments of 2.5° |

**Sweep Test Protocol:**
1. Start at Minimum Step for target parameter.
2. Execute 3 consecutive Maneuver Quanta.
3. Measure response: precession angle, thrust sensor, vibration FFT.
4. If response > noise floor (3σ): record threshold, stop sweep.
5. If no response: increase to next increment, repeat.
6. Document full response curve: Input vs. Output.

**Safety Limits:**
- Never exceed Maximum Range without explicit authorization.
- Abort sweep if vibration > 2.5 mm/s RMS or temperature rise > 10°C/min.
- Always return to nominal state between increments (Settle Phase).

#### 3.6.6. Fluid Dynamics Timing Constraints
Control quantum duration must exceed hydraulic response time of fluid in channel.

**Tier 0 Calculations:**
- Channel length: 5.65 m
- Tesla valve unit length: 0.1 m
- Estimated flow velocity (300 RPM, centrifugal drive): 0.3-0.5 m/s
- Valve transit time: 200-300 ms (minimum)

**Constraint:** Hold Phase must exceed valve transit time. Start testing at 500 ms, scale to 1000 ms or 2000 ms if no response detected.

#### 3.6.7. Eddy Current Mitigation
Time-varying B-field induces eddy currents in conductive rotor disks → braking torque + resistive heating.

**Mitigation Options:**
1. Radial slots in aluminum disk (width ≥ 2 mm, depth ≥ 5 mm) to break current loops.
2. Use non-conductive structural material (FR4 glass-epoxy) for rotor disk; only fluid channel remains conductive.
3. Prefer pulsed field operation (5s on / 5s off) to limit average eddy losses.

### 3.7. Expected Performance (Classical Physics)
- Angular Momentum (per rotor): L = I × ω ≈ 408 N·m·s at 300 RPM
  - I_fluid = m × R² = 13.53 × 0.9² = 10.96 kg·m²
  - I_disk (approx) = 0.5 × m × R_eff² = 0.5 × 17 × 0.45² = 1.72 kg·m²
  - I_total (conservative) ≈ 13 kg·m²
- Precession Rate: Ω = τ / L → 1 N·m torque → ~0.14 deg/s precession at 300 RPM
- MHD Thrust (Atmosphere, Estimate): 0.1-1 N at B=0.1-0.3 T, ω=300 RPM (requires validation)
- Efficiency: Motor 85%, MHD interaction 5-15% (estimated)

### 3.8. Manufacturing Tolerances
- Rotor Balance: G6.3 ISO standard (static + dynamic)
  - Note: Balance MUST include fluid and magnets (dynamic balancing at operational speed required)
- Shaft Runout: < 0.1 mm TIR
- Bearing Fit: H7/h6 (interference fit on shaft)
- Channel Alignment: < 1 mm deviation from nominal radius
- Magnet Placement: < 2 mm positional tolerance, verified polarity
- Tesla Valve Inserts: < 0.5 mm fit tolerance in channel

## 4. Tier 1 Industrial Specifications (Atmospheric Vehicle)

### 4.1. Geometry
- Rotor Diameter: 10.0 m
- Rotor Thickness: 50 mm (center), 20 mm (periphery), tapered
- Channel Configuration: Single toroidal loop or 4 parallel loops
- Channel Radius: 4.5 m (from center)
- Channel Cross-section: 150 mm × 150 mm (square)
- Channel Length: 28.3 m (circumference at R=4.5m)
- Fluid Volume: 0.636 m³ per rotor
- Fill Ratio: 92% fluid (0.585 m³), 8% Argon buffer (0.051 m³)

### 4.2. Tesla Valve Design (Tier 1 Advanced)
- Configuration: Integrated wall channels with asymmetric geometry
- Channel Shape: Curved asymmetric loops (Tesla's original patent)
- Unit Cell Length: 300 mm
- Number of Unit Cells: 94 (full loop)
- Target Diodicity: 3.0 (validated via CFD for this geometry)
  - Note: Actual diodicity depends on Reynolds number. Expected range: 2.5-3.5 at design flow.
- Forward Pressure Drop: 0.1 bar at design flow
- Reverse Pressure Drop: 0.3 bar at design flow
- Material: Machined into liner or additively manufactured (metal/ceramic)

### 4.3. Mass Properties
- Fluid Mass (Mercury): 8,610 kg per rotor
- Liner Mass (Haynes 230, 12mm): 751 kg per rotor
- Structural Mass (Ti-6Al-4V center + CFRP rim): 8,530 kg per rotor
- External Bandage (CFRP): 201 kg per rotor
- Stator Mass: 3,000-5,000 kg
- Total System Mass: 40,000-45,000 kg

### 4.4. Material Specifications
- Liner: Haynes 230 (Ni-Cr-W-Mo superalloy), 12mm thickness
- Coating: SiC CVD, 50 micron thickness
- Structure (Center): Ti-6Al-4V (Grade 5), 40-60mm thickness
- Structure (Rim): CFRP T800, filament wound
- External Bandage: CFRP T800, 20mm × 200mm cross-section
- Shaft: High-strength steel alloy or Ti-6Al-4V tube, 150mm OD
- Bearings: Active Magnetic Bearings (AMB) + ceramic backup (Si3N4)
- Coils: Copper Cu-OFHC or HTS YBCO tapes (optional)

### 4.5. Operational Limits
- Nominal RPM: 380 (40 rad/s)
- Max RPM: 600 (63 rad/s, emergency)
- Centrifugal Pressure: 14.6 MPa (at 40 rad/s)
- Max Temperature: 200°C (nominal), 350°C (emergency shutdown)
- Gas Buffer Pressure: 1.2 atm (cold) to 3.0 atm (hot, 200°C)
- Design Life: 10,000 hours (liner replacement interval)
- Safety Factor: 3.0 (on yield strength)

### 4.6. Power and Control
- Primary Power: Gas turbine 2 MWe electric (e.g., Capstone C2000)
- Buffer: Supercapacitors 50 MJ
- Reserve: Li-ion battery 200 kWh
- Motor Power: 4-5 MW peak (MHD mode)
- Stator Power: 500 kW - 2 MW (field coils)
  - Copper coils: Limited to ~500 kW continuous (liquid cooling required)
  - HTS coils (YBCO): Enable 2 MW+ fields with cryogenic cooling (77 K)
  - Recommendation: Start with copper at 500 kW, upgrade to HTS for high-thrust modes
- Cooling: Liquid cooling (stator), passive convection (rotors)
- Control System:
  - FAST_LOOP: 1 ms (AMB stabilization, field control)
  - SLOW_LOOP: 100 ms (navigation, mode selection)
  - MISSION_LOOP: 1 s (trajectory planning)
- Sensors:
  - Fiber Bragg Grating (FBG) strain gauges
  - Eddy current displacement sensors (AMB)
  - High-resolution encoders (position)
  - Temperature sensors (RTD, thermocouples)
  - Pressure transducers (gas buffer)
  - Vibration sensors (accelerometers)

### 4.7. Expected Performance
- Stored Kinetic Energy: 362 MJ (total, 2 rotors)
- Angular Momentum: 18.1 MN·m·s (total)
- MHD Thrust (Atmosphere): Up to 100 kN (theoretical max)
- Specific Impulse (MHD): 500-1000 s (estimated)
- Vacuum Thrust (Hypothetical): Up to 20 kN (requires validation)
- Power-to-Thrust Ratio: 40-50 kW/kN (MHD mode)

### 4.8. Safety and Containment
- Primary Containment: Welded Haynes 230 liner with SiC coating
- Secondary Containment: CFRP external bandage (hoop strength)
- Design Basis: Containment maintained under:
  - 500 m free-fall impact
  - 10 g lateral acceleration
  - 350°C internal temperature
- Emergency Systems:
  - Mechanical brakes (disk type, 2 independent systems)
  - Active magnetic damping
  - Emergency gas venting (filtered)
  - Automatic shutdown on:
    - Vibration > 5.0 mm/s (emergency stop)
    - Vibration > 2.5 mm/s (warning, reduce load)
    - Temperature > 300°C
    - Pressure > 5 atm
    - Bearing fault detection
- Maintenance:
  - Liner inspection: Every 500 hours (ultrasonic thickness)
  - Liner replacement: Every 2,000 hours (planned)
  - Bearing inspection: Every 1,000 hours
  - Rotor swap time: 4-8 hours (modular design)

## 5. Tier 2 Lunar Mission Specifications

### 5.1. Key Differences from Tier 1
- Diameter: 8.0-10.0 m (optimized for mass)
- Fluid Mass: 10,000-15,000 kg per rotor (increased margin)
- Liner Material: Monolithic SiC ceramic (no chemical reaction with Hg)
- Gas Buffer: 12% volume (Argon, accounting for -100°C to +300°C range)
  - Thermal range: -100°C to +300°C (ΔT = 400 K)
  - Pressure variation (ideal gas, constant volume):
    - At 20°C (293 K): 1.2 atm (fill condition)
    - At -100°C (173 K): P = 1.2 × (173/293) = 0.71 atm
    - At +300°C (573 K): P = 1.2 × (573/293) = 2.35 atm
  - Design pressure: 3.0 atm (factor of 1.3 on max expected)
  - Liner yield strength (Haynes 230): 310 MPa at 300°C → sufficient margin
- Power Source: Kilopower nuclear reactor 1-10 kWe + solar panels (reserve)
- Heat Rejection: Droplet radiators 100-200 m²
- Crew: 1-2 persons in suits or light capsule
- Mission Duration: 7-14 days
- Delta-V: 3.2 km/s (LEO to lunar surface), 2.7 km/s (return)

### 5.2. Vacuum Adaptations
- No atmospheric MHD thrust (no working medium)
- Reliance on:
  - Gyroscopic precession for attitude control
  - Gravitomagnetic thrust (if validated)
  - Reserve ion thrusters (Isp 3000-5000 s)
- Thermal Management:
  - Radiative cooling only (no convection)
  - Multi-layer insulation (MLI)
  - Heat pipes for internal distribution

## 6. Tier 3 Interstellar Probe Specifications

### 6.1. Key Parameters
- Diameter: 4.0-6.0 m (mass optimization)
- Fluid Mass: 20,000-30,000 kg per rotor (max inertia)
- Liner Material: ZrC or CMC (ceramic matrix composite), 50,000+ hour life
- Gas Buffer: 15% volume (Ar/He mix, -150°C to +400°C range)
- Power Source: Nuclear reactor 10 MWe (thermal 30-50 MW)
- Heat Rejection: Droplet radiators 500-1000 m² (liquid metal, 800-1000 K)
- Payload: 50 kg scientific instruments
- Mission Duration: 40-50 years (to Proxima Centauri)
- Cruise Velocity: 0.1c (30,000 km/s) — HYPOTHETICAL
  - Requires validation of gravitomagnetic coupling effect
  - If classical physics only: Reserve ion thrusters (Isp 3000-5000 s) for trajectory corrections
  - Energy requirement for 0.1c (classical): E = 0.5 × m × v² = 0.5 × 100,000 kg × (3e7 m/s)² = 4.5e19 J (~10,000 TWh) — not feasible with known propulsion
  - If gravitomagnetic effect confirmed: Thrust scales with ω² × B² × geometry, enabling continuous acceleration without propellant
- Communication: Laser 100W-1kW + 3-5m deployable antenna
- Data Rate: 1-10 bits/sec at 4.24 ly distance

### 6.2. Autonomous Systems
- AI-based decision making (4.24 year communication delay)
- Self-diagnostics and reconfiguration
- Redundant systems (triple modular redundancy)
- Radiation-hardened electronics
- Micrometeoroid shielding (Whipple shield + magnetic deflection)

## 7. Tesla Valve Calculations (Detailed)

### 7.1. Design Equations
Tesla valve diodicity (Di) = (ΔP_reverse) / (ΔP_forward)

For asymmetric curved channel:
- Forward flow: Gradual curves, minimal separation
- Reverse flow: Sharp turns, flow separation, recirculation zones

Pressure drop (Darcy-Weisbach):
ΔP = f × (L/D) × (ρ × v² / 2)

Where:
- f = friction factor (function of Re and geometry)
- L = channel length
- D = hydraulic diameter
- ρ = fluid density
- v = flow velocity

### 7.2. Tier 0 Valve Parameters
- Channel: 16 mm ID PVC pipe
- Insert Material: PETG, POM, or polyurethane 70D (3D printed or machined)
- Insert Geometry: 
  - Forward path: Gradual 90° curve, radius 50 mm
  - Reverse path: Sharp 90° turn with recirculation pocket
- Unit Cell Length: 100 mm
- Number of Cells: 4 (spaced 90°)
- Expected Diodicity: 2.0-2.5 (simplified geometry)
- CFD Validation: Recommended (OpenFOAM, simpleFoam)

### 7.3. Tier 1 Valve Parameters
- Channel: 150mm × 150mm square
- Unit cell: 300mm length, asymmetric loop
- Number of cells: 94 (full 28.3m loop)
- Target Diodicity: 3.0-5.0 (optimized geometry)
- CFD Validation: Required (ANSYS Fluent / OpenFOAM)

## 8. Manufacturing Processes

### 8.1. Tier 0 (Garage)
- Rotor disks: CNC cutting or waterjet from 20mm Al plate or FR4
- Channel: Off-the-shelf PVC/HDPE pipe, glued/welded
- Shaft: Turned steel rod, 25mm diameter
- Bearings: Press-fit 6202-2RS or 7202 angular contact pair
- Assembly: Hand tools, torque wrench
- Balancing: Static balancing on knife-edges, dynamic with vibration sensor at operational speed

### 8.2. Tier 1 (Industrial)
- Liner: 
  - Method: Welded Haynes 230 plates or spun tube
  - Machining: CNC milling of Tesla valve channels
  - Coating: CVD SiC deposition (50 micron)
  - Inspection: Ultrasonic thickness, dye penetrant
- Structure:
  - Center: Forged Ti-6Al-4V, CNC machined
  - Rim: CFRP filament winding (automated)
- Bandage: CFRP prepreg layup + autoclave cure
- Shaft: Seamless Ti-6Al-4V tube, precision ground
- Bearings: Active magnetic bearing system (commercial supplier)
- Assembly: Clean room, laser alignment, dynamic balancing to G2.5

## 9. Testing Protocols

### 9.1. Tier 0 Tests
1. Static Balance Test (empty): < 1 gram imbalance at 1m radius
2. Fluid Fill Test: Fill horizontally, rotate disk slowly to distribute gas buffer evenly
3. Seal Integrity Test: Pressure test channel at 1.5 atm with soapy water
4. Run-in Test: 1 hour at 100 RPM (empty), monitor temperature/vibration
5. Dynamic Balance Test (with fluid): 
   - Fill channel to 88-92%
   - Run at 50-100 RPM
   - Measure vibration, add correction weights
   - Target: < 0.5 mm/s at 300 RPM
6. Precession Test: Apply known torque, measure orthogonal response
7. MHD Test: Energize stator coils, measure current change, thrust (if any)
8. Sweep Test Protocol (per 3.6.5): Characterize system response to quantized control inputs
9. Endurance Test: 10 cycles of 10 minutes at 300 RPM

### 9.2. Tier 1 Tests
1. Hydrostatic Test: Liner at 2x operating pressure (30 MPa)
2. Spin Test: Rotor in vacuum chamber, ramp to 600 RPM
3. Thermal Test: Operating temperature cycle (-20°C to +200°C)
4. MHD Thrust Test: Measure thrust in atmospheric chamber
5. Precession Control Test: Closed-loop attitude control
6. Endurance Test: 100 hours continuous operation
7. Failure Mode Test: Controlled bearing failure, verify containment

## 10. Assumptions and Limitations

10.1. **Classical Physics**: MHD and gyroscopic effects are based on established electrodynamics (Faraday, Ampere, Lorentz) and rigid body mechanics (Euler).

10.2. **Hypothetical Effects**: Vacuum thrust via gravitomagnetic coupling (analogous to Lense-Thirring effect) is unproven and requires experimental validation (Tier 0-1 primary objective).

10.3. **Material Properties**: All material data from manufacturer specifications at room temperature. High-temperature derating factors applied (0.7 at 200°C for Haynes 230).

10.4. **Scalability**: Performance estimates for Tier 1+ are theoretical and subject to CFD/FEA validation. Reynolds number scaling may affect MHD efficiency.

10.5. **Safety Margins**: Factor of 3.0 on yield strength for rotating components (aerospace standard). Factor of 2.0 for pressure containment.

10.6. **Mercury Compatibility**: Mercury amalgamates with aluminum, copper, and many metals. Use only with:
- Stainless steel (304, 316)
- Titanium
- HDPE, PTFE, PVC (short-term)
- Coated surfaces (epoxy barrier)

10.7. **Dynamic Balancing**: Fluid redistribution during rotation changes mass distribution. Static balance is insufficient. Dynamic balancing at operational speed is required.

10.8. **Counter-Rotation Drive**: Independent motor control is required for true counter-rotation. Single-motor solutions require mechanical reversal (planetary gearbox, belt cross).

10.9. **Scalability Constraints**
- Stress scaling: Centrifugal stress σ ∝ ρ × ω² × R²
  - Doubling radius quadruples stress at same ω
  - Tier 3 (smaller radius) enables higher ω without material failure
- Mass scaling: m ∝ R³ for geometric similarity
  - Tier 3 mass optimized for launch constraints, not maximum thrust
- Thermal management: Vacuum operation requires radiative cooling only
  - Heat rejection ∝ T⁴ × area (Stefan-Boltzmann)
  - Tier 3 droplet radiators enable high heat flux at low mass

10.10. **Mercury Mass Clarification**
- Mercury mass at 88% fill (1.00 L): 13.53 kg
- Use stainless steel rotor or epoxy-coated aluminum if mercury is employed
- PVC/HDPE channels acceptable for short-term testing; metal liners required for Tier 1+

10.11. **Eddy Current Mitigation**
- Time-varying magnetic fields induce eddy currents in conductive rotor disks
- Mitigation: radial slots in aluminum disks OR use non-conductive structural material (FR4)
- Pulsed field operation reduces average eddy losses

10.12. **Thrust Mechanism Clarification** (Updated v1.7)
System Architecture:
Rotors (fluid-filled disks): Counter-rotating at 300-600 RPM inside stationary chassis
Chassis: Does NOT rotate; houses stator coils, bearings, structure
Shaft: Stationary structural spine
Fluid: Contained within sealed rotor channels; no mass ejection
Atmospheric Mode:
MHD force acts on contained conductive fluid (Hg/Galinstan) via Lorentz force (F = ∫(J × B) dV).
Force transfers: Fluid → channel walls → rotor disk → bearings → stationary chassis.
Precession: Applied torque on rotor axis creates orthogonal reaction force on bearings.
External momentum exchange:
On test stand: Force measured at mounting points (internal force transfer, not propulsion)
In flight: Chassis interacts with air via aerodynamic surfaces during precession maneuvers
No ionization, no plasma, no mass ejection required
Important Distinction:
Tier 0 tests measure force transfer through structure (bearing loads, chassis reaction)
This is NOT net propulsion in a closed system
Atmospheric propulsion requires external momentum exchange (aerodynamic interaction with air)
If no external interaction exists, measured force is internal load only
Vacuum Mode:
Gravitomagnetic coupling hypothesis (unproven, requires validation)
If unconfirmed: defaults to conventional ion propulsion (mass ejection required)
Conservation of Momentum:
Upheld in all cases
Atmospheric: Momentum exchanged via aerodynamic interaction (chassis surfaces + air)
Vacuum (if gravitomagnetic exists): Momentum exchange with gravitational field (hypothetical)
Closed system with no external interaction: No net thrust (Newton's 3rd law)

10.13. **Known Hypotheses Requiring Experimental Validation**
1. Tesla valve anisotropy effect on pressure distribution in rotating channel — untested
2. Asymmetric pressure from flow rectification may affect bearing loads; thrust implications unknown
3. Dynamic synchronization of precession + flow + field modulation — control bandwidth unknown
4. Net thrust in vacuum — explicitly hypothetical, requires Tier 2 validation
All hypotheses marked above will be tested in Tier 0-2. Null results will be published.

10.14. **Architectural Principle: Discretized Control of Nonlinear Fluid Dynamics**
1. All control inputs are quantized into fixed-duration maneuvers (Maneuver Quanta).
2. Quantum duration exceeds hydraulic response time of working fluid (Hold Phase ≥ valve transit time).
3. Input amplitude begins at minimum detectable step (0.1% RPM, 0.01 T).
4. Amplitude scales upward only after null response (3 repetitions, sweep protocol).
5. All quantum responses are logged for digital twin training and system identification.

Rationale:
- Prevents resonant accumulation of vibrational energy
- Enables system identification via step response analysis
- Creates structured dataset for ML-based control optimization
- Protects mechanical integrity during experimental phase

## 11. Revision History
v1.0 (2026-03-01): Initial specification release.
v1.1 (2026-03-01): Added Tesla valve details, Tier 0-3 nomenclature, manufacturing processes, testing protocols.
v1.2 (2026-03-01): Corrected fill ratio, angular momentum calculation, stator power units, vibration thresholds, gas buffer thermal analysis, scalability constraints.
v1.3 (2026-03-01): Added discrete control protocol, eddy current mitigation, bearing selection notes, mercury compatibility warnings.
v1.4 (2026-03-01): Added minimal viable perturbation protocol, sweep testing methodology, phase control options.
v1.5 (2026-03-01): Integrated fluid dynamics timing constraints, architectural principle #1, clarified thrust mechanisms, updated Tesla valve material guidance.
v1.7 (2026-03-02): 10.12
---

**Author**: ORVACT Collective (dz9ikx)  
**Repository**: https://github.com/dz9ikx/ORVACT  
**License**: CC-BY-SA 4.0 (docs), CERN-OHL-S-2.0 (hardware), GPL-3.0 (software), ORVACT Open Humanity License v1.0 (anti-patent terms)

*"From Garage to Stars — One Validated Step at a Time"*
