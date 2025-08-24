### CHPC: Cheap Heat Pump Controller v1.x
<b>The CHPC a minimal cost Heat Pump (HP) controller, which can be used as provided, or can be adopted to nearly all use cases due to open source nature.</b>
<br><br>
<b>Note that the program in the current version, but still unfinished, occupies more than 110% of the Flash memory of the Arduino Pro mini.
Therefore, I have divided [depending on the FIRST_USE directive] the program into a part for programming the Dallas thermoments and a program for operating the heat pump.<br><br>
Procedure: first program the Dallas thermometers by setting the [FIRST_USE] directive. After programming the thermometers, the [FIRST_USE] directive should be commented out and the Arduino Pro mini should be programmed again. This will free up more than 3kB of Flash memory!</b> (Waldek)
<br><br>

## Real life installation.
Works from ground heat collectors (loops) to radiant in-floor heating system.

 ![Installation example](./m_CHPC_i2.jpg)

Driving:
- EEV, 
- Heat Pump Compressor (1kW electrical power),
- Circulating Pumps,
- Compressor Heater,
- Three-way valve for domestic hot water heating,
- Four-way reversing valve for chilled water cooling.

Temperature sensors installed:
- Before/After Evaporator,
- Cold In/Cold Out,
- Hot In(used as Target)/Hot Out,
- Outdoor temperature,
- Compressor,
- Domestic hot water tank.

Controlled via both RS-485 and 16x2 display with buttons.

