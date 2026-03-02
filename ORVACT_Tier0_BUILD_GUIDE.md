# ORVACT Tier 0 Build Guide v1.4
## Garage-Scale Gyroscopic Propulsion Prototype

**Repository:** https://github.com/dz9ikx/ORVACT  
**License:** CERN-OHL-S-2.0 (hardware), CC-BY-SA 4.0 (docs)  
**Target Build Cost:** ~$600-900 (excluding motors/electronics)  
**Estimated Build Time:** 40-60 hours

---
## ⚠️ CRITICAL OPERATIONAL SAFETY NOTICE

**READ BEFORE PROCEEDING:**

This prototype contains rotating masses (~40 kg per rotor) at speeds up to 600 RPM. 

**FAILURE MODE:** If a rotor disk cracks, a bearing fails, or fluid containment is breached, high-energy fragments can be ejected at lethal velocities.

**MANDATORY SAFETY PROTOCOLS:**

1. **NO PERSONNEL NEARBY DURING OPERATION**
   - Minimum safe distance: 10 meters (33 feet) from rotating assembly
   - Operator must be behind protective barrier during any spin-up >50 RPM

2. **PROTECTIVE ENCLOSURE REQUIRED**
   - Material: Polycarbonate (Lexan) 10-12 mm minimum thickness
   - Coverage: Full 360° around rotor perimeter + top/bottom shields
   - Mounting: Independently anchored (not attached to test stand)

3. **REMOTE OPERATION ONLY**
   - All controls (power, throttle, E-Stop) must be accessible from behind barrier
   - Use camera + telemetry for monitoring; do not visually inspect while spinning

4. **PRE-TEST CHECKLIST (Every Run)**
   - [ ] Enclosure secured, no gaps >5mm
   - [ ] All fasteners torqued and verified
   - [ ] No tools/debris inside enclosure
   - [ ] E-Stop tested and functional
   - [ ] All personnel clear and accounted for

**Rule:** If you wouldn't stand behind it with a loaded gun pointed at you, don't stand near it while it's spinning.

*Proceed only when all safety measures are verified.*

##  SAFETY FIRST

| Hazard | Mitigation |
|--------|-----------|
| **Rotating mass at speed** | Polycarbonate enclosure (10-12mm), never operate without guard |
| **Galinstan contact** | Nitrile gloves, eye protection; non-toxic but leaves metallic residue |
| **Electrical (48V, 120A)** | Insulated tools, E-Stop button within reach, fuse protection |
| **Epoxy/chemicals** | Ventilation, respirator when sanding/cutting composites |
| **Pinch points** | Keep hands clear of rotating assembly; use tools for adjustments |
| **Catastrophic rotor failure** | Polycarbonate enclosure (10-12mm), remote operation, 10m exclusion zone |
| **High-velocity fragment ejection** | Full 360° shielding; never operate with exposed rotating assembly |
| **Bearing seizure → sudden stop** | Vibration monitoring; auto-shutdown if >2.5 mm/s RMS |
| **Fluid leak during operation** | Secondary containment tray; absorbent material ready; remote shutdown |

**Rule #1:** If something feels unsafe, stop. Reassess. Proceed only when confident.

---

##  BILL OF MATERIALS (BOM)

### Structural Components

| Item | Specification | Qty | Source/Notes |
|------|--------------|-----|-------------|
| Rotor disks | Aluminum 6061-T6, Ø2000mm, 20mm thick | 2 | Metal supplier + laser/waterjet cut |
| Shaft | Steel 1045, Ø25mm, length ~500mm | 1 | Machine shop or precision ground rod |
| Bearings | 7202 angular contact pair (or 6202-2RS minimum) | 4 | Bearing supplier; preload capable preferred |
| Bearing housings | Custom machined or pillow block (Ø25mm bore) | 2 | Machine shop or adapt off-the-shelf |
| Fasteners | M8 stainless steel bolts, nuts, washers | ~40 sets | Hardware store; grade A2/A4 |
| Spacers | Aluminum or nylon, 20mm length, Ø8mm ID | ~40 | Machine shop or 3D print (nylon) |

### Fluid Channel

