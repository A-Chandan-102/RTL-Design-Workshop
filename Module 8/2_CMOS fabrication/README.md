# 16-Mask CMOS Process and CMOS Inverter Layout Lab

---

## The 16-Mask CMOS Process

**Selecting a Substrate**
- P-type, high resistivity (5–50 Ω·cm), doping ~10¹⁵ cm⁻³, orientation (100)
- Substrate doping must be lower than well doping

**Creating Active Regions — Mask 1**
- ~40 nm SiO₂ + ~80 nm Si₃N₄ grown/deposited, ~1 µm photoresist patterned
- Nitride protects active area; field regions get oxidized
- Field oxide grown via **LOCOS** → forms the "bird's beak" profile

**N-Well and P-Well Formation — Masks 2 & 3**
- N-well → houses PMOS; P-well → houses NMOS
- Photoresist patterned, dopant implanted, then driven in at high temperature

**Formation of the Gate — Mask 4**
- Gate is the most critical MOSFET terminal (sets Vt)
- `Vt = Vto + γ(√|−2Φf + Vsb| − √|−2Φf|)`
- Boron implant (~60 keV) tunes doping under gate
- Old oxide stripped (HF) and fresh high-quality gate oxide regrown

**Lightly Doped Drain (LDD) Formation**
- P⁻ implant on NMOS side, N⁻ implant on PMOS side
- Plasma anisotropic etch forms sidewall spacers
- Reduces hot-carrier effects, softens field near drain junction

**Source and Drain Formation**
- Arsenic implant (~75 keV)
- P⁺ into N-well (PMOS S/D), N⁺ into P-well (NMOS S/D)
- Spacers self-align the heavy implants

**Contacts and Local Interconnect**
- Wafer heated 650–700 °C in N₂ for 60 sec
- Reacts metal with Si → forms TiN, used only for local (in-cell) interconnect

**Higher-Level Metal Formation**
- ~1 µm PSG/BPSG (phosphorus- or boron-doped SiO₂) deposited as ILD
- Via/contact holes opened, Metal 1 deposited and patterned to S/G/D terminals

---

- Additional ILD + via mask(s) → connects up to Metal 2 (and further metals if needed)
- Passivation (overglass) layer deposited to protect the chip
- Bond pad opening mask → exposes pads for wire/bump bonding

*(Active area, N-well, P-well, gate, LDD ×2, S/D ×2, contact/local-interconnect, Metal 1 ≈ 9–10 masks; remaining via/metal/passivation/pad masks complete the commonly cited 16.)*

---

# Lab: CMOS Inverter Layout in Magic

**Open the layout**
```bash
magic -T sky130A sky130_inv.mag
```

**Layers present**

| Layer / Label | Purpose |
|---|---|
| `VPWR` / `VGND` | Power / ground rails |
| `nsubstratecontact`, `nsubstratediff` | N-substrate tap (near VPWR) |
| `pubstratecontact`, `pubstratediff` | P-substrate tap (near VGND) |
| `N-WELL` | PMOS well |
| `pdiff`, `pdcontact` | PMOS diffusion + contact |
| `ndiff`, `ndcontact` | NMOS diffusion + contact |
| `Poly`, `pcontact` | Gate poly + gate contact |
| `licon`, `locali` (li1) | Local-interconnect contact + metal |
| `metal1` | First routing metal |
| `FIXED_BBOX` | Cell bounding box |
| `A` / `Y` | Input / output pins |

- Build order: bounding box → power/ground segments (Metal 1) → logic (wells, diffusion, poly, contacts) — same for any standard cell

**Bounding box**
```tcl
property FIXED_BBOX {0 0 138 272}
```
- Width 1.38 µm × Height 2.72 µm (138 × 272 λ)

**Check a layer/label**
```tcl
% what
Selected mask layers: locali
Selected label(s): "Y" is attached to locali in cell def sky130_inv
```

**Extract SPICE netlist**
```tcl
% extract all              → creates sky130_inv.ext
% ext2spice cthresh 0 rthresh 0   → sets extraction thresholds (include small R/C)
% ext2spice                 → creates sky130_inv.spice
```

**Files generated**
- `sky130_inv.mag` — original layout
- `sky130A-GDS.tech` — technology file
- `sky130_inv.ext` — extracted netlist
- `sky130_inv.spice` — SPICE netlist (ready for ngspice)

**Key takeaway**
- Layout → extract → SPICE netlist closes the loop between physical design and circuit-level characterization.
