Intelligent Multi-Disaster Alert System
A Verilog-based FPGA system that detects earthquakes, fires, floods, and storms in real-time with intelligent priority-based alerting.

Files
This repository contains two files:

intelligent_multi_disaster_system.v - Main system code with 8 modules

tb_intelligent_multi_disaster_system.v - Testbench for verification

Software Used
VS Code (code editor)

Icarus Verilog (compiler and simulator)

GTKWave (waveform viewer)

Requirements
Hardware (for FPGA implementation)
FPGA board with 10K+ logic cells

100 MHz clock source

2 DIP switches

8-10 LEDs

1 push button

USB-UART module

Software (for simulation)
Icarus Verilog

VS Code (optional)

GTKWave (optional)

Quick Start
1. Download Files
text
intelligent_multi_disaster_system.v
tb_intelligent_multi_disaster_system.v
2. Open Terminal
Navigate to the folder containing both files.

3. Compile
bash
iverilog -o testbench intelligent_multi_disaster_system.v tb_intelligent_multi_disaster_system.v
4. Run Simulation
bash
vvp testbench
5. Expected Output
text
===========================================
 SIMULATION COMPLETE - ALL TESTS PASSED 
===========================================
Simulation Output Details
When you run the simulation, you will see:

text
===========================================
 INTELLIGENT MULTI-DISASTER ALERT SYSTEM 
 COMPREHENSIVE TEST 
===========================================
[TIME: 100] Reset released

=== TEST 1: Normal Operation ===
[Normal] Status: 0x00 | Sensors: 0000 | Alert: 0 (Priority: 000)
[Normal] System Healthy: 1

=== TEST 2: Earthquake Detection ===
[SENSOR] Earthquake threshold breached!
[ALERT] CRITICAL: Earthquake detected!
[UART] Received: 0x45 ('E')
[UART] Received: 0x43 ('C')

=== TEST 3: Fire Detection ===
[SENSOR] Fire threshold breached!
[ALERT] HIGH: Fire detected!
[UART] Received: 0x46 ('F')
[UART] Received: 0x48 ('H')

=== TEST 4: Flood Detection ===
[SENSOR] Flood threshold breached!
[ALERT] MEDIUM: Flood detected!
[UART] Received: 0x4C ('L')
[UART] Received: 0x4D ('M')

=== TEST 5: Multiple Disasters ===
[ALERT] CRITICAL: Earthquake detected!

=== TEST 6: System Recovery ===
[Recovery] System Healthy: 1

=== TEST 7: UART Transmission Test ===

===========================================
 SIMULATION COMPLETE - ALL TESTS PASSED 
===========================================
Operating Modes
Set the two DIP switches to select mode:

SW1	SW0	Mode	Sensor Behavior
0	0	Normal	All sensors normal
0	1	Earthquake	Seismic: 80-100, Pressure drops
1	0	Fire	Temperature: 85-100°C, Humidity drops
1	1	Flood	Humidity: 90-100%, Temperature drops
LED Indicators
LED	Signal	Meaning When ON
LED0	sensor_indicators[0]	Storm detected
LED1	sensor_indicators[1]	Flood detected
LED2	sensor_indicators[2]	Fire detected
LED3	sensor_indicators[3]	Earthquake detected
LED4	alert_priority_out[0]	Priority bit 0
LED5	alert_priority_out[1]	Priority bit 1
LED6	alert_priority_out[2]	Priority bit 2
LED7	alert_active	Alert is active
LED8	system_healthy	System working fine
UART Output
Connect to serial monitor at 9600 baud.

Format: First byte = Disaster, Second byte = Severity

Output	Meaning
E C	Earthquake Critical
F H	Fire High
L M	Flood Medium
S L	Storm Low
N L	Normal
Pin Connections (Example for DE10-Lite)
Signal	Direction	Pin
sys_clk	Input	PIN_P11
reset	Input	PIN_B8
simulation_mode[0]	Input	PIN_C10
simulation_mode[1]	Input	PIN_C11
uart_tx	Output	PIN_A9
system_status[7:0]	Output	LED pins
sensor_indicators[3:0]	Output	LED pins
alert_priority_out[2:0]	Output	LED pins
alert_active	Output	LED pin
system_healthy	Output	LED pin
File Structure Explained
intelligent_multi_disaster_system.v
This file contains 8 modules:

Module	Line	Function
clock_manager	10	Generates 1MHz and 9600Hz clocks
advanced_sensor_simulator	60	Creates sensor data based on mode
adaptive_comparator	150	Checks thresholds
intelligent_decision_logic	250	Determines disaster type
dynamic_priority_encoder	330	Sets alert priority
advanced_uart_tx	410	Sends serial data
alert_dissemination	520	Simulates SMS/Email
system_monitor	570	Checks system health
top module	640	Connects all modules
tb_intelligent_multi_disaster_system.v
This testbench:

Generates 100MHz clock

Runs 7 test cases

Monitors UART output

Displays results

Creates waveform file

Test Cases
Test	Duration	What It Verifies
1	2000 ns	No false alerts in normal mode
2	3000 ns	Earthquake detection works
3	3000 ns	Fire detection works
4	3000 ns	Flood detection works
5	4000 ns	Earthquake prioritized over others
6	2000 ns	System recovers after disaster
7	5000 ns	UART sends correct data
Configurable Parameters
Edit these in intelligent_multi_disaster_system.v:

verilog
// Clock settings (line 20)
parameter SYS_CLK_FREQ = 100_000_000;  // Change if your clock is different
parameter BAUD_RATE = 9600;             // Change UART speed

// Threshold values (line 150)
reg [7:0] seismic_threshold = 8'd70;    // Earthquake at 70+
reg [7:0] temp_threshold = 8'd60;        // Fire at 60°C+
reg [7:0] hum_threshold = 8'd80;         // Flood at 80%+
reg [7:0] pres_threshold_low = 8'd980;   // Storm below 980

// Confirmation counts (line 180)
// Earthquake: 5 readings, Fire: 4, Flood: 3, Storm: 2
Common Issues
Problem	Solution
iverilog: command not found	Install Icarus Verilog
No output in terminal	Check both files are in same folder
Test fails	Use original files without modifications
UART shows garbage	Set baud rate to 9600
Waveforms not showing	Install GTKWave
VS Code Setup
Install VS Code

Install "Verilog" extension (mshr-h.Verilog)

Open folder with both files

Open terminal (Ctrl + `)

Run simulation commands

Commands Summary
bash
# Clone repository
git clone https://github.com/yourusername/intelligent-multi-disaster-alert-system.git

# Enter folder
cd intelligent-multi-disaster-alert-system

# Compile
iverilog -o testbench intelligent_multi_disaster_system.v tb_intelligent_multi_disaster_system.v

# Run
vvp testbench

# View waveforms (optional)
gtkwave multi_disaster_system.vcd

# Clean up
rm -f testbench multi_disaster_system.vcd
What You Will Learn
Writing Verilog modules

Creating testbenches

State machine design

UART communication

Priority encoding

Digital simulation

FPGA implementation

Checklist
Downloaded both files

Installed Icarus Verilog

Compiled successfully

Saw "ALL TESTS PASSED"

Tried different modes

Connected to FPGA (optional)