| Item | Specification | Qty | Source/Notes |
|------|--------------|-----|-------------|
| PVC pipe | Schedule 40, 20mm OD / 16mm ID, length 6m | 1 | Hardware store |
| End caps | PVC, 20mm OD, with Schrader valve fitting | 2 | Plumbing supplier + modify one |
| O-rings | Nitrile, Ø16mm ID × 2mm cross-section | 2 | Sealing supplier |
| Epoxy adhesive | Structural grade (Loctite EA 9460 or equivalent) | 1 tube | Automotive/industrial supplier |
| Isopropyl alcohol | 99% purity, for surface prep | 1 bottle | Pharmacy/chemical supplier |

### Working Fluid

| Item | Specification | Qty | Source/Notes |
|------|--------------|-----|-------------|
| Galinstan | Ga-In-Sn eutectic, 99.99% purity | 1.0 L | Specialty metals supplier (~$200-300) |
| Syringe | 50-100 mL, Luer-lock compatible | 2 | Medical supply or lab equipment |
| Transfer tubing | Silicone, Ø4mm ID, Ø6mm OD, ~30cm | 1 | Lab supply |

### Tesla Valve Inserts (Optional but Recommended)

| Item | Specification | Qty | Source/Notes |
|------|--------------|-----|-------------|
| Filament | Polyurethane (Shore 70D) or POM-compatible | ~200g | 3D printing supplier |
| 3D printer | FDM, 0.4mm nozzle, heated bed recommended | 1 | Own or use printing service |
| Valve design | STL files in `/cad/tesla_valve_tier0/` | 4 | Print with 100% infill, orient for strength |

### Sensors & Instrumentation (Minimum Viable)

| Item | Specification | Qty | Source/Notes |
|------|--------------|-----|-------------|
| MPU6050 | IMU (gyro + accel), I2C interface | 1 | Electronics supplier |
| Optical tachometer | Reflective, 1-10000 RPM range | 2 | Electronics supplier |
| Load cell | 0-10 N range, 0.01 N resolution (for thrust) | 1 | Scale components supplier |
| HX711 amplifier | 24-bit ADC for load cell | 1 | Electronics supplier |
| ESP32 dev board | WiFi/BT, for data logging | 1 | Electronics supplier |
| Wiring, connectors | 22-24 AWG silicone wire, JST connectors | assorted | Electronics supplier |

### Power & Control (Not Included in Base BOM)

| Item | Specification | Qty | Notes |
|------|--------------|-----|-------|
| BLDC motors | MXUS 3K or equivalent, 48V, 1.5 kW peak | 2 | One per rotor |
| VESC controllers | VESC 6 Plus, 120A, CAN-capable | 2 | One per motor |
| Power supply | 48V, 50A minimum (2.4 kW) | 1 | Server PSU or Li-ion pack |
| Stator coils | Custom wound, 50-100 turns, 1.5mm Cu wire | 4-8 | See SPECIFICATION.md §3.6.2 |

---

##  TOOLS REQUIRED

### Essential
- Drill + drill bits (3-10mm, HSS)
- Angle grinder or Dremel (for slot cutting)
- Torque wrench (5-50 N·m range)
- Digital scale (0.1g resolution, 10 kg capacity)
- Calipers or micrometer
- Hex key set (metric)
- Safety glasses, gloves, hearing protection

### Recommended
- Vacuum pump (for channel evacuation before fill)
- Vibration meter or smartphone app with FFT
- Infrared thermometer (for thermal monitoring)
- Multimeter (for electrical checks)

### Nice-to-Have
- CNC or milling machine (for precision pazing)
- Dynamic balancing rig (can improvise)
- Oscilloscope (for signal analysis)

---

##  PREPARATION: DISK FABRICATION

### Step 1: Cut Disks to Spec
- Material: Aluminum 6061-T6, 20mm thick
- Outer diameter: 2000 mm
- Center hole: Ø25 mm (for shaft), tolerance H7
- Bolt circle: Ø1800 mm, 36 holes @ 10° spacing, Ø8.5 mm clearance

### Step 2: Cut Eddy Current Slots (Critical)
**Purpose:** Break circular current paths to prevent braking/heating.

```
Slot specifications:
- Count: 12 slots per disk
- Spacing: 30° apart (360° / 12)
- Width: 3 mm minimum
- Depth: 850 mm radial cut (from R=900mm to R=50mm)
- Tool: Angle grinder with 3mm cutoff wheel or CNC mill
```

