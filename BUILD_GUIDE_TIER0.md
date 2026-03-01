# ORVACT Tier 0 Build Guide v1.2

## Overview
This guide details the construction of the ORVACT Tier 0 prototype (2m diameter, garage-scale).

## Critical Safety Notes (Read First)

### Mercury Warning
- Mercury amalgamates with aluminum. If using mercury:
  - DO NOT allow contact with aluminum rotor
  - Coat aluminum surfaces with epoxy barrier, OR use stainless steel rotor
  - Use metal end caps with O-ring seals (epoxy is permeable to mercury vapor)
  - Work in ventilated area with spill kit (sulfur powder, mercury absorber)
  - Wear nitrile gloves, safety glasses, lab coat

### High-Speed Rotation Warning
- 40 kg rotor at 600 RPM stores ~2 kJ kinetic energy
- 5 mm polycarbonate may not contain fragment impact
- Use 10-12 mm polycarbonate OR double-layer enclosure with 50 mm air gap
- Add rubber damping between enclosure and frame
- Safety cable rated for 200 kg minimum

### Magnet Warning
- N52 50x20mm magnets weigh ~300g each, 32 pcs = 9.6 kg per rotor
- Magnets attract with extreme force (pinching hazard)
- Keep metal tools away during installation
- Consider smaller magnets (25x10mm) for easier handling

## Bill of Materials (BOM)

### Mechanical Components
| Item | Specification | Qty | Est. Cost | Notes |
|------|---------------|-----|-----------|-------|
| Aluminum plate | 6061-T6, 2000x2000x20mm | 2 | $200 | OR stainless 304 if using mercury |
| PVC pipe | Schedule 40, 20mm OD, 16mm ID, 6m | 1 | $30 | For channel |
| Steel shaft | 1045, 25mm dia, 500mm | 1 | $40 | Upgraded for magnet load |
| Bearings | 6202-2RS deep groove | 4 | $40 | Upgraded from 608ZZ |
| Belt | GT2, 20mm width, 3m | 2 | $80 | One per motor |
| Pulleys | GT2, 20T + 60T | 4 | $80 | Two sets |
| Fasteners | M6-M12 SS304 kit | 1 | $50 | |
| Magnet inserts | 3D printed PETG, Tesla valve | 4 | $20 | For flow control |
| End caps | Metal with O-ring groove | 2 | $30 | NOT epoxy for mercury |
| **Subtotal** | | | **$570** | |

### Electrical Components
| Item | Specification | Qty | Est. Cost | Notes |
|------|---------------|-----|-----------|-------|
| BLDC motor | MXUS 3K, 1.5kW, 48V | 2 | $300 | One per rotor |
| Motor controller | VESC 6 Plus, 120A | 2 | $200 | One per motor |
| Power supply | 48V 4000W | 1 | $250 | Or dual 2000W |
| Stator wire | Enamel copper, 1.5mm, 500m | 2kg | $50 | |
| Neodymium magnets | N52, 50x20mm disc | 32 | $100 | Handle with care |
| Controller board | ESP32 DevKit v4 | 1 | $10 | |
| IMU sensor | MPU6050 | 2 | $5 | |
| Current sensor | ACS712 30A | 2 | $10 | |
| Tachometer | Hall effect + magnet | 2 | $10 | One per rotor |
| OLED display | 0.96" I2C | 1 | $5 | |
| Wiring/connectors | Assorted | 1 | $50 | |
| **Subtotal** | | | **$990** | |

### Safety Equipment
| Item | Specification | Qty | Est. Cost |
|------|---------------|-----|-----------|
| Safety enclosure | Polycarbonate 10-12mm, 2x2m | 1 sheet | $200 |
| Safety glasses | Closed type, ANSI Z87.1 | 2 | $20 |
| Gloves | Nitrile (mercury) + mechanical | 4 pairs | $40 |
| Fire extinguisher | ABC, 2-5kg | 1 | $50 |
| Steel cable | 6mm dia, 5m, 200kg rating | 1 | $40 |
| Mercury spill kit | Sulfur powder, absorber pads | 1 | $50 |
| **Subtotal** | | | **$400** | |

