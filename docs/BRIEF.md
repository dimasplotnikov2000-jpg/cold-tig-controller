# Cold tig controller

# STRICT PCB ROUTING BRIEF (Skip Part Selection / Skip BOM Generation)
# Target: Generate ONLY the 2-layer PCB layout and Gerber tracks view. 
# Dimensions: 100mm x 50mm.

## STRICT OPERATIONAL COMMAND:
- DO NOT use any part-selection, BOM, or component validation tools.
- DO NOT search for manufacturer part numbers (MPNs).
- Skip all documentation stages and proceed IMMEDIATELY to schematic netlist binding and PCB layout routing.
- Use generic standard footprints only.

## Component Footprints Configuration
- Microcontroller: 1x Arduino Nano (Standard through-hole DIP-30 module package).
- Transistors: 3x Generic NPN in SOT-23 surface-mount package.
- Passive Components: All resistors and capacitors must use standard 0603 SMD footprints.
- User Interface: 3x 3mm through-hole LEDs, 4x 2-pin 5mm tactile switches.
- Connectors: 2x 2-pin screw terminals with 5.08mm pitch for external lines.
- Optocouplers: 2x PC817 in standard DIP-4 or SMD-4 package.

## Netlist and Trace Connections
1. Display Tracks (3-Digit Common Cathode):
   - Route tracks from Arduino Nano pins D9, D10, D11, D12, D13, A2, A3 through 0603 resistors (220 Ohm value) directly to display segments A, B, C, D, E, F, G.
   - Route display multiplexing digits DIG1, DIG2, DIG3 to the Collectors of the 3x SOT-23 NPN transistors.
   - Route Arduino Nano pins A4, A5, A6 to the Bases of these transistors via 0603 resistors. Emitters to Arduino GND.

2. UI & Controls Tracks:
   - Route 4x 5mm buttons between Arduino pins D2, D3, D4, D5 and GND.
   - Route 3x 3mm LEDs from Arduino pins D6, D7, D8 to GND via 0603 series resistors.

3. Isolated Output Trigger Tracks:
   - Route Arduino pin A0 (D14) through an 0603 resistor to the PC817 internal LED input.
   - Route the PC817 output phototransistor directly to the first 2-pin screw terminal block. Place one 0603 protection resistor inline with the Collector track.

4. Isolated Arc Feedback Input Tracks:
   - Route the second 2-pin screw terminal block directly to the second PC817 input LED pins.
   - Route the phototransistor output to Arduino pin A1 (D15) with an 0603 pull-up resistor connection to the Nano 5V pin.

## High-Frequency Noise Isolation & Routing Rules
- Ground Plane Policy: Separate the PCB into two distinct zones. Fill the entire Arduino Nano digital section with a solid GND copper pour. The welding machine terminal section must be completely isolated (no ground sharing, no copper tracks crossing the barrier except inside the optocouplers).
- Decoupling Capacitors Placement:
  - Route one 0603 0.1uF capacitor directly adjacent to the Torch Button pin D2 track.
  - Route one 0603 0.1uF capacitor directly across the input pins (1 and 2) of the feedback optocoupler.
  - Route one 0603 0.1uF capacitor directly across the output pins (3 and 4) of the feedback optocoupler.
  - Route one 0603 0.1uF capacitor directly across the trigger optocoupler output terminal block pins.
- Track Width: Use thick traces for the terminal blocks and power lines, and standard signal traces for the Arduino digital lines. Keep HF lines as short as possible.
