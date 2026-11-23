<div align="center">

# openWB

### Wideband controller · Hardware based on SLC Free

A revised component selection and design changes aimed at making the hardware easier to use.

<img width="1284" height="825" alt="image" src="https://github.com/user-attachments/assets/0b3c1f1c-1134-49bc-8306-e82a3570743c" />


</div>

---

## About the project

**openWB** is a hardware project for a wideband lambda sensor controller, based on **14Point7's SLC Free**. This type of controller allows the air/fuel mixture to be monitored using an oxygen sensor.

The project adapts the SLC Free design with **a different selection of components and changes to the board design**, aiming to make it easier to use and integrate. This repository contains the electrical schematic, PCB layout, and exported project outputs from Altium Designer.

## Design changes

| Area | openWB approach |
| --- | --- |
| **Components** | A component selection that differs from the reference design. |
| **Board design** | Changes to the circuit and physical implementation to accommodate the project's design choices. |
| **Usability** | Adaptations intended to make the hardware more practical to use and integrate. |
| **Project files** | Editable Altium Designer schematic and PCB files, alongside fabrication and assembly outputs. |

The schematic, PCB, and bill of materials document the actual openWB implementation. Electrical compatibility, pin assignments, and firmware requirements should be checked for this implementation; its SLC Free origins alone do not establish that these are identical to the original project.

## 0–5 V analog output

openWB provides a **0–5 V wideband analog output** for interfacing with an ECU, data logger, or another device with a compatible analog input. The signal is named **`wb_sig`** in the schematic and is routed to connector **J1**.

This output represents the measured mixture as a voltage. The receiving device converts that voltage into lambda or AFR using the controller's output calibration. This makes it possible to record mixture data alongside other engine parameters or display it through the receiving device.

- Connect `wb_sig` to a compatible **0–5 V analog input**.
- Use the schematic's signal-ground connection as the measurement reference and account for the separate `pwr_gnd` heater return when planning the wiring.
- Configure the ECU or logger with the **voltage-to-lambda/AFR relationship used by the installed firmware**.
- Check warm-up and fault behavior before interpreting the output as a valid mixture reading.

**Output scaling:** the repository does not currently specify the AFR or lambda values corresponding to 0 V and 5 V. These must be confirmed from the firmware and calibration used on the board. A 0–5 V range alone does not define the conversion, and 2.5 V should not automatically be interpreted as stoichiometric.

For a firmware configuration with a linear lambda output, the receiver would use:

```text
lambda = lambda_at_0V + (Vout / 5) × (lambda_at_5V - lambda_at_0V)
AFR    = lambda × fuel_stoichiometric_AFR
```

The endpoint values must come from the actual firmware configuration. AFR also depends on the fuel's stoichiometric ratio; lambda expresses the mixture relative to that ratio.

## Schematic overview

The circuit is contained in [`main.SchDoc`](main.SchDoc). Its main functional blocks are:

### Controller and sensor interface