### Working Fluid (Choose ONE)
| Option | Material | Volume | Cost | Notes |
|--------|----------|--------|------|-------|
| Budget | Salt solution (NaCl + CuSO4) | 1.5L | $20 | Safe, low conductivity |
| Mid-range | Galinstan | 1.0L | $1000+ | Good conductivity, expensive |
| Advanced | Mercury | 1.0L | $200-500 | Best conductivity, HAZARDOUS |

**Total Estimated Cost: $1,960 - $2,960** (depending on fluid choice)

## Tools Required
- Angle grinder or CNC/waterjet service
- Drill press
- Welding (PVC cement or epoxy for channels)
- Soldering iron
- Multimeter
- Torque wrench
- Balancing equipment (knife-edges or dynamic balancer)
- Safety equipment (glasses, gloves, ventilation)

## Step-by-Step Assembly

### Step 1: Rotor Fabrication
1. Cut two 2000mm diameter disks from 20mm Al plate (or stainless 304 for mercury)
2. Drill center hole: 25mm (shaft clearance)
3. Drill mounting holes: 8-12 holes, M6, evenly spaced
4. Machine groove for channel (optional): 20mm wide, 10mm deep, at R=900mm
5. Deburr all edges
6. If using mercury: Coat entire rotor surface with epoxy barrier (2 coats, 24h cure)

### Step 2: Channel Installation
**Option A: Surface-mounted pipe with 3D printed Tesla inserts**
1. Print 4 Tesla valve inserts (PETG, 100mm length each)
2. Insert into PVC pipe before bending (slide in, orient flow direction)
3. Cut PVC pipe to 5.65m length
4. Form into circle (900mm radius)
5. Attach to rotor disk with stainless clamps (not epoxy for mercury)
6. Install metal end caps with O-ring seals (NOT epoxy for mercury)
7. Leave 12% volume (0.14L) for gas buffer

**Gas Buffer Management**
- After filling, seal channel with cap that includes Schrader valve or septum
- This allows gas volume adjustment and pressure monitoring
- Fill horizontally, rotating disk slowly to distribute gas evenly

### Step 3: Shaft and Bearings
1. Press 2 bearings (6202-2RS) into center hole of each rotor
2. Insert 500mm steel shaft (25mm) through:
   - Lower rotor
   - Spacer (stator mounting point)
   - Upper rotor
3. Secure with set screws or retaining rings
4. Check runout: < 0.1mm TIR

### Step 4: Magnet Installation
1. Mark magnet positions on rotor inner surface (32 positions, alternating polarity)
2. WARNING: Magnets attract with extreme force. Use non-magnetic tools.
3. Apply high-strength epoxy (Loctite) to magnet back
4. Place magnet, hold 30 seconds for initial set
5. Verify polarity with compass before epoxy cures
6. Allow 24h full cure time
7. Note: 32 magnets x 300g = 9.6kg added mass. Re-balance after installation.

### Step 5: Drive System (Counter-Rotation)
**Option A: Two independent motors (recommended)**
1. Mount 60T pulley on each rotor shaft
2. Mount 20T pulley on each motor shaft
3. Install both motors on adjustable bases
4. Install GT2 belts (one per motor)
5. Adjust tension: Belt should deflect 10mm with moderate finger pressure
6. Wire each motor to its own VESC controller

**Option B: Single motor + planetary gearbox**
1. Use planetary gearbox with reverse output
2. Connect motor to gearbox input
3. Connect gearbox outputs to each rotor shaft
4. Verify rotation directions are opposite
5. Note: More complex, less flexible than dual-motor

### Step 6: Electronics
1. Mount TWO VESC controllers (one per motor)
2. Wire each motor to its VESC (3 phases)
3. Wire power supply to both VESCs (48V parallel)
4. Mount ESP32 controller
5. Connect sensors:
   - MPU6050 to ESP32 (I2C)
   - ACS712 to ESP32 (ADC, one per motor)
   - Hall tachometer to ESP32 (Digital, one per rotor)
6. Connect VESCs to ESP32 (UART or CAN)
7. Test all connections with multimeter

### Step 7: Safety Enclosure
1. Build frame from aluminum profile or wood
2. Mount 10-12mm polycarbonate panels on all sides
   - Alternative: Double-layer 5mm with 50mm air gap