## Changelog
- 13 Apr, 2019: EEV support development started
- 16 Apr, 2019: Standalone EEV (no thermostat) with only 2 T sensors written and debugged
- 30 Apr, 2019: HP system updated to CHPC
- 01 May, 2019: CHPC fully tested and released
- 02 May, 2019: PCB rev.1.3 coming up, main feature: a lot of DS18B20 inputs
- 17 May, 2019: <b>PCB 1.3 tested, [assembly instructions added](https://github.com/gonzho000/chpc/wiki/assembly)</b>
- 24 Nov 2022: (Waldek)
  - added half-steps to the EEV increasing accuracy
  - delay added, Cold WP turns on first and compressor turns on later
  - same when the compressor is turned off, after [COMPRESSOR_DELAY] shuts down Cold WP
- 15 sep 2023: (Waldek)
  - added heat pump cooling capability (four-way reversing valve needed)
  - [RELAY_4WAY_VALVE] relay controls [valve4w_state]
  - moved [T_setpoint_cooling] to eeprom
  - added possibility to heat domestic hot water (three-way valve needed)
  - [RELAY_SUMP_HEATER] relay controls [valve_cwu_position] ATTENTION!!!!! No sump heating in this!!!!! The heat pump must stand in a room with a minimum temperature of 10 degrees Celsius!!!!!
  - [T_TARGET_CWU] was moved to eeprom, [CWU_INTERVAL] - every how many hours in milliseconds to start a water heating cycle, [CWU_MAX_HEATING_TIME] - how many hours in milliseconds to heat water in one cycle
  - added [CWU_HYSTERESIS] and moved to eeprom
  - transferred [T_EEV_setpoint] to eeprom
  - overload error handling
  - lack of start handling
  - added buffer charging procedure
- 10 Apr 2023: version 1.9w released (Waldek)
  - easy MENU (like XD old Nokia 3310)
- 16 Aug 2025: version 2.0w relased (Waldek)
  - eevise procedure rewritten
  - DHW and buffer handling added to the menu, as well as a procedure for manual resetting of the heat pump/errors
  - corrections of several minor bugs
<br><br>
## Applications:
| Usage. |	Brief description. | 	Application examples	| Available protections	|
| ---------- | ------------------ | ------------------ | -------------------- |
| 1. Thermostat.	|  Precision thermostat. Simple and cheap. Only one relay and one temperature sensor required.<br> | Room heat control. A chicken coop climate control. Distillation column. Else. | N/A	|
| 2. Heat pump (HP) control. | Controller drives HP system components: compressor, Cold and Hot side Circulating Pumps (CP). Protect system from an overload, overheat and freezing up. Drives EEV to optimize running conditions. | DIY heat pump system. Repair module for commercial system. Water heater, house heating systems and same applications. | Compressor: cold start or overheat. Discharge and suction lines protection. Short-term power loss. Anti-freeze. Power overload protection. |
| 3. EEV controller. | Only drives EEV, no relays. Require two T sensors. | Upgrade your system from capillary tube to EEV. | Protects from liquid at suction line by design. |

For more information about Heap Pumps look at [Wikipedia about HP](https://en.wikipedia.org/wiki/Heat_pump)
<br><br>
## Features:
- Up to 13 T sensors (see "T sensor abbreviations" for full list)
- 5 relays (Compressor, Hot CP or Air Fun, Cold CP or Air Fun, Compressor Heater, 4-way valve)
- 4 inputs
- 5/6 pin EEV connection,
- 1602 display support
- RS485 or Serial(UART 5V) support
- Automatically turns on/of system when heating required
- Takes care of system components health
- On board or off board power supply
<br><br>
## Control interfaces:
 <b>None:</b> Target temperature uploaded to board with firmware and not changed anymore. System used as an fixed thermostat. You can change target temperature with firmware re-upload.<br>
 <b>0.96 OLED or 1602 LCD screen + buttons:</b> Simple, local screen controlled system. <br>
 <b>Remote computer terminal over RS-485 line. </b> Target temperature and running conditions under remote control. A user can get stats from all T sensors. Up to 1.2 kilometer line.\*<br>
 <b> Remote automated control/stats via RS-485.</b> Firmware was written with python scripting in mind (and real scripts at the prototype 485 network).<br>
 <b> Both screen + buttons and RS-485.</b> Combination allowed.
 
\* RS-485 specification. The hardware test succeeded on 400 meters line.

Example: day/night setpoint control and data visualization with JSON communication way.
![graph example](./m_t_graph_example.png)
<br><br>

## Relays:
### "Thermostat":
Only one Relay: drive an electric heater (any) 
### "Heat Pump". Capillary tube, TXV, EEV:
5 Relays, drives all you need:
* Compressor (relay can be used as external relay driver for High Power systems)
* Cold Circulating Pump (CP)
* Hot CP
* Compressor Heater (optional, recommended for outdoor HP installations)
* and one reserved to support 4-way Valve
<br><br>
## Temperature sensors:
* Up to 13 temperature sensors can be connected to CHPC to control all processes that you want. 
* Only 1 sensor needed for "Thermostat" or "Heat Pump capillary/TXV" 
* 3 sensors needed for "HP with EEV" (absolute minimum scheme)
<br><br>
## Temperature sensors installation example (medium scheme)
![medium scheme](./m_HeatPump_t_sensors_med.png)
<br><br>
 ## Get your own CHPC:
* download PCB Gerber file, *CHPC_v1.x_PCB_Gerber.zip*
* search google [where to order PCB](https://www.google.com/search?q=order+pcb) or make your own at CNC machine
* order electronic components, see BOM (Bill Of Materials) list, *CHPC_v1.x_PCB_BOM.html*
* solder, [assembly instructions here](https://github.com/gonzho000/chpc/wiki/assembly)
* install firmware *CHPC_firmware.ino*
* install CHPC at your system
* enjoy
<br><br>
## T sensor abbreviations:
These abbreviations used in the interface during sensors installation procedure

| Abbr. | Full name | Required for |
| ----- | -------------------- | -------------------- |
| Tae | after evaporator | EEV <br>Anti-liquid protection at suction line |
| Tbe | before evaporator | EEV |
| Ttarget | target | Thermostat<br>Thermostat+EEV |
| Tsump | sump | Automatic Compressor Heater<br>Compressor  overheat protection |
| Tci | cold in | Antifreeze protection |
| Tco | cold out | Antifreeze protection |
| Thi | hot in |  Hot CP automatic control   |
| Tho | hot out | Overheat protection     |
| Tbc | before condenser | Discharge overheat protection           |
| Tac | after condenser |          |
| Touter | outer (outdoor) |          |
| Tcwu | domestic hot water tank |          |
| Ts2 | additional sensor2 | Buffer         |

<br><br>
 ## Photos:<br>
 ![v1.3](./m_PCB_v1.3_noscreen.jpg)
 ![v1.3](./m_PCB_v1.3_screen.jpg)
 ![v1.3](./m_v1.3_PCBdemo.png) 

 
 ## Older revisions and prototypes:<br>
 PCB v1.1
 
 ![proto3](./m_proto3.jpg)
 ![proto3 without screen](./m_proto3_noscreen.jpg)
 
 This is prototype 2 (PCB v1.0).
 1602 is the best choice.
 
 ![proto2](./m_proto2.jpg)
 
 EEV development started here, PCB v1.0.
 
 ![proto2_EEVdev](./m_proto2_EEVdev.jpg)
 
 ![proto2 PCB](./m_proto2_PCB.jpg)
 
Prototype 1. Say v0.0.
History ) But worked well for a 2018-19 season.
 ![proto1](./m_proto1.jpg)

<br><br>

## Author:
gonzho АТ web.de (c) 2018-2019

If you have any comments or questions, please do not hesitate to contact me.