**IC1, CY8C27443-24PXI**, is the central programmable controller. The CY8C27xxx PSoC family combines processing with configurable analog and digital resources, making it suitable for implementing the control and signal-processing functions of this design. See the [Infineon family datasheet](https://www.infineon.com/assets/row/public/documents/30/49/infineon-cy8c27143-cy8c27243-cy8c27443-cy8c27543-cy8c27643-psoc-programmable-system-on-chip-datasheet-en.pdf) for device details.

The schematic exposes the sensor-interface nets `LSU_IP`, `LSU_IA`, `LSU_VGND`, and `LSU_NERMST` (spelled this way in the project). These connect the controller and surrounding passive network to the sensor connector. The exact supported sensor model and required firmware should be confirmed before selecting a sensor.

### Sensor heater drive

**Q1, IRF3710PBF**, is the N-channel power MOSFET in the heater switching stage. The schematic connects this stage to `LSU_HEATER-` and `pwr_gnd`, while `LSU_HEATER+` is connected to the incoming supply. Heater operation depends on the controller firmware. Device information is available from [Infineon](https://www.infineon.com/part/IRF3710).

### Power supply and protection components

The input supply is labeled **`12v_in`**. **IC2, LM7805**, provides the board's regulated **5 V rail**, with bulk and local decoupling capacitors around the supply and controller. The design also includes **three P6KE24A TVS diodes** and a series resistor in the supply section.

The `12v_in` label describes the intended nominal supply; it is not a specification of the complete board's allowable voltage range or automotive transient immunity.

### External connections

**J1** brings together the sensor connections, analog output, power, and ground nets. **J2** exposes `5v`, `GND`, `XRES`, `SCL`, and `SDA` for access to the controller's programming/control signals. These net names do not establish a supported external communications protocol without the corresponding firmware.

Use the pin numbers in the schematic when making a harness. PCB orientation and connector mating views can differ, so this overview is not a substitute for a numbered wiring diagram.

## Main components

The following parts are listed in the project's schematic and assembly export:

| Reference | Component / value | Role |
| --- | --- | --- |
| **IC1** | CY8C27443-24PXI | Main PSoC controller, using a 28-pin DIP footprint. |
| **IC2** | LM7805 | Regulated 5 V supply. |
| **Q1** | IRF3710PBF | N-channel MOSFET for the sensor heater stage. |
| **D1–D3** | P6KE24A | TVS protection components. |
| **J1** | 43045-1200 | Main external connector. |
| **J2** | 61900519521 | Controller access connector. |
| **C1, C3** | UHE2A470MPD | Polarized bulk capacitors. |
| **C2, C4, C5, C7, C8, C10** | 100 nF | Local decoupling and filtering capacitors. |
| **C6** | 1 µF | Capacitor in the sensor-interface network. |
| **R3** | 61.9 Ω | Resistor in the `LSU_IP` interface path. |
| **R2, R5, R6, R8, R9, R12, R13** | 1 kΩ | Resistors distributed across the signal and drive circuitry. |
| **R4, R10** | 10 kΩ | Resistors in the sensor-interface and controller circuitry. |
| **R7** | 120 kΩ | Resistor in the sensor-interface network. |
| **R1** | CRCW121010R0FKEA | Series resistor in the supply section; verify the part specification when ordering. |

Many of the passive components use **1206 SMD footprints**, while the controller, power devices, connectors, and bulk capacitors use through-hole footprints. This mixed assembly approach leaves the larger parts accessible and uses relatively large SMD passives for assembly and rework.

The table is an overview, not a replacement for the [full BOM](Project%20Outputs%20for%20Free%20Documents/BOM/Bill%20of%20Materials-openWB.xlsx). Check values, tolerances, voltage ratings, packages, and manufacturer part numbers before ordering. In particular, R1's generic description in the pick-and-place export conflicts with its part number and should not be used as its purchasing specification.

## PCB and assembly

The repository includes top and bottom copper Gerbers, solder mask, paste and legend layers, a board profile, and plated/non-plated drill outputs. Pick-and-place files provide component references, coordinates, rotation, and board side for assembly.

When preparing a build:

1. Review the schematic and verify the selected sensor and firmware combination.
2. Check the BOM against the PCB footprints and intended component substitutions.
3. Review the electrical and PCB design-rule checks in Altium.
4. Regenerate fabrication and assembly files from the same project revision.
5. Verify the 5 V rail, firmware, heater operation, and analog-output calibration during bring-up.

## Project files

| File / folder | Contents |
| --- | --- |
| [`openWB.PrjPcb`](openWB.PrjPcb) | Main Altium Designer project. |
| [`main.SchDoc`](main.SchDoc) | Electrical schematic. |
| [`main.PcbDoc`](main.PcbDoc) | PCB layout and routing. |
| [`Job3.OutJob`](Job3.OutJob) | Output generation configuration. |
| [`Project Outputs for openWB/`](Project%20Outputs%20for%20openWB/) | Gerbers, drill files, pick and place files, and other exported outputs. |
| [`Project Outputs for Free Documents/`](Project%20Outputs%20for%20Free%20Documents/) | Bill of materials and additional assembly files. |
| [`History/`](History/) | Local revision history saved by Altium. |
| [`Project Logs for openWB/`](Project%20Logs%20for%20openWB/) | Change logs generated by Altium. |

### Quick access

- **[Bill of materials — BOM](Project%20Outputs%20for%20Free%20Documents/BOM/Bill%20of%20Materials-openWB.xlsx)**
- **[Pick and place — CSV](Project%20Outputs%20for%20openWB/Pick%20Place/Pick%20Place%20for%20main.csv)**
- **[Exported document — PDF](Project%20Outputs%20for%20openWB/Job4.PDF)**

## Getting started

1. Clone the repository:

   ```bash
   git clone https://github.com/gongas123lol/openWB.git
   ```

2. Open `openWB.PrjPcb` in **Altium Designer**.
3. Review `main.SchDoc` to understand the circuit and its connections.
4. Open `main.PcbDoc` to explore component placement and board layout.
5. Consult the BOM and exported outputs to review the components and assembly information.

If you modify the project, regenerate the outputs from the revision you intend to manufacture. The exported files included in this repository are snapshots and may not reflect later changes to the schematic or PCB.

## Technical documentation

This repository focuses on **hardware**. It does not currently include firmware source code or a complete setup, calibration, and wiring guide. The schematic and BOM are the starting point for checking the components, connectors, and connections used in this version.

## Origins and credits

openWB is based on **SLC Free**, developed by **14Point7**. Credit for the reference design belongs to its original authors; openWB brings together this project's component choices and design adaptations.

- [14Point7 — SLC Free documentation, source code, and design files](https://www.14point7.com/pages/software-and-documentation)
- [openWB repository](https://github.com/gongas123lol/openWB)

---

<img width="1440" height="616" alt="image" src="https://github.com/user-attachments/assets/09f6f173-c1d9-4579-a4f6-a6408e888bea" />