**Procedure:**
1. Mark slot positions on disk edge (12 marks @ 30° intervals).
2. Cut from outer edge toward center, stopping 50 mm from shaft hole.
3. Deburr all cut edges (file or sandpaper).
4. Verify: No continuous conductive path around disk circumference.

### Step 3: Machine Pazing (Optional but Recommended)
**Purpose:** Seat the PVC channel securely between disks.

```
Paze specifications (per disk):
- Radius: 900 mm from center
- Width: 21 mm (to fit 20mm OD pipe with slight clearance)
- Depth: 10 mm (so two disks create 20mm total channel height)
```

**Alternative (simpler):** Use 20mm thick spacers between disks instead of machining pazes. Less precise but functional.

### Step 4: Drill Mounting Holes
- Bolt holes: Ø8.5 mm clearance for M8 bolts
- Center bore: Ø25 mm H7 tolerance for shaft fit
- Sensor mounts: Add 2-4 M3 tapped holes for IMU/bracket

### Step 5: Surface Preparation
1. Degrease both disk faces with isopropyl alcohol.
2. Lightly sand bonding surfaces (120-220 grit) for epoxy adhesion.
3. Clean again with alcohol; let dry completely.

---

##  CHANNEL ASSEMBLY

### Step 1: Cut and Form PVC Pipe
- Length: 5.65 m (circumference at R=900mm) + 10 cm extra for connections
- Form into circle: Heat gently with heat gun (~150°C) while bending around a 1800mm diameter form.
- Let cool in shape before proceeding.

### Step 2: Install Tesla Valve Inserts (If Using)
1. Divide channel into 4 equal segments (~1.4m each).
2. Insert one 3D-printed Tesla valve into each segment:
   - Orientation: Forward direction = direction of intended fluid flow during operation
   - Secure with small amount of epoxy or heat-shrink sleeve
3. Verify: No obstruction to flow; inserts centered in pipe.

### Step 3: Prepare End Caps
**Fill port end:**
- Install Schrader valve (like bicycle tire) into one end cap.
- Seal with epoxy; pressure test before final assembly.

**Closed end:**
- Cap permanently with epoxy + mechanical fastener (small screw through cap into pipe).

### Step 4: Dry Fit Assembly
1. Place lower disk on flat surface.
2. Position PVC ring in paze (or on spacers).
3. Place upper disk on top.
4. Insert all M8 bolts loosely; verify alignment.
5. Disassemble; clean all surfaces again with alcohol.

---

##  FINAL ASSEMBLY & SEALING

### Step 1: Apply Epoxy
1. Mix structural epoxy per manufacturer instructions.
2. Apply thin, continuous bead to:
   - Paze or spacer contact area on lower disk
   - Outer edge of PVC pipe where it contacts disks
3. Avoid excess: Epoxy inside channel = flow obstruction.

### Step 2: Assemble Sandwich
1. Place PVC ring onto lower disk.
2. Align upper disk; insert all bolts.
3. Tighten bolts in star pattern to specified torque:
   - M8 stainless: 20-25 N·m (check fastener specs)
4. Wipe away any epoxy squeeze-out immediately.

### Step 3: Cure
- Let epoxy cure fully (24-72 hours per product specs).
- Keep assembly flat and undisturbed during cure.

### Step 4: Leak Test (Before Filling)
1. Connect air source to Schrader valve.
2. Pressurize channel to 0.3 atm (~4.5 PSI) — **do not exceed**.
3. Spray all joints with soapy water.
4. Watch for bubbles for 10 minutes.
5. If leaks found: Depressurize, repair, retest.
6. If sealed: Depressurize; proceed to fill.

---

##  FLUID FILL PROCEDURE

### Preparation
- Work in well-ventilated area, wear nitrile gloves.
- Have spill kit ready: paper towels, plastic bags, container for waste.
- Weigh empty rotor assembly; record mass (M_empty).

### Step 1: Position for Fill
- Mount rotor vertically on test stand (shaft horizontal).
- Position fill port (Schrader valve) at highest point.

### Step 2: Evacuate Air (Optional but Recommended)
1. Connect vacuum pump to Schrader valve via tubing.
2. Pull vacuum to ~0.8 atm below ambient (do not exceed PVC rating).
3. Hold 2-3 minutes to remove moisture/air from channel.
4. Close valve; disconnect pump.