3. Add rubber damping between enclosure and frame
4. Install interlock switch (cuts power when opened)
5. Mount safety cable (6mm, 200kg rating) from ceiling to rotor assembly
6. Place fire extinguisher and mercury spill kit within reach

### Step 8: Fluid Filling (Critical)
**For Mercury (HAZARDOUS)**
1. Position rotor HORIZONTALLY (not vertically)
2. Use funnel with long tube, metal end cap with Schrader valve
3. Fill 1.0 liter (88% of 1.136L capacity)
4. Leave 0.14L for gas buffer (12%)
5. Rotate disk slowly during filling to distribute gas evenly
6. Seal with metal end cap + O-ring (NOT epoxy)
7. Check for leaks with soapy water
8. Place drip tray under entire channel assembly

**For Galinstan**
1. Same procedure as mercury
2. Use PTFE or PP channel (Galinstan wets glass/Al)
3. Fill under inert atmosphere if possible (prevents oxidation)

**For Salt Solution**
1. Mix: Distilled water + 360g/L NaCl + 50-100g/L CuSO4
2. Fill 1.0 liter
3. Add corrosion inhibitor (optional)
4. Seal with end caps + O-ring
5. Check for leaks

### Step 9: Balancing (Critical)
**Static Balance (empty rotor)**
1. Place rotor on knife-edges or low-friction bearings
2. Mark heavy side (rotates to bottom)
3. Add weight to light side or remove from heavy side
4. Repeat until rotor stays in any position

**Dynamic Balance (WITH FLUID AND MAGNETS)**
1. Mount vibration sensor on bearing housing
2. Fill channel with fluid (88-92%)
3. Install magnets (if not already done)
4. Run at 50 RPM
5. Measure vibration amplitude and phase
6. Add correction weights at calculated positions
7. Increase to 100 RPM, repeat balancing
8. Target: < 0.5 mm/s vibration at 300 RPM
9. Note: Fluid redistributes during rotation. Balance at operational speed.


### Step 10: Emergency Stop Circuit

#### 10.1. Principle
Fail-safe design: Power is ON only when the contactor coil is energized.  
Pressing E‑Stop de‑energizes the coil, opening all power paths simultaneously.

#### 10.2. Control Circuit (24V DC)
- 24V PSU (+) → E‑Stop button (NC) → Contactor coil K1 (A1)  
- Contactor coil K1 (A2) → 24V PSU (−)
- Coil is energized only when E‑Stop is NOT pressed.

#### 10.3. Power Circuits (48V DC)
- 48V PSU (+) → Contactor K1 common input (3× NO contacts)  
- Contact 1 output → VESC #1 power input  
- Contact 2 output → VESC #2 power input  
- Contact 3 output → Stator coil supply (+)  
- 48V PSU (−) → Common ground for all loads
- All contacts open simultaneously when coil is de‑energized.

#### 10.4. Components
| Item | Specification |
|------|---------------|
| E‑Stop button | NC contact, mushroom head, min 24V/10A rating |
| Contactor | Coil 24V DC, 3× NO contacts, min 48V/100A per contact |
| 24V PSU | Separate low‑power supply for control circuit |
| Control wiring | 0.5–0.75 mm² stranded copper |
| Power wiring | 16–25 mm² stranded copper |

#### 10.5. Testing Procedure
1. Verify E‑Stop button:  
   - Not pressed: resistance ≈ 0 Ω  
   - Pressed: resistance ∞ Ω
2. Measure contactor coil resistance: 200–500 Ω (typical for 24V coil)
3. Verify NO contacts:  
   - Coil de‑energized: ∞ Ω  
   - Coil energized: ≈ 0 Ω
4. Assemble control circuit:  
   - Press E‑Stop → coil de‑energizes  
   - Release E‑Stop → coil energizes
5. Connect power circuit:  
   - With E‑Stop pressed, measure voltage at all loads → 0V  
   - With E‑Stop released, measure voltage → 48V
6. Confirm no auto‑restart:  
   - After power loss, system does NOT restart automatically when power returns

