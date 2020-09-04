# Ultra-low distortion sine wave oscillator

This design is complete, has been built, and works. Fab files can be found in `kicad\fab_v1.0.zip`, and the BOM with DigiKey part numbers in `bom.xlsx`.

Licence: All content, *except* that in the directories `lib` and `uld_osc.pretty`, is made available for reuse under the [Creative Commons BY 4.0 Licence](https://creativecommons.org/licenses/by/4.0/). Content in the directories `lib` and `uld_osc.pretty` was sourced from the component manufacturers and retains its original licence.

## Description

An ultra-low THD+N audio sine wave oscillator, useful for testing audio equipment.

This design is a close derivative and update of the [Vojtěch Janásek schematic](http://www.janascard.cz/PDF/An%20ultra%20low%20distortion%20oscillator%20with%20THD%20below%20-140%20dB.pdf), a Wien bridge oscillator with LDR AGC, with the following changes:

  1. Frequency changed from 2k Hz to 1k Hz.
  2. Discontinued LME49710 replaced with OPA1611.
  3. R15 in the original design (1 Meg in parallel with R3 in this design) removed; R3 reduced to compensate.
  4. Level set resistors (R8, R9, VR2) changed to give a [0.5, 4.0] V_pk ([0.35, 2.8] V_rms) output range.
  5. Reduced R6 to increase TL431 current drive and allow lower voltage rails.


## Construction

### Requirements

  1. 1x double-sided PCB (see `kicad\fab_v1.0.zip` for fab files). This can be purchased from a variety of PCB houses at minimal cost; I use JLCPCB for which 5 PCBs can be had for around 4 USD.
  2. Components (see `bom.xlsx` for part numbers). For all components in the BOM, expect to pay around 45 USD. Some of these components are optional (see below).
  3. A dual rail power source (see [Supply rails](#supply-rails) below).

### Optional components
* Diodes D3 and D4 (Zeners) protect the input rails and can be ommitted if you are sure the rails will not exceed +/- 18 V (the supply limit of the TL081 op amps).
* Capacitors C9 and C10 filter the input rails and can be ommitted, or downgraded in capacity, if you are sure the power supply is of high quality.
* Potentiometer VR2 controls the output level and can be replaced with a fixed resistor if variable output is not needed.


### Modifications

* The output currently uses a BNC plug; this can be changed to whatever is suitable for your application. The output also has limited drive capability, and should not be used with loads of less than 1k ohm at 1 kHz. To improve drive capability, add a buffer stage.
* Oscillation frequency is fixed by C5-C7 and R10, R12, R14 to approximately 1k Hz, and can be changed if required. Variable frequency is in principle possible with this circuit but the design has not been tried.
* Output level is set by R8, R9, and VR2: `V_out ~ 2.5/(VR2+R8)*R9`, and can be changed by varying these components.


### Construction

This is a mixed SMD/TH design, but not a challenging one to build: SMD components are not smaller than 0603 size, and ICs are all SOIC. The board can be easily constructed with solder paste and tweezers via the [reflow skillet](https://hackaday.com/2013/07/28/electric-skillet-reflow-soldering-guide/) or toaster oven methods, or with patience and a fine tipped iron can be done by hand soldering.

One thing to keep in mind is how the IC pins are labelled. Pin 1 is not labelled with a dot on the silkscreen, but rather with a bar. See the following figure for an explanation:
![Pin 1 explanatory figure](doc/pin1.jpg?raw=true "Identifying Pin 1")

### Testing and initial calibration

1. Before powering up, verify there are no bridges between the supply rails. Ideally power up with a current-limited supply, max +/- 16 V, and verify 0 VDC on the output. Power down.
2. Set VR2 to minimum level (fully counterclockwise).
3. Connect a voltmeter across TP1 and TP2, and ideally an oscilloscope to the output.
4. Power up and monitor the TP1-TP2 voltage and oscilloscope output, while *slowly* varying VR1. The AGC loop's time constant is several seconds, so move VR1 slowly. At some point you should see oscillation appear on the output, and an increase in the TP1-TP2 voltage. If this is not observed at all across the VR1 range, disconnect power and check the circuit construction, and that U1 has been inserted in the correct orientation.
5. Slowly adjust VR1 to achieve 2 V across TP1-TP2. The output should be a clean sine.
6. Vary VR2 to confirm that the output level changes.


## Usage

### Supply rails

This circuit requires dual rails. It should work between +/- 9 V and +/- 16 V, and has been tested and confirmed to work at +/- 15 V.

### Usage

After [calibration](#testing-and-initial-calibration), simply power up, wait a few seconds for the AGC to stabilise, then take the sine from the output. Load the output with at least 1k ohm at 1k Hz; if a lower impedance drive capability is needed then add a buffer. Use VR2 to adjust the output voltage.
