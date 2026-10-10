# FMC650 configuration for the bZ4X

Two forms of the same settings:

- **This page** — the parameters that matter for the car, with their ids, so they
  can be set by SMS or by a server command (`setparam`) without the Configurator.
- **[fmc650-bz4x.cfg](fmc650-bz4x.cfg)** — a Configurator file read from the
  tracker in the car, with everything personal removed (see *What was removed*).
  Load it in the Teltonika Configurator, **fill in your own server and APN**, and
  check every page before *Save to device*.

Firmware on the tracker: **03.01.01.Rev.15**. Parameter ids follow Teltonika's
FMC650 parameter list; the LVCAN element ids follow the FMB LVCAN list (xx0 =
priority, xx1 = operand), which the FMC650 shares — proven by reading them back.

## Set in the Configurator (no `setparam` for these)

| setting | value |
|---|---|
| Data protocol | **Codec 8 Extended** |
| RS232/RS485 → COM1 mode | **LVCAN** |
| COM1 baud rate | **Default** (the top entry — *not* 115200) |
| LVCAN → Program number | **16145** (or SMS/GPRS `lvcansetprog 16145`) |
| I/O → LVCAN elements | all wanted ones enabled (out of the box most are *None*) |
| I/O → Security / Control / Indicator State Flags P4 | enabled |
| Server, port, protocol TCP, APN | **your own** — type the APN in, do not leave AutoAPN on |

## Parameters

| id | parameter | value | note |
|---|---|---|---|
| 101 | Ignition source | **48** | Power Voltage (32) + Movement (16); 33 = DIN1 + Power Voltage if you wire DIN1 to a switched fuse |
| 104 | High voltage level | 30000 | mV |
| 105 | Low voltage level | **13700** | mV; 12600 while charging, set by the server (README, *Keeping the tracker awake*) |
| 100 | Movement source | **0** | Ignition |
| 19001 / 19002 | Movement start / stop delay | 1 / 60 | s |
| 1000 | Open Link Timeout | **30** | s |
| 10000 / 10004 / 10005 | Home, On Stop: Min Period / Saved Records / Send Period | **60 / 1 / 125** | Roaming = +100, Unknown = +200, same values |
| 10050 / 10055 | Home, Moving: Min Period / Send Period | **5 / 10** | Roaming = +100, Unknown = +200; min distance 100 m, angle 10°, speed delta 10, 1 record |
| 45630 / 45631 | HV battery level (AVL 142): priority / operand | **2 / 5** | High + On Change |
| 46030 / 46031 | Security State Flags P4 (AVL 12710): priority / operand | **2 / 5** | High + On Change |
| 46040 / 46050 | Control / Indicator State Flags P4 (12711 / 12712): priority | 1 | Low, operand Monitoring (3) |
| 45170 / 45171 | Door status (AVL 143): priority / operand | **1 / 3** | Low + Monitoring — High + On Change sends every contact bounce |
| 50220 / 50230 / 50240 | Accelerometer axes X / Y / Z | 0 | Traccar reads AVL 236 as a false alarm |
| 50820, 50250 | IMSI, ICCID | 0 | |
| 50690 / 50700 | DO3 / DO4 | 0 | |
| 50650 | Network type | 0 | |
| 45180, 45190 | LVCAN program number, module id (in every record) | 0 | |
| 45340 / 45430 | legacy Control / Security flags (38 / 47) | 0 | the P4 elements carry the same |
| 45350, 45890, 45920 | agricultural flags, CNG, engine oil | 0 | not on this car |

Priority: 0 off, 1 Low, 2 High, 3 Panic (High + SMS when GPRS fails).
Operand: 3 Monitoring, 5 On Change.

## As commands

Send by SMS (`<login> <password> setparam …`, or two leading spaces when login
and password are empty) or as a server command. The tracker answers
`New value id:value;…` for what it took. Send one group at a time and wait for
its reply — the tracker handles one command at a time.

```
setparam 101:48;104:30000;105:13700
setparam 100:0;19001:1;19002:60;1000:30
setparam 10000:60;10004:1;10005:125
setparam 10100:60;10104:1;10105:125
setparam 10200:60;10204:1;10205:125
setparam 10050:5;10055:10
setparam 10150:5;10155:10
setparam 10250:5;10255:10
setparam 45630:2;46030:2
setparam 45631:5;46031:5
setparam 45170:1;45171:3
setparam 50220:0;50230:0;50240:0
setparam 50820:0;50250:0;50690:0;50700:0;50650:0
setparam 45180:0;45190:0;45340:0;45430:0;45350:0;45890:0;45920:0
lvcansetprog 16145
```

Read back with `getparam id;id;…` (a few at a time — a long list comes back cut
off), and afterwards read the whole configuration from the device and save a
fresh file. A *Save to device* from an older file puts the old values back.

## What was removed from the .cfg

Every free-text field that was filled in (server domain, APN and its login,
phone numbers, names and identifiers) is empty, and the one geofence that was
set up has been reset to the values of an unused, disabled zone. The server
port is left at 5027, Traccar's Teltonika port. Fields left as they are hold
Teltonika's defaults. The file name no longer carries the IMEI. Fill in your
own server and APN before *Save to device*.

There is no "LVCAN mode" parameter (45000 on the FMB family) in this FMC650
configuration: the adapter is chosen by COM1 = LVCAN alone.