#### 10.6. Notes
- This circuit cuts main power only.  
- VESC firmware must implement secondary protection (overcurrent, overtemp).  
- Optional: Use a 4th NO contact to break VESC ENABLE logic signals, but this does NOT replace main power disconnection.

### Step 11: Testing Protocol
**Test 1: Low-speed run (empty)**
- Speed: 100 RPM
- Duration: 10 minutes
- Monitor: Temperature, vibration, noise
- Accept: Bearing temp < 60C, vibration < 0.5 mm/s

**Test 2: Fluid fill verification**
- Position: Horizontal
- Action: Fill to 88-92%, seal, check for leaks
- Accept: No leaks at 1.5 atm test pressure

**Test 3: Dynamic balance (with fluid)**
- Speed: 50-100 RPM
- Action: Measure vibration, add correction weights
- Accept: < 0.5 mm/s at 300 RPM

**Test 4: Nominal speed (with fluid)**
- Speed: 300 RPM
- Duration: 5 minutes
- Monitor: All parameters
- Accept: Stable operation, no unusual noise

**Test 5: Precession demonstration**
- Speed: 300 RPM
- Action: Apply gentle torque to shaft (tilt)
- Observe: Orthogonal motion (90 degree phase shift)
- Document: Video, IMU data

**Test 6: MHD interaction**
- Speed: 300 RPM
- Action: Energize stator coils (start with 1A, increase to 5A)
- Measure: Motor current change, vibration change
- Document: Data logs, observations

**Test 7: Endurance**
- Speed: 300 RPM
- Duration: 10 cycles x 10 minutes
- Monitor: Temperature trend, vibration trend
- Accept: No degradation, stable parameters

## Firmware Setup
1. Install Arduino IDE or PlatformIO
2. Install libraries:
   - MPU6050 library
   - VESC library (if using UART/CAN)
3. Download ORVACT firmware from /firmware directory
4. Configure parameters:
   - MAX_RPM: 600
   - BALANCE_THRESHOLD: 0.5 (mm/s)
   - CURRENT_LIMIT: 50 (A)
5. Upload to ESP32
6. Test Bluetooth/WiFi connection
7. Verify sensor readings

## Troubleshooting
| Problem | Possible Cause | Solution |
|---------|----------------|----------|
| Excessive vibration | Unbalanced rotor (with fluid) | Dynamic balance at operational speed |
| Mercury leak | Epoxy seal (permeable) | Use metal end caps with O-ring |
| Aluminum corrosion | Mercury contact | Coat with epoxy or use stainless rotor |
| Motor won't sync | Two VESCs not synchronized | Use master-slave CAN configuration |
| No MHD effect | Coils not energized, wrong polarity | Check current, verify magnet polarity |
| Overheating bearings | Misalignment, overtightened belt | Check alignment, reduce tension |

## Data Logging
Record for each test:
- Date, time, operator
- RPM target and actual (both rotors)
- Vibration (mm/s RMS, per bearing)
- Bearing temperature (C, per bearing)
- Motor current (A, per motor)
- Stator current (A)
- Gas buffer pressure (if monitored)
- Observations (noise, behavior, fluid movement)
- Issues encountered

## Tier Nomenclature
- Tier 0: Garage prototype (2m diameter, proof of physics)
- Tier 1: Atmospheric vehicle (10m diameter, commercial)
- Tier 2: Lunar missions (8-10m diameter, piloted)
- Tier 3: Interstellar probe (4-6m diameter, autonomous)

Note: Previous "Level 1/2/3" terminology is deprecated. Use Tier 0-3.

## Disclaimer
Building and operating high-speed rotating machinery with hazardous materials involves significant risks:
- Mechanical failure: 40 kg rotor at 600 RPM stores ~2 kJ. Fragment impact can penetrate 5mm polycarbonate.
- Electrical hazard: 48V DC at 100A can cause arc flash, burns, electrocution.
- Chemical hazard: Mercury vapor is toxic. Amalgamation with aluminum causes structural failure.
- Fire hazard: Motor windings, electronics, epoxy can ignite.

You assume all risks. The authors provide no warranty. Follow all local safety regulations. Consult professionals for hazardous material handling.

---

Good luck with your build. Document everything. Share your results.