### Step 3: Inject Galinstan
1. Fill syringe with Galinstan (pre-warm to ~25°C for lower viscosity).
2. Connect syringe to Schrader valve via transfer tubing.
3. Inject slowly:
   - Target volume: 1.00 L (±10 mL)
   - Target mass gain: +6.44 kg (Galinstan density: 6.44 g/mL)
4. Pause periodically to let fluid settle and air escape.

### Step 4: Seal Fill Port
1. When target mass reached, disconnect syringe.
2. Immediately close Schrader valve core.
3. Apply small amount of epoxy over valve for secondary seal.
4. Let cure 24 hours before handling.

### Step 5: Verify Fill
- Weigh assembled rotor; confirm mass = M_empty + 6.44 kg (±50g).
- Gently rotate rotor by hand; listen for fluid movement (should be smooth, no sloshing).

---

##  BALANCING PROCEDURE

**Critical:** An unbalanced rotor at 300+ RPM will destroy bearings and create dangerous vibration.

### Static Balance (Initial)
1. Mount rotor on low-friction horizontal shaft (knife-edges or air bearings ideal).
2. Let rotor settle; mark lowest point (heavy spot).
3. Add temporary weight (clay, tape) to opposite side (light spot).
4. Iterate until rotor stays in any position (neutral balance).
5. Replace temporary weights with permanent correction (drill + epoxy weights or remove material).

### Dynamic Balance (Operational)
1. Mount rotor on test stand with vibration sensor (MPU6050 or dedicated meter).
2. Spin up to 50 RPM; measure vibration amplitude.
3. If vibration > 1.0 mm/s RMS:
   - Add correction weight to rim at angle opposite vibration peak
   - Re-test; iterate until < 0.5 mm/s RMS at 50 RPM
4. Gradually increase speed: 100 → 150 → 200 → 300 RPM
5. At each step, verify vibration remains < 0.5 mm/s RMS
6. If vibration spikes at any speed: STOP. Re-balance at lower speed before proceeding.

**Note:** Fluid movement changes balance dynamically. Final balance should be done with fluid filled and at target operating temperature.

---

## 🔌 ELECTRICAL & CONTROL SETUP

### Motor Mounting
- Mount one BLDC motor per rotor, aligned with shaft.
- Use 3:1 belt reduction (20T motor pulley → 60T rotor pulley).
- Verify belt tension: ~10-15 mm deflection at midpoint.

### VESC Configuration (Per Motor)
```
Basic settings:
- Motor type: BLDC
- Battery voltage: 48V nominal
- Current limit: 80A continuous, 120A peak
- RPM limit: 1800 RPM motor side (= 600 RPM rotor side)
- Sensor mode: Sensorless (or hall if equipped)
- CAN interface: Enabled, unique ID per controller

Advanced:
- PID tuning: Start conservative; increase responsiveness after testing
- Brake mode: Current braking (not short circuit)
- Safety: Enable over-temp, over-current, RPM limits
```

### Stator Coil Wiring
- Wire coils in series or parallel per SPECIFICATION.md §3.6.2.
- Include flyback diodes or snubber circuit for inductive kickback.
- Test coil resistance before connecting to power: Expect 0.5-2 Ω total.

### Sensor Integration
- MPU6050: Mount on stationary chassis, aligned with rotor axis.
- Tachometers: Aim at reflective tape on rotor rim.
- Load cell: Mount between test stand and chassis mounting point.
- ESP32: Log all sensors at ≥100 Hz; stream via WiFi or log to SD.

---

## 🔒 OPERATIONAL SAFETY PROCEDURES

### Pre-Test Safety Verification
**Complete this checklist before EVERY test run:**

