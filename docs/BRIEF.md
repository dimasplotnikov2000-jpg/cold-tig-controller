# Cold tig controller

An Arduino Nano based Cold TIG welding controller board designed to fit a 100 by 50 mm panel (2 layers PCB).

Power Supply Constraints:
- The board is powered by an external stable +5V DC power supply.
- There is NO dedicated screw terminal block for power on the PCB. External 5V and GND wires will connect directly to the existing 5V and GND pins of the Arduino Nano board.

Component Package and Footprint Constraints:
- Microcontroller: Arduino Nano (through-hole DIP module footprint).
- Transistors: Three NPN transistors in SOT-23 surface-mount package (marking "2X", matching MMBT4401 or SST4401).
- Resistors: All resistors must use 0603 SMD packages.
- Capacitors: All decoupling and filtering capacitors must use 0603 SMD packages.
- Status LEDs: Three 3mm through-hole (DIP) LEDs.
- Buttons: Four tactile push buttons, through-hole (DIP), 5mm diameter with 2 pins.
- Optocouplers: Two PC817 optocouplers.
- Connectors: Screw terminal blocks for external connections (except power).

Hardware Design and Connections:
1. Display: 3-digit 7-segment common cathode LED display (part number: 3631AGG). 
   - Segments A, B, C, D, E, F, G connected to Arduino pins D9, D10, D11, D12, D13, A2, A3 respectively through 0603 220 Ohm current-limiting resistors.
   - Digits DIG1, DIG2, DIG3 (cathodes) driven via the three SOT-23 NPN transistors controlled by Arduino pins A4, A5, A6 to protect MCU pins.
2. User Interface: 
   - Four 5mm 2-pin tactile buttons connected to GND with internal/external pull-ups: Torch Button (D2), Mode Button (D3), Plus Button (D4), Minus Button (D5).
   - Three 3mm DIP status LEDs with 0603 220 Ohm series resistors connected to Arduino pins D6 (Mode 1), D7 (Mode 2), D8 (Mode 3) to ground.
3. Output Interface (Welding Machine Trigger):
   - One PC817 optocoupler. Arduino pin A0 (D14) drives the optocoupler's internal LED anode through an 0603 resistor. The phototransistor output (Collector/Emitter) connects to a screw terminal block for the torch switch line. 
   - Add an 0603 100 Ohm protection resistor in series with the collector.
4. Input Interface (Arc Feedback from SG3525 PWM pin 10):
   - One PC817 optocoupler. The input side connects to a screw terminal block for the welding machine's SG3525 chip (Anode to pin 10 through an 0603 1k Ohm resistor, Cathode to pin 12/GND of the machine).
   - The phototransistor output side connects to Arduino pin A1 (D15) with a pull-up resistor.

High-Frequency (HF) Oscillator Noise and Interference Protection:
- Completely isolate the welding machine's electrical ground from the Arduino's ground. 
- Place an 0603 0.1uF (104) ceramic decoupling capacitor physically close to the Torch Button input pin D2 on the board.
- Place an 0603 0.1uF ceramic capacitor directly across pins 1 and 2 of the input feedback optocoupler.
- Place an 0603 0.1uF ceramic capacitor directly across pins 3 and 4 of the input feedback optocoupler.
- Place an 0603 0.1uF ceramic capacitor directly across the output pins of the output trigger optocoupler.
- Keep all high-frequency signal traces as short as possible. Use a solid ground plane for the Arduino Nano section.
