# IO Placement Revision, SPICE Deck and CMOS Inverter Characterization

---

#  IO Placement Revision

IO placement deals with deciding the **location and spacing of input/output pins around the chip boundary**.

### Pin Spacing

The spacing between pins can be modified during IO placement.

Two common arrangements are:

**Equidistant Pin Placement**
→ Pins are distributed with approximately equal spacing.

**Stacked / Clustered Pin Placement**
→ Pins are placed closer together or grouped according to connectivity and design requirements.

### Why is pin placement important?

- Avoids routing congestion
- Provides better access to internal logic
- Reduces unnecessary routing distance
- Helps maintain design-rule constraints
- Makes power and signal routing easier

The IO placement can therefore be revised by changing the distance between adjacent pins.

---

# SPICE Deck

A SPICE deck is a text file that describes the circuit, device models, component values, connections and simulation commands required by a SPICE simulator.

For a CMOS inverter, the basic circuit consists of:

```
PMOS
  │
 VDD
  │
 OUT
  │
 NMOS
  │
 GND
```

The input `Vin` is connected to the gates of both PMOS and NMOS devices.

An output load capacitor can be connected at `out`.

---

# Main Contents of a SPICE Deck

A SPICE deck contains:

1. Model descriptions
2. Netlist / component connectivity
3. Component values and dimensions
4. Node identification and naming
5. Input sources
6. Supply voltage
7. Simulation commands
8. Model-library inclusion

---

# Model Description

The SPICE model describes the electrical behaviour of the MOS transistor.

The model contains many technological and electrical parameters.

Important parameters visible in the model include:

| Parameter | Meaning |
|---|---|
| `TNOM` | Nominal temperature |
| `TOX` | Oxide thickness |
| `XJ` | Junction depth |
| `NCH` | Channel doping concentration |
| `VTH0` | Zero-bias threshold-voltage parameter |
| `K1, K2, K3` | Body-effect / threshold-related model parameters |
| `U0` | Carrier mobility-related parameter |
| `VSAT` | Saturation velocity-related parameter |
| `RDSW` | Source/drain resistance-related parameter |

These parameters allow SPICE to model the transistor according to the selected technology.

---

# Technological Parameters

The model shown in the lab contains values such as:

```
TNOM ≈ 27 °C
TOX  ≈ 5.8 × 10⁻⁹ m
XJ   ≈ 1 × 10⁻⁷ m
NCH  ≈ 2.3549 × 10¹⁷
VTH0 ≈ 0.39 V
```

The exact model contains many additional parameters used to accurately represent the MOS devices.

---

# CMOS Inverter Device Dimensions

The initial CMOS inverter used:

$$ W_n = W_p = 0.375\ \mu m $$
$$ L_n = L_p = 0.25\ \mu m $$

Therefore:

$$ \frac{W_n}{L_n} = \frac{W_p}{L_p} = \frac{0.375}{0.25} = 1.5 $$

A second case changes the PMOS width:

$$ W_n = 0.375\ \mu m \qquad W_p = 0.9375\ \mu m $$
$$ L_n = L_p = 0.25\ \mu m $$

Therefore:

$$ \frac{W_n}{L_n} = 1.5 $$

$$ \frac{W_p}{L_p} = \frac{0.9375}{0.25} = 3.75 $$

This allows the effect of transistor sizing on inverter behaviour to be studied.

---

# Static Simulation

Static simulation examines the steady-state DC behaviour of the CMOS inverter.

The input is swept over the supply-voltage range.

Example:

```spice
.dc Vin 0 2.5 0.05
```

This produces the inverter's **Voltage Transfer Characteristic (VTC)**:

- `Vin` → Input voltage
- `Vout` → Output voltage

For a CMOS inverter:

```
Vin LOW
   ↓
NMOS OFF, PMOS ON
   ↓
Vout HIGH
```

```
Vin HIGH
   ↓
PMOS OFF, NMOS ON
   ↓
Vout LOW
```

---

# Switching Threshold – Vm

The switching threshold voltage, **Vm**, is the point on the inverter VTC where:

$$ V_{in} = V_{out} $$

It represents the approximate transition point between the logic-low and logic-high operating regions.

From the lab results:

**Case 1:**

$$ \frac{W_n}{L_n} = 1.5 \qquad \frac{W_p}{L_p} = 1.5 $$

$$ V_m \approx 0.98\ V $$

**Case 2 (second sizing):**

$$ \frac{W_n}{L_n} = 1.5 \qquad \frac{W_p}{L_p} = 3.75 $$

$$ V_m \approx 1.2\ V $$

Thus, changing the relative strength of PMOS and NMOS changes the switching threshold.

---

# Why Does Vm Change?

The switching point depends on the relative drive strength of PMOS and NMOS.

```
Increase PMOS strength
        ↓
PMOS can pull the output HIGH more strongly
        ↓
Higher input voltage is required for NMOS to dominate
        ↓
Vm shifts upward
```

Therefore, transistor sizing can be used to modify inverter switching behaviour.

---

# Static Behaviour Evaluation

The DC VTC is also used to evaluate the robustness of the CMOS inverter.

Important observations include:

- Switching threshold
- Logic HIGH level
- Logic LOW level
- Transition region
- Relative PMOS/NMOS strength

The point:

$$ V_{in} = V_{out} $$

is especially important for determining Vm.

---

# Dynamic Simulation

Dynamic simulation studies the time-dependent behaviour of the CMOS inverter.

Unlike DC analysis, the input changes with time.

A larger load generally makes the output transition slower.

---

# Static vs Dynamic Simulation

| Static Simulation | Dynamic Simulation |
|---|---|
| Studies DC behaviour | Studies time-domain behaviour |
| Uses DC sweep | Uses time-varying input |
| Produces VTC | Produces waveform |
| Used to find Vm | Used to measure delay and transition |
| Focuses on steady-state behaviour | Focuses on transient behaviour |

---

# SPICE Waveform

The SPICE waveform for the inverter shows the output changing as the input voltage changes.

For the CMOS inverter:

```
Vin ↑
   ↓
Vout ↓
```

Therefore the inverter has an inverting characteristic:

- LOW input → HIGH output
- HIGH input → LOW output

The transition region becomes visible around the switching threshold.

---

# Effect of PMOS Sizing

The lab compares two inverter configurations:

**Configuration 1**

$$ W_n = W_p = 0.375\ \mu m $$

**Configuration 2**

$$ W_n = 0.375\ \mu m \qquad W_p = 0.9375\ \mu m $$

Comparison:

```
        Wp/Lp
          ↑
     PMOS strength
          ↑
   Switching behaviour
          ↑
         Vm
```

The experiment demonstrates that transistor sizing affects the CMOS inverter's voltage-transfer characteristics and switching threshold.

---

# Lab – Opening CMOS Inverter Layout Using Magic

The CMOS inverter physical layout can be opened in Magic Layout using the technology file.

General command:

```bash
magic -T sky130A sky130_inv.mag
```
---

The circuit-level behaviour and physical layout together form the basis of a standard-cell design.

---

# Key Takeaways

1. IO placement determines the location and spacing of chip pins.

2. Pin spacing can be revised to improve routing and reduce congestion.

3. A SPICE deck describes circuit connectivity, device values, models, sources and simulation commands.

4. Model descriptions contain technology-specific MOS parameters.

5. A CMOS inverter consists of a PMOS pull-up network and NMOS pull-down network.

6. Static DC simulation is used to generate the VTC and determine the switching threshold Vm.

7. Vm is the point where:

   $$ V_{in} = V_{out} $$

8. Changing PMOS/NMOS sizing changes the switching threshold.

9. Dynamic simulation studies time-dependent behaviour such as propagation delay and rise/fall transitions.

10. The lab compares:

    $$ \frac{W_n}{L_n} = \frac{W_p}{L_p} = 1.5 $$

    with

    $$ \frac{W_n}{L_n} = 1.5 \quad \text{and} \quad \frac{W_p}{L_p} = 3.75 $$

11. The observed Vm values are approximately 0.98 V and 1.2 V respectively.

12. Magic is used to view the physical CMOS inverter layout.

13. Example:

    ```bash
    magic -T sky130A sky130_inv.mag
    ```

14. SPICE simulation verifies electrical behaviour, while Magic represents the physical implementation.