[ ] ENCLOSURE
Polycarbonate panels intact, no cracks or deep scratches
All fasteners secure; no gaps >5mm between panels
Viewing ports (if any) use laminated polycarbonate, not glass
[ ] ROTOR ASSEMBLY
All M8 bolts torqued to 20-25 N·m, verified with marker pen
No visible cracks, deformation, or epoxy failure
Fluid fill port sealed and epoxy-cured (24h minimum)
[ ] MECHANICAL
Bearings lubricated, no play or unusual noise when spun by hand
Belt tension correct (10-15mm deflection); guards installed
Shaft alignment verified; no binding
[ ] ELECTRICAL
All high-current connections tight, insulated, strain-relieved
E-Stop circuit tested: cuts power within <100ms
Fuses installed and rated correctly
[ ] PERSONNEL
All non-essential personnel cleared from 10m exclusion zone
Operator positioned behind protective barrier
Communication method established (radio/hand signals)
[ ] ENVIRONMENT
Floor clear of tools, debris, trip hazards
Fire extinguisher (Class C for electrical) within reach
First aid kit accessible


**If ANY item fails: DO NOT PROCEED. Fix, re-check, then continue.**

### During Test: Remote Operation Protocol
1. **Arm system** only when behind barrier.
2. **Start at lowest speed** (50 RPM); monitor telemetry for 60 seconds.
3. **Increment slowly**: Wait 2-3 minutes between speed steps.
4. **Watch for red flags**:
   - Vibration spike >0.5 mm/s RMS
   - Bearing temperature rise >10°C above ambient
   - Unusual acoustic signature (grinding, knocking, whining)
   - Thrust sensor drift unrelated to control input
5. **If any red flag appears**: Hit E-Stop immediately. Do not investigate until rotors are fully stopped.

### Post-Test: Safe Shutdown
1. Allow rotors to coast to complete stop (do not brake hard unless emergency).
2. Wait 60 seconds after stop before approaching enclosure.
3. Visually inspect through viewing port before opening.
4. Log any anomalies before next test.

### Emergency Procedures
**If enclosure is breached or catastrophic failure occurs:**
1. **DO NOT APPROACH** — secondary fragments may be ejected.
2. Cut main power at breaker (not just E-Stop).
3. Wait 5 minutes for all motion/energy to dissipate.
4. Approach only with full PPE (face shield, gloves, long sleeves).
5. Document failure mode with photos before disassembly.

**Remember:** No data point is worth a life. If in doubt, shut it down.


##  TESTING PROTOCOL (Tier 0 Validation)

### Phase 1: Cold Spin (No Field, No Fluid Motion)
- Goal: Verify mechanical integrity, balance, bearing temps.
- Procedure:
  1. Spin to 50 RPM; hold 5 minutes; monitor vibration, temp.
  2. Increment by 50 RPM steps to 300 RPM.
  3. At each step: Record vibration (RMS), bearing temp, motor current.
  4. Abort if: Vibration > 0.5 mm/s, temp rise > 10°C above ambient.

### Phase 2: Precession Test (No MHD Field)
- Goal: Validate classical gyroscopic response.
- Procedure:
  1. Spin rotors to 300 RPM (counter-rotating).
  2. Apply known torque to chassis (e.g., hang 100g weight at 0.5m lever).
  3. Measure precession rate via MPU6050.
  4. Compare to theory: Ω = τ / L (expect ~0.14 deg/s per 1 N·m).
  5. Repeat for multiple torque values; plot Ω vs. τ.

### Phase 3: MHD Activation (Atmospheric)
- Goal: Detect additional force beyond classical prediction.
- Procedure:
  1. With rotors at 300 RPM, energize stator coils at low field (0.05 T).
  2. Use discrete control quanta (per SPEC §3.6.5):
     - Ramp: 300 ms → Hold: 500 ms → Ramp down: 300 ms → Settle: 1000 ms
  3. Measure thrust via load cell; record synchronized with control signal.
  4. Sweep field strength: 0.05 → 0.1 → 0.2 → 0.3 T.
  5. Analyze: Does thrust scale with B²? Is signal above noise floor (3σ)?

### Phase 4: Null Test (Control)
- Goal: Confirm signal is MHD, not artifact.
- Procedure:
  1. Repeat Phase 3 with stator coils disconnected (same control signals).
  2. Any "thrust" measured now is artifact (vibration, EMI, thermal drift).
  3. Subtract artifact baseline from Phase 3 data.

### Success Criteria
- Precession matches Euler prediction within 10% → Control model validated.
- MHD thrust detected > 0.01 N, scaling with B² → Propulsion model validated.
- If either fails: Document null result; revise model; publish findings.

---

##  TROUBLESHOOTING

