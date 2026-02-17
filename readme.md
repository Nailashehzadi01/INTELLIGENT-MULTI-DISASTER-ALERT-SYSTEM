# 🌍 Intelligent Multi-Disaster Alert System

A **Verilog-based FPGA system** that detects earthquakes, fires, floods, and storms in real-time with intelligent priority-based alerting.

---

## 📌 What is This Project?

This is a **complete hardware design** written in Verilog that simulates environmental sensors, detects disasters using adaptive thresholds, and sends alerts via UART. It's designed to run on an FPGA board.

---

## 📂 What's Included?

| File | What It Does |
|------|-------------|
| `intelligent_multi_disaster_system.v` | Main system code (top module + 8 sub-modules) |
| `tb_intelligent_multi_disaster_system.v` | Testbench to verify everything works |

---

## 💻 Software I Used

| Software | Purpose |
|----------|---------|
| **VS Code** | Writing and editing Verilog code |
| **Icarus Verilog** | Compiling and simulating Verilog |
| **GTKWave** | Viewing waveform outputs |
| **EDA Playground** | Online testing and simulation |

---

## 🔧 What Do You Need?

### Hardware (to run on actual FPGA)
- FPGA board (any with ~10K logic cells)
- 100 MHz clock source
- 2 DIP switches (mode selection)
- 8-10 LEDs (status display)
- 1 push button (reset)
- USB-UART module (to see alerts on PC)

### Software (for simulation/testing)
- Icarus Verilog (free) OR ModelSim OR Vivado
- GTKWave (optional - for waveforms)
- Serial monitor (PuTTY, Screen, Arduino Serial Monitor)

---

## 🚀 How to Use

