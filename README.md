# ORVACT — Gyroscopic Propulsion Research (Archived)

> **Status: Archived — Hypothesis Not Confirmed**

This repository documents an open-source research project exploring counter-rotating gyroscopic propulsion with MHD interaction.

## 🔍 Original Hypothesis

Could a closed system of counter-rotating fluid rotors, driven by MHD forces and controlled via gyroscopic precession, generate net linear thrust without mass ejection?

## 📚 What We Learned

After theoretical analysis and peer review, we concluded:

1. **Gyroscopes provide torque, not linear thrust.** Precession changes orientation, but cannot produce net force in a closed system without external momentum exchange.

2. **MHD forces in a sealed channel** create internal pressure on walls, but the vector sum over a full rotation cancels out (∮ F·dθ = 0).

3. **Conservation of momentum holds.** Without ejecting mass or interacting with an external medium (air, plasma, field), no net thrust is possible.

4. **Safety constraints are significant.** Rotating ~40 kg at 300-600 RPM with contained liquid metal poses real risks if containment fails.

## 🛑 Why Development Stopped

- No physically viable mechanism for net thrust in a closed system was identified.
- Building a prototype would not test new physics, only confirm classical mechanics.
- Risk/benefit ratio did not justify construction.

## 📦 What's in This Repository

- `SPECIFICATION.md` — Full technical specification with calculations, tolerances, and control protocols (v1.8)
- `BUILD_GUIDE_TIER0.md` — Step-by-step assembly guide (not recommended for construction)
- `/cad` — Design files for components (Tesla valve inserts, disk layouts)
- `/docs` — Research notes, peer feedback, and derivation attempts

## 💡 What Could Work Instead

| Concept | Status | Notes |
|---------|--------|-------|
| CMG-based stabilization | ✅ Proven | Gyroscopes for attitude control (used on ISS) |
| Atmospheric MHD propulsion | ⚠️ Partial | Requires ionized air / mass flow; not a closed system |
| Ion/electric propulsion | ✅ Proven | Vacuum thrust via mass ejection; low thrust, high Isp |
| Conventional rotors/propellers | ✅ Proven | Efficient, well-understood, scalable |

## 🤝 Acknowledgements

Thanks to the engineering community for rigorous peer review, especially the critics who helped clarify the physics. Null results are still results.

## 📜 License

- Documentation: CC-BY-SA 4.0
- Hardware designs: CERN-OHL-S-2.0
- Software: GPL-3.0

> *"From Garage to Stars — One Validated Step at a Time"*

---

**If you have questions or ideas for related research, feel free to open an Issue.**
**If you build something based on this work — please share your results, positive or negative.**