| Symptom | Likely Cause | Fix |
|---------|-------------|-----|
| High vibration at low RPM | Static imbalance | Re-balance statically; check for fluid slosh |
| Vibration spikes at specific RPM | Resonance / dynamic imbalance | Add damping; re-balance dynamically; avoid resonant speeds |
| Motor overheating | Eddy current braking (disk not slotted) | Verify slots are cut fully; reduce field strength |
| No thrust signal, high noise | Poor sensor isolation / EMI | Shield cables; separate power/signal grounds; use differential sensing |
| Fluid leak | Epoxy bond failure / cracked PVC | Depressurize; drain fluid; repair with fresh epoxy + reinforcement |
| Galinstan not flowing | Air lock / high viscosity | Warm fluid to 30°C; evacuate channel before fill; check for blockages |
| **Loud bang / vibration spike** | HIT E-STOP. Do not approach for 5 minutes. Inspect remotely first. |
| **Visible fluid leak during spin** | E-Stop. Allow full stop. Contain spill with absorbent; do not touch Galinstan with bare skin. |
| **Smoke / burning smell** | E-Stop. Cut main power. Ventilate area. Check for electrical short before re-energizing. |

---

##  DATA LOGGING TEMPLATE

Log these parameters at ≥100 Hz during tests:

```csv
timestamp,rotor_A_rpm,rotor_B_rpm,stator_field_T,thrust_N,precession_deg_s,vibration_mm_s,bearing_temp_C,motor_A_current_A,motor_B_current_A
```

**Analysis checklist:**
- [ ] Thrust signal synchronized with control quanta?
- [ ] Noise floor characterized (std dev during null test)?
- [ ] Signal-to-noise ratio > 3 for claimed detection?
- [ ] Scaling with B² and ω consistent with Lorentz model?

---

##  MAINTENANCE & STORAGE

### After Each Test
1. Power down; let rotors stop completely.
2. Inspect for leaks, unusual wear, loose fasteners.
3. Log any anomalies in build journal.

### Long-Term Storage
- Store rotor horizontally to minimize fluid pressure on seals.
- Keep in dry environment; Galinstan can oxidize slowly in humid air.
- Periodically rotate by hand to prevent bearing brinelling.

### End-of-Life / Disposal
- Galinstan: Collect in sealed container; return to supplier or dispose as electronic waste (not household trash).
- Aluminum: Recycle as scrap metal.
- PVC/Epoxy: Dispose per local regulations for mixed materials.

---


##  FREQUENTLY ASKED QUESTIONS

**Q: Do we really need to spin at 600 RPM?**  
A: No. Tier 0 validation can begin at 100-200 RPM. Lower speed = safer, easier to balance, less stress. MHD signal scales with velocity, so start low, confirm classical behavior, then scale up only if needed.

**Q: What if Galinstan is too expensive?**  
A: Salt water is 1000× less conductive → thrust signal drops 1000×. Not recommended for quantitative testing. If budget is tight, reduce scale (smaller diameter) rather than fluid quality.

**Q: Can I skip the Tesla valves?**  
A: Yes. They are for damping secondary flows, not core MHD function. Build without them first; add later if slosh interferes with measurements.

**Q: How do I know if my thrust signal is real?**  
A: Run the Null Test (Phase 4). If signal disappears when stator is off, it's likely real. If it persists, it's artifact (vibration, EMI, thermal).

**Q: Can I stand next to the rig during low-speed tests (<100 RPM)?**  
A: No. The exclusion zone and barrier requirement apply at ANY speed >50 RPM. Failure modes are unpredictable; a small crack can propagate catastrophically in milliseconds. Remote operation is non-negotiable.

**Q: What if I don't have polycarbonate?**  
A: Do not test. Alternatives like acrylic (PMMA) are brittle and can shatter into sharp fragments. Steel mesh can stop large pieces but not fine debris. Polycarbonate (Lexan/Makrolon) is the minimum acceptable material. If unavailable, reduce scale (smaller rotor) until proper shielding is sourced.

---

##  CONTRIBUTING

Found an error? Have a better assembly technique?  
→ Open an Issue or Pull Request at https://github.com/dz9ikx/ORVACT

All improvements must remain open under CERN-OHL-S-2.0.

---

*"From Garage to Stars — One Validated Step at a Time"*