### 1. Download the Files
```bash
git clone https://github.com/yourusername/intelligent-multi-disaster-alert-system.git
cd intelligent-multi-disaster-alert-system
You'll see two files:

intelligent_multi_disaster_system.v

tb_intelligent_multi_disaster_system.v

2. Simulate (Test Without FPGA)
Using Icarus Verilog:
bash
# Compile both files
iverilog -o testbench intelligent_multi_disaster_system.v tb_intelligent_multi_disaster_system.v

# Run simulation
vvp testbench
Expected Output:
text
===========================================
 INTELLIGENT MULTI-DISASTER ALERT SYSTEM 
 COMPREHENSIVE TEST 
===========================================
[TIME: 100] Reset released

=== TEST 1: Normal Operation ===
[Normal] Status: 0x00 | Sensors: 0000 | Alert: 0

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
[ALERT] CRITICAL: Earthquake detected! (takes priority)

=== TEST 6: System Recovery ===
[Recovery] System Healthy: 1

=== TEST 7: UART Transmission Test ===

===========================================
 SIMULATION COMPLETE - ALL TESTS PASSED 
===========================================
3. View Waveforms (Optional)
bash
# After running simulation, you'll see multi_disaster_system.vcd
gtkwave multi_disaster_system.vcd
4. Load onto FPGA (For Real Hardware)
Open Quartus (for Intel/Altera) or Vivado (for Xilinx)

Create new project

Add intelligent_multi_disaster_system.v as source file

Assign pins (see Pin Table below)

Compile and program FPGA

🎮 How to Operate
DIP Switch Settings
Switch 1	Switch 0	Mode	What Happens
0	0	Normal	Everything normal
0	1	Earthquake	Seismic spikes, pressure drops
1	0	Fire	Temperature rises, humidity drops
1	1	Flood	Humidity rises, temperature drops
LED Indicators
LED	What It Shows
LED0	Storm detected
LED1	Flood detected
LED2	Fire detected
LED3	Earthquake detected
LED4-6	Alert priority (0-7)
LED7	Alert active
LED8	System healthy
📡 UART Output
Connect FPGA uart_tx pin to USB-UART module, then open serial monitor at 9600 baud.

Message Format:

First byte: Disaster type (E=Earthquake, F=Fire, L=Flood, S=Storm, N=Normal)

Second byte: Severity (C=Critical, H=High, M=Medium, L=Low)

Examples:

text
E C  (Earthquake Critical)
F H  (Fire High)
L M  (Flood Medium)
S L  (Storm Low)
N L  (Normal)
⚙️ Pin Configuration (For FPGA)
Signal	Direction	Description	Example Pin (DE10-Lite)
sys_clk	Input	100 MHz clock	PIN_P11
reset	Input	Reset button	PIN_B8
simulation_mode[0]	Input	DIP switch 0	PIN_C10
simulation_mode[1]	Input	DIP switch 1	PIN_C11
uart_tx	Output	Serial output	PIN_A9
system_status[7:0]	Output	8 LEDs	PIN_W15, etc.
sensor_indicators[3:0]	Output	4 LEDs	Any LED pins
alert_priority_out[2:0]	Output	3 LEDs	Any LED pins
alert_active	Output	LED	Any LED pin
system_healthy	Output	LED	Any LED pin
(Adjust pins based on your specific FPGA board)

🧪 What the Testbench Tests
The testbench automatically runs these 7 tests:

Test	What It Checks
1	Normal operation - no false alerts
2	Earthquake detection works
3	Fire detection works
4	Flood detection works
5	Multiple disasters - Earthquake gets highest priority
6	System recovers after disaster ends
7	UART transmission sends correct data
⚙️ Key Parameters (You Can Change)
Edit these in intelligent_multi_disaster_system.v:

verilog
// Clock settings (line ~20)
parameter SYS_CLK_FREQ = 100_000_000;  // Change if your FPGA clock is different
parameter BAUD_RATE = 9600;             // Change UART speed

// Threshold settings (line ~150)
reg [7:0] seismic_threshold = 8'd70;    // Earthquake triggers at 70+
reg [7:0] temp_threshold = 8'd60;        // Fire triggers at 60°C+
reg [7:0] hum_threshold = 8'd80;         // Flood triggers at 80%+
reg [7:0] pres_threshold_low = 8'd980;   // Storm below 980 hPa
📁 Files Explained
intelligent_multi_disaster_system.v
This file contains 8 modules:

Module	What It Does
clock_manager	Creates 1MHz and 9600Hz clocks from 100MHz
advanced_sensor_simulator	Generates fake sensor data based on mode
adaptive_comparator	Checks if sensor values exceed thresholds
intelligent_decision_logic	Decides which disaster is happening
dynamic_priority_encoder	Assigns priority to multiple disasters
advanced_uart_tx	Sends alerts over serial
alert_dissemination	Simulates SMS/Email/IoT alerts
system_monitor	Checks if system is working properly
tb_intelligent_multi_disaster_system.v
This is the testbench that:

Generates clock signals

Cycles through all simulation modes

Checks if alerts are correct

Displays results on screen

Creates waveform file (.vcd)

🎯 What You Can Do With This
Learn Verilog - See how a complete system is built

Test on FPGA - Load it and see LEDs light up

Modify for your needs - Add sensors, change thresholds

Use in projects - Disaster monitoring, safety systems

Teach digital design - Example of state machines, modules

❓ Common Issues & Solutions
Problem	Solution
Simulation shows nothing	Check Icarus installation, file paths correct
No UART output	Check baud rate (9600), correct pin connected
LEDs not lighting	Check pin assignments in your FPGA project
Testbench failing	Make sure both files are in same folder
"Module not found" error	Check file names match module names
🔄 Quick Commands Reference
bash
# Clone repository
git clone https://github.com/yourusername/intelligent-multi-disaster-alert-system.git

# Compile
iverilog -o testbench intelligent_multi_disaster_system.v tb_intelligent_multi_disaster_system.v

# Run simulation
vvp testbench

# View waveforms
gtkwave multi_disaster_system.vcd

# Clean up
rm -f testbench multi_disaster_system.vcd
⭐ If this helped you, star the repository!

Built for learning and disaster resilience

text

This README:
- ✅ Tells them what software YOU used
- ✅ Removes license section
- ✅ Removes contact section
- ✅ Short but complete
- ✅ Focuses on practical usage
- ✅ Includes common issues and solutions
- ✅ Has quick commands reference