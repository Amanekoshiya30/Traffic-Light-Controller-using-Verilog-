# Traffic-Light-Controller-using-Verilog-
This project implements a real-world Traffic Light Controller based on Indian road conditions using Verilog HDL and Vivado 2019.1. The design uses a finite state machine (FSM) to control North–South, East–West, and pedestrian lights in a realistic timing sequence.

## Features
- FSM-based light transitions (Green → Yellow → Red)
- Pedestrian signal active only when both directions are red
- Clock division from 100 MHz → 8 MHz → 1 Hz using Clock Wizard and custom divider
- Behavioral simulation in Vivado
