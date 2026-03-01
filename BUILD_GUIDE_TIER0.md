# ORVACT Tier 0 Build Guide

## Overview
This guide details the construction of the ORVACT Tier 0 prototype (2m diameter, garage-scale).

## Bill of Materials (BOM)

### Mechanical Components
| Item | Specification | Qty | Est. Cost | Source |
|------|---------------|-----|-----------|--------|
| Aluminum plate | 6061-T6, 2000x2000x20mm | 2 | $200 | Metal supplier |
| PVC pipe | Schedule 40, 20mm OD, 6m | 1 | $30 | Hardware store |
| Steel shaft | 1045, 20mm dia, 500mm | 1 | $30 | Metal supplier |
| Bearings | 608ZZ (skateboard) | 4 | $20 | Online |
| Belt | GT2, 20mm width, 3m | 1 | $40 | 3D printer supplier |
| Pulleys | GT2, 20T + 60T | 2 | $40 | Online |
| Fasteners | M6-M12 SS304 kit | 1 | $50 | Hardware store |
| **Subtotal** | | | **$410** | |

### Electrical Components
| Item | Specification | Qty | Est. Cost | Source |
|------|---------------|-----|-----------|--------|
| BLDC motor | MXUS 3K, 1.5kW, 48V | 1 | $150 | E-bike supplier |
| Motor controller | VESC 6 Plus, 120A | 1 | $100 | Online |
| Power supply | 48V 2000W | 1 | $150 | Online |
| Stator wire | Enamel copper, 1.5mm, 500m | 2kg | $50 | Electronics supplier |
| Neodymium magnets | N52, 50x20mm disc | 32 | $100 | Magnet supplier |
| Controller board | ESP32 DevKit v4 | 1 | $10 | Online |
| IMU sensor | MPU6050 | 2 | $5 | Online |
| Current sensor | ACS712 30A | 2 | $10 | Online |
| Tachometer | Hall effect + magnet | 1 | $5 | Online |
| OLED display | 0.96" I2C | 1 | $5 | Online |
| Wiring/connectors | Assorted | 1 | $50 | Electronics supplier |
| **Subtotal** | | | **$635** | |

### Safety Equipment
| Item | Specification | Qty | Est. Cost |
|------|---------------|-----|-----------|
| Safety enclosure | Polycarbonate 5mm, 2x2m | 1 sheet | $100 |
| Safety glasses | Closed type | 2 | $20 |
| Gloves | Mechanical | 2 pairs | $20 |
| Fire extinguisher | ABC, 2-5kg | 1 | $50 |
| Steel cable | 3mm dia, 5m | 1 | $20 |
| **Subtotal** | | | **$210** | |

### Working Fluid (Choose ONE)
| Option | Material | Volume | Cost | Notes |
|--------|----------|--------|------|-------|
| Budget | Salt solution (NaCl + CuSO4) | 1.5L | $20 | Low conductivity, safe |
| Mid-range | Galinstan | 1.0L | $1000+ | Good conductivity, expensive |
| Advanced | Mercury | 1.0L | $200-500 | Best conductivity, HAZARDOUS |

**Total Estimated Cost: $1,255 - $2,255** (depending on fluid choice)

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
1. Cut two 2000mm diameter disks from 20mm Al plate
2. Drill center hole: 20mm (shaft clearance)
3. Drill mounting holes: 8-12 holes, M6, evenly spaced
4. Machine groove for channel (optional): 20mm wide, 10mm deep, at R=900mm
5. Deburr all edges
6. Static balance: Place on knife-edges, add weight to heavy side until balanced

### Step 2: Channel Installation
**Option A: Surface-mounted pipe**
1. Cut PVC pipe to 5.65m length
2. Form into circle (900mm radius)
3. Attach to rotor disk with clamps or epoxy
4. Leave 5-10cm unfilled for gas buffer
5. Seal ends with caps or epoxy

**Option B: Embedded channel**
1. Machine groove in rotor (20mm wide, 10mm deep)
2. Place pipe in groove
3. Cover with epoxy or retaining ring

**Tesla Valve (Simplified)**
1. Install 4 flow restrictions (tapered sections)
2. Spacing: 90 degrees apart
3. Taper: 16mm ID to 12mm ID over 100mm length

### Step 3: Shaft and Bearings
1. Press 2 bearings into center hole of each rotor
2. Insert 500mm steel shaft through:
   - Lower rotor
   - Spacer (stator mounting point)
   - Upper rotor
3. Secure with set screws or retaining rings
4. Check runout: < 0.1mm TIR

### Step 4: Stator Assembly
1. Cut two 400mm diameter disks from 10mm fiberglass/phenolic
2. Wind 4-8 coils:
   - Wire: 1.5mm enamel copper
   - Turns: 50-100 per coil
   - Connection: Series or parallel (test both)
3. Mount coils on stator disk with epoxy
4. Attach stator between rotors (non-rotating)
5. Air gap: 3-5mm to rotor magnets

