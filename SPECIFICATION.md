# ORVACT Technical Specification v1.1

## 1. System Overview
ORVACT is a modular gyroscopic propulsion platform. It consists of two counter-rotating fluid rotors with Tesla valve channels, an axial electromagnetic stator, and a static structural shaft.

## 2. Physical Principles
2.1. Gyroscopic Precession: Torque applied to the spin axis results in orthogonal motion (Euler's equations).
2.2. MHD Interaction: Lorentz force (F = q(v x B)) acts on conductive fluid in a magnetic field.
2.3. Tesla Valve Effect: Passive flow rectification without moving parts, damping secondary flows during precession.
2.4. Momentum Conservation: Counter-rotation cancels net reactive torque on the chassis.

## 3. Tier 0 Prototype Specifications (Garage)

### 3.1. Geometry
- Rotor Diameter: 2.0 m
- Rotor Thickness: 40 mm (center), 20 mm (periphery)
- Channel Configuration: Single toroidal loop
- Channel Radius: 0.9 m (from center)
- Channel Cross-section: 20 mm outer diameter, 16 mm inner diameter (PVC/HDPE pipe)
- Channel Length: 5.65 m (circumference at R=0.9m)
- Fluid Volume: 1.13 liters
- Fill Ratio: 92% fluid (1.04 L), 8% gas buffer (0.09 L)

### 3.2. Tesla Valve Design (Tier 0 Simplified)
- Configuration: Loop with asymmetric restrictions
- Restriction Type: Gradual taper (20mm to 12mm over 100mm)
- Placement: 4 equidistant points around loop
- Purpose: Dampen secondary flows during precession
- Pressure Drop Ratio: Forward:Reverse = 1:3 (target)

### 3.3. Mass Properties
- Fluid Mass: 
  - Mercury: 13.5 kg (density 13,534 kg/m3)
  - Galinstan: 7.3 kg (density 6,440 kg/m3)
  - Salt solution: 1.4 kg (density 1,200 kg/m3)
- Rotor Mass (Aluminum 6061-T6): ~15 kg per disk
- Stator Mass: ~5 kg
- Total System Mass: ~40 kg (with frame)

### 3.4. Material Specifications
- Rotor Disk: Aluminum 6061-T6, 20mm plate
- Channel: PVC Schedule 40 or HDPE, 20mm OD
- Shaft: Steel 1045, 20mm diameter, ground finish
- Bearings: 608ZZ skateboard bearings (2 per rotor)
- Fasteners: M6-M12 stainless steel (grade 304)

### 3.5. Operational Limits
- Max RPM: 600 (63 rad/s)
- Nominal RPM: 300 (31 rad/s)
- Run Time: 10-15 minutes continuous (thermal limit)
- Max Temperature: 80 C (bearing limit)
- Vibration Limit: 0.5 mm/s RMS (balanced)

### 3.6. Power and Control
- Motor: BLDC outrunner, 1.5-2 kW peak (e.g., MXUS 3K)
- Controller: VESC 6 Plus, 120A, FOC algorithm
- Power Supply: 48V 2000W PSU or LiFePO4 48V 20Ah
- Stator Coils: 4-8 coils, 50-100 turns each, 1.5mm enamel copper wire
- Stator Power: 12-24V 5-10A DC supply
- Sensors: 
  - MPU6050 (IMU, gyro+accel)
  - ACS712 30A (current sensing)
  - Optical tachometer (RPM feedback)
  - BMP280 (temperature/pressure, optional)
- Control Loop: 
  - FAST_LOOP: 1 kHz (motor commutation)
  - SLOW_LOOP: 100 Hz (stator control, data logging)

### 3.7. Expected Performance
- Gyroscopic Stiffness: ~465 N m s (at 300 RPM, Mercury)
- Angular Momentum: 465 N m s
- Precession Rate: 1 deg/s per 8 N m applied torque (at 300 RPM)
- MHD Thrust (Estimate): 0.1-1 N (atmospheric test, requires validation)
- Efficiency: Motor 85%, MHD interaction 5-15% (estimated)

### 3.8. Manufacturing Tolerances
- Rotor Balance: G6.3 ISO standard (static + dynamic)
- Shaft Runout: < 0.1 mm TIR
- Bearing Fit: H7/h6 (interference fit on shaft)
- Channel Alignment: < 1 mm deviation from nominal radius
- Coil Position: < 2 mm from design position

## 4. Tier 1 Industrial Specifications (Atmospheric Vehicle)

### 4.1. Geometry
- Rotor Diameter: 10.0 m
- Rotor Thickness: 50 mm (center), 20 mm (periphery), tapered
- Channel Configuration: Single toroidal loop or 4 parallel loops
- Channel Radius: 4.5 m (from center)
- Channel Cross-section: 150 mm x 150 mm (square)
- Channel Length: 28.3 m (circumference at R=4.5m)
- Fluid Volume: 0.636 m3 per rotor
- Fill Ratio: 92% fluid (0.585 m3), 8% Argon buffer (0.051 m3)

### 4.2. Tesla Valve Design (Tier 1 Advanced)
- Configuration: Integrated wall channels with asymmetric geometry
- Channel Shape: Curved asymmetric loops (Tesla's original patent)
- Unit Cell Length: 300 mm
- Number of Unit Cells: 94 (full loop)
- Forward Pressure Drop: 0.1 bar at design flow
- Reverse Pressure Drop: 0.3 bar at design flow
- Diodicity: 3.0 (target)
- Material: Machined into liner or additively manufactured

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
- External Bandage: CFRP T800, 20mm x 200mm cross-section
- Shaft: High-strength steel alloy or Ti-6Al-4V tube, 150mm OD
- Bearings: Active Magnetic Bearings (AMB) + ceramic backup (Si3N4)
- Coils: Copper Cu-OFHC or HTS YBCO tapes (optional)

### 4.5. Operational Limits
- Nominal RPM: 380 (40 rad/s)
- Max RPM: 600 (63 rad/s, emergency)
- Centrifugal Pressure: 14.6 MPa (at 40 rad/s)
- Max Temperature: 200 C (nominal), 350 C (emergency shutdown)
- Gas Buffer Pressure: 1.2 atm (cold) to 3.0 atm (hot, 200C)
- Design Life: 10,000 hours (liner replacement interval)
- Safety Factor: 3.0 (on yield strength)

### 4.6. Power and Control
- Primary Power: Gas turbine 2 MW electric (e.g., Capstone C2000)
- Buffer: Supercapacitors 50 MJ
- Reserve: Li-ion battery 200 kWh
- Motor Power: 4-5 MW peak (MHD mode)
- Stator Power: 500 kW - 2 MW (field coils)
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
- Angular Momentum: 18.1 MN m s (total)
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
  - 350 C internal temperature
- Emergency Systems:
  - Mechanical brakes (disk type, 2 independent systems)
  - Active magnetic damping
  - Emergency gas venting (filtered)
  - Automatic shutdown on:
    - Vibration > 2.0 mm/s
    - Temperature > 300 C
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
- Gas Buffer: 12% volume (Argon, accounting for -100 C to +300 C range)
- Power Source: Kilopower nuclear reactor 1-10 kWe + solar panels (reserve)
- Heat Rejection: Droplet radiators 100-200 m2
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
- Gas Buffer: 15% volume (Ar/He mix, -150 C to +400 C range)
- Power Source: Nuclear reactor 10 MWe (thermal 30-50 MW)
- Heat Rejection: Droplet radiators 500-1000 m2 (liquid metal, 800-1000 K)
- Payload: 50 kg scientific instruments
- Mission Duration: 40-50 years (to Proxima Centauri)
- Cruise Velocity: 0.1c (30,000 km/s, hypothetical)
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
Tesla valve diodicity (Di) = (DeltaP_reverse) / (DeltaP_forward)

For asymmetric curved channel:
- Forward flow: Gradual curves, minimal separation
- Reverse flow: Sharp turns, flow separation, recirculation zones

Pressure drop (Darcy-Weisbach):
DeltaP = f * (L/D) * (rho * v^2 / 2)

Where:
- f = friction factor (function of Re and geometry)
- L = channel length
- D = hydraulic diameter
- rho = fluid density
- v = flow velocity

### 7.2. Tier 0 Valve Parameters
- Channel OD: 20 mm, ID: 16 mm
- Restriction: Taper from 16mm to 12mm over 100mm
- Number of restrictions: 4 (90 degree spacing)
- Expected Di: 2.0-3.0 (simplified design)

### 7.3. Tier 1 Valve Parameters
- Channel: 150mm x 150mm square
- Unit cell: 300mm length, asymmetric loop
- Number of cells: 94 (full 28.3m loop)
- Target Di: 3.0-5.0 (optimized geometry)
- CFD Validation: Required (ANSYS Fluent / OpenFOAM)

## 8. Manufacturing Processes

### 8.1. Tier 0 (Garage)
- Rotor disks: CNC cutting or waterjet from 20mm Al plate
- Channel: Off-the-shelf PVC/HDPE pipe, glued/welded
- Shaft: Turned steel rod, 20mm diameter
- Bearings: Press-fit 608ZZ
- Assembly: Hand tools, torque wrench
- Balancing: Static balancing on knife-edges, dynamic with vibration sensor

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
1. Static Balance Test: < 1 gram imbalance at 1m radius
2. Run-in Test: 1 hour at 100 RPM, monitor temperature/vibration
3. Balance Test: 300 RPM, vibration < 0.5 mm/s
4. Precession Test: Apply known torque, measure orthogonal response
5. MHD Test: Energize stator coils, measure current change, thrust (if any)
6. Endurance Test: 10 cycles of 10 minutes at 300 RPM

### 9.2. Tier 1 Tests
1. Hydrostatic Test: Liner at 2x operating pressure (30 MPa)
2. Spin Test: Rotor in vacuum chamber, ramp to 600 RPM
3. Thermal Test: Operating temperature cycle (-20 C to +200 C)
4. MHD Thrust Test: Measure thrust in atmospheric chamber
5. Precession Control Test: Closed-loop attitude control
6. Endurance Test: 100 hours continuous operation
7. Failure Mode Test: Controlled bearing failure, verify containment

## 10. Assumptions and Limitations

10.1. Classical Physics: MHD and gyroscopic effects are based on established electrodynamics (Faraday, Ampere, Lorentz) and rigid body mechanics (Euler).

10.2. Hypothetical Effects: Vacuum thrust via gravitomagnetic coupling (analogous to Lense-Thirring effect) is unproven and requires experimental validation (Tier 0-1 primary objective).

10.3. Material Properties: All material data from manufacturer specifications at room temperature. High-temperature derating factors applied (0.7 at 200 C for Haynes 230).

10.4. Scalability: Performance estimates for Tier 1+ are theoretical and subject to CFD/FEA validation. Reynolds number scaling may affect MHD efficiency.

10.5. Safety Margins: Factor of 3.0 on yield strength for rotating components (aerospace standard). Factor of 2.0 for pressure containment.

## 11. Revision History
v1.0 (2026-03-01): Initial specification release.
v1.1 (2026-03-02): Added Tesla valve details, Tier 0-3 nomenclature, manufacturing processes, testing protocols.