### Step 5: Magnet Installation
1. Mark magnet positions on rotor inner surface
2. Pattern: Alternating N-S-N-S (32 magnets total)
3. Adhesive: High-strength epoxy (Loctite)
4. Verify polarity with compass before gluing
5. Allow 24h cure time

### Step 6: Drive System
1. Mount 60T pulley on rotor shaft
2. Mount 20T pulley on motor shaft
3. Install motor on adjustable base
4. Install GT2 belt
5. Adjust tension: Belt should deflect 10mm with moderate finger pressure

### Step 7: Electronics
1. Mount VESC controller
2. Wire motor to VESC (3 phases)
3. Wire power supply to VESC (48V)
4. Mount ESP32 controller
5. Connect sensors:
   - MPU6050 to ESP32 (I2C: SDA, SCL, 3.3V, GND)
   - ACS712 to ESP32 (ADC, 5V, GND)
   - Hall tachometer to ESP32 (Digital, 5V, GND)
6. Connect VESC to ESP32 (UART or CAN)
7. Test all connections with multimeter

### Step 8: Safety Enclosure
1. Build frame from aluminum profile or wood
2. Mount 5mm polycarbonate panels on all sides
3. Install interlock switch (optional but recommended)
4. Mount safety cable from ceiling to rotor assembly
5. Place fire extinguisher within reach

### Step 9: Fluid Filling
**For Mercury (HAZARDOUS - Use ventilation, gloves, spill kit)**
1. Position rotor vertically
2. Use funnel with long tube
3. Fill 1.0 liter (92% of 1.13L capacity)
4. Leave 0.13L for gas buffer
5. Seal ends completely
6. Check for leaks with soapy water

**For Galinstan**
1. Same procedure as mercury
2. Use PTFE or PP channel (Galinstan wets glass/Al)
3. Fill under inert atmosphere if possible (prevents oxidation)

**For Salt Solution**
1. Mix: Distilled water + 360g/L NaCl + 50-100g/L CuSO4
2. Fill 1.0 liter
3. Add corrosion inhibitor (optional)
4. Seal and check for leaks

### Step 10: Balancing
**Static Balance**
1. Place rotor on knife-edges or low-friction bearings
2. Mark heavy side (rotates to bottom)
3. Add weight to light side or remove from heavy side
4. Repeat until rotor stays in any position

**Dynamic Balance (Recommended)**
1. Mount vibration sensor on bearing housing
2. Run at 100 RPM
3. Measure vibration amplitude and phase
4. Add correction weights at calculated positions
5. Repeat until vibration < 0.5 mm/s

### Step 11: Testing Protocol
**Test 1: Low-speed run**
- Speed: 100 RPM
- Duration: 10 minutes
- Monitor: Temperature, vibration, noise
- Accept: Bearing temp < 60C, vibration < 0.5 mm/s

**Test 2: Nominal speed**
- Speed: 300 RPM
- Duration: 5 minutes
- Monitor: All parameters
- Accept: Stable operation, no unusual noise

**Test 3: Precession demonstration**
- Speed: 300 RPM
- Action: Apply gentle torque to shaft (tilt)
- Observe: Orthogonal motion (90 degree phase shift)
- Document: Video, IMU data

**Test 4: MHD interaction**
- Speed: 300 RPM
- Action: Energize stator coils (start with 1A, increase to 5A)
- Measure: Motor current change, vibration change
- Document: Data logs, observations

**Test 5: Endurance**
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
| Excessive vibration | Unbalanced rotor | Re-balance, check for debris |
| Overheating bearings | Misalignment, overtightened belt | Check alignment, reduce tension |
| Motor won't spin | VESC not configured | Run VESC detection routine |
| No MHD effect | Coils not energized, wrong polarity | Check current, verify magnet polarity |
| Fluid leak | Poor seal | Re-seal with epoxy, check for cracks |

## Safety Checklist (Before Every Run)
- [ ] Enclosure closed and secured
- [ ] Safety cable attached
- [ ] Fire extinguisher accessible
- [ ] Safety glasses and gloves on
- [ ] Area clear (3m radius)
- [ ] Emergency stop tested
- [ ] Sensors reading correctly
- [ ] No fluid leaks visible
- [ ] Belt tension correct
- [ ] Bearings lubricated

## Data Logging
Record for each test:
- Date, time, operator
- RPM target and actual
- Vibration (mm/s RMS)
- Bearing temperature (C)
- Motor current (A)
- Stator current (A)
- Observations (noise, behavior)
- Issues encountered

## Next Steps After Tier 0
1. Analyze data: Does precession match theory?
2. Document results: Video, graphs, raw data
3. Publish: GitHub, forums, YouTube
4. Iterate: Improve design based on findings
5. Plan Tier 1: Scale-up requires industrial partners

## Disclaimer
Building and operating high-speed rotating machinery involves significant risks including:
- Mechanical failure and projectile hazards
- Electrical shock (48V DC, high current)
- Chemical exposure (if using mercury or Galinstan)
- Fire hazard (motor, electronics)

You assume all risks. The authors provide no warranty. Follow all local safety regulations.

---

Good luck with your build. Document everything. Share your results.
