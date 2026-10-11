# FMX6 — CAN adapter (LV-CAN200 / ALL-CAN300) in Teltonika Configurator 1.8.26

> **Verified against an FMC650 (firmware 03.01.01.Rev.15, Configurator 1.8.26, 2026-10-11).** The list
> below is the model embedded in the Configurator executable; `io_elements_all.tsv` keeps one name per
> IoId, and 1,052 variants were dropped in the de-duplication, so a name can belong to another firmware
> family (FMB). The device is the authority, and it says:
>
> | id (priority / operand / … / avg = id+5) | on the FMC650, in the Configurator UI and read back from the device | the model says |
> |---|---|---|
> | 45440 / 45450 / 45460 | **Security / Control / Indicator State Flags P4** (AVL 12710/12711/12712) - set High + On Change on 45440, the flags arrive on every change | tachograph distance / trip distance / tachograph speed (the FMB variant) |
> | 46030 / 46040 / 46050 | **Clutch switch / PTO state / Accelerator pedal (FMS IO)** - a setparam here changes nothing on a passenger car | Security / Control / Indicator P4 (list below) - **wrong for the FMC650** |
> | 45970 | **Module ID 17B** (AVL 529; not shown in the UI, switched off with setparam 45970:0) | 45970 LVC CNG Level, 45980 Module ID 17B in the list below; `io_elements_all.tsv` says 45970 = Module ID 17B |
> | 45100, 45110, 45150, 45170, 45220, 45620, 45630, 45635, 45640, 45930, 45940, 45960 | speed, pedal, total mileage, doors, mileage counted, battery temperature, battery level, its averaging constant, DTC faults, fault codes, range on battery, VIN | agrees |
>
> So: use this table for 454xx / 460xx on the FMx6 family and the model for the rest. A parameter id is
> proven by reading it off the Configurator's row and with `getparam` on the device - not by a `setparam`
> being accepted.

Extracted from the embedded parameter model in Teltonika.Configurator_1.8.26_B.FMX6_R.194.exe
Wiki: https://wiki.teltonika-gps.com/view/FMB110_CAN_adapters

## Configuration parameters on the 'CAN Adapter' tab
- 45002  Program number  (Int32, 0..99999, default 11120)  -> selects the vehicle profile in the adapter
- 45003  DTC request period  (0..65535 min, default 0)
- 45006  IO sending options  (collection LVCAN_IO_Sending_Opt, default 2)
- Source selector: None / LV-CAN200 (=1) / ALL-CAN300 (=2)

Flag tabs (ALL-CAN300 bit fields): Security (SSF), Control (CSF), Indicator (ISF), Agricultural (ASF), Utility (USF), Cistern (CiSF)

## Complete list of CAN adapter I/O elements (AVL ID / name / unit / SMS event ID)

45100	Vehicle speed [km/h]	(SMS 8141)
45110	Acceleration pedal position [%]	(SMS 8142)
45120	Fuel Consumed [ltr×10]	(SMS 8143)
45130	Fuel level [ltr]	(SMS 8144)
45140	Engine RPM [rpm]	(SMS 8145)
45150	Total mileage [m]	(SMS 8146)
45160	Fuel level [%]	(SMS 8147)
45170	Door status	(SMS 8148)
45180	Program number	(SMS 8149)
45190	Module ID 8B	(SMS 8150)
45200	Engine worktime [min]	(SMS 8151)
45210	Engine worktime (counted) [min]	(SMS 8152)
45220	Total mileage (counted) [m]	(SMS 8153)
45230	Fuel consumed (counted) [ltr×10]	(SMS 8154)
45240	Fuel rate [ltr/h]	(SMS 8155)
45250	AdBlue level [%]	(SMS 8156)
45260	AdBlue level [ltr×10]	(SMS 8157)
45270	Engine load [%]	(SMS 8158)
45280	Engine temperature [℃×10]	(SMS 8159)
45290	Axle 1 load [kg]	(SMS 8160)
45300	Axle 2 load [kg]	(SMS 8161)
45310	Axle 3 load [kg]	(SMS 8162)
45320	Axle 4 load [kg]	(SMS 8163)
45330	Axle 5 load [kg]	(SMS 8164)
45340	Control state flags	(SMS 8165)
45350	Agricultural machinery flags	(SMS 8166)
45360	Harvesting time [min]	(SMS 8167)
45370	Area of harvest [m²]	(SMS 8168)
45380	Mowing efficiency [m²/h]	(SMS 8169)
45390	Grain mown volume [kg]	(SMS 8170)
45400	Grain moisture [%]	(SMS 8171)
45410	Harvesting drum RPM [rpm]	(SMS 8172)
45420	Gap under harvesting drum [mm]	(SMS 8173)
45430	Security state flags	(SMS 8174)
45440	Tachograph total vehicle distance [m]	(SMS 8175)
45450	Trip distance [m]	(SMS 8176)
45460	Tachograph vehicle speed [km/h]	(SMS 8177)
45470	Tachograph driver card presence	(SMS 8178)
45480	Driver 1 states	(SMS 8179)
45490	Driver 2 states	(SMS 8180)
45500	Driver 1 continuous driving time [min]	(SMS 8181)
45510	Driver 2 continuous driving time [min]	(SMS 8182)
45520	Driver 1 cumulative break time [min]	(SMS 8183)
45530	Driver 2 cumulative break time [min]	(SMS 8184)
45540	Driver 1 selected activity duration [min]	(SMS 8185)
45550	Driver 2 selected activity duration [min]	(SMS 8186)
45560	Driver 1 cumulative driving time [min]	(SMS 8187)
45570	Driver 2 cumulative driving time [min]	(SMS 8188)
45580	Driver 1 ID High	(SMS 8189)
45590	Driver 1 ID Low	(SMS 8190)
45600	Driver 2 ID High	(SMS 8191)
45610	Driver 2 ID Low	(SMS 8192)
45620	Battery temperature [℃]	(SMS 8193)
45630	Battery level [%]	(SMS 8194)
45640	DTC Faults	(SMS 8195)
45650	Slope Of Arm [°]	(SMS 8196)
45660	Rotation Of Arm [°]	(SMS 8197)
45670	Eject Of Arm [m]	(SMS 8198)
45680	Horizontal Distance Arm Vehicle [m]	(SMS 8199)
45690	Height Arm Above Ground [m]	(SMS 8200)
45700	Drill RPM [rpm]	(SMS 8201)
45710	Amount Of spread Salt Square Meter [g/m²]	(SMS 8202)
45720	Battery Voltage [V]	(SMS 8203)
45730	Amount Of Spread Fine Grained Salt [T]	(SMS 8204)
45740	Amount Of Spread Coarse Grained Salt [T]	(SMS 8205)
45750	Amount Of Spread DiMix [T]	(SMS 8206)
45760	Amount Of Spread Coarse Grained Calcium [m³]	(SMS 8207)
45770	Amount Of Spread Calcium Chloride [m³]	(SMS 8208)
45780	Amount Of Spread Sodium Chloride [m³]	(SMS 8209)
45790	Amount Of Spread Magnesium Chloride [m³]	(SMS 8210)
45800	Amount Of Spread Gravel [T]	(SMS 8211)
45810	Amount Of Spread Sand [T]	(SMS 8212)
45820	Width Pouring Left [m]	(SMS 8213)
45830	Width Pouring Right [m]	(SMS 8214)
45840	Salt Spreader Working Hours [h]	(SMS 8215)
45850	Distance During Salting [km]	(SMS 8216)
45860	Load Weight [kg]	(SMS 8217)
45870	Retarder Load [%]	(SMS 8218)
45880	Cruise Time [min]	(SMS 8219)
45890	CNG Status	(SMS 8236)
45900	CNG Used [kg]	(SMS 8237)
45910	CNG Level [%]	(SMS 8238)
45920	Oil Level	(SMS 8239)
45930	Fault Codes	(SMS 8265)
45940	Vehicles Range On Battery [m]	(SMS 8288)
45950	Vehicles Range On Additional Fuel [m]	(SMS 8289)
45960	VIN
45970	LVC CNG Level Kg [kg×10]	(SMS 8380)
45980	Module ID 17B	(SMS 8409)
46020	Distance to next service [km]	(SMS 8425)
46030	Security state flags P4	(SMS 8443)
46040	Control state flags P4	(SMS 8444)
46050	Indicators state flags P4	(SMS 8445)
46060	Agricultural state flags P4	(SMS 8446)
46070	Utility state flags P4	(SMS 8447)
46080	Cistern state flags P4	(SMS 8448)
46090	Total LNG Used [kg]	(SMS 8574)
46100	Total LNG Used (counted) [kg]	(SMS 8575)
46110	Total LNG Level [%]	(SMS 8576)
46120	Total LNG Level [kg]	(SMS 8577)
46130	Distance from need of service [km]	(SMS 8585)
46140	Distance from last service [km]	(SMS 8586)
46150	Time to next service [days]	(SMS 8587)
46160	Time from need of service [days]	(SMS 8588)
46170	Time from last serivce [days]	(SMS 8589)
46180	Distance to next oil service [km]	(SMS 8590)
46190	Time to next oil service [days]	(SMS 8591)
46200	Vehicles Range [km]	(SMS 8632)
46320	Total CNG Used (counted) [kg]	(SMS 8681)
48410	Total Bale Count	(SMS 8891)
48420	Bale Count	(SMS 8892)
48430	Cut Bale Count	(SMS 8893)
48440	Bale Slices	(SMS 8894)
48620	Total LPG Used [ltr]	(SMS 8912)
48630	Total LPG Used (counted) [ltr]	(SMS 8913)
48640	Total LPG Level [%]	(SMS 8914)
48650	Total LPG Level [ltr]	(SMS 8915)
48680	Remaining work time on HV battery [min]
48730	Max road speed [km/h]	(SMS 8934)
48740	Exceeded road speed [km/h]	(SMS 8935)
48780	Engine oil pressure [bar×10]
48790	Engine oil level [%]
48890	Water level for fire fighting [%×10]
48900	Foam level for fire fighting [%×10]
48910	Pump pressure for fire fighting [bar×10]
48920	Wind speed [m/s]
48930	Temperature outside [°C]
48940	Gross vehicle combination weight [kg]
48950	Drive system temperature [°C]
48960	Rotation speed of auger conveyor [rpm]
48970	Rotation speed grain auger [rpm]
48980	Rotation speed of chopper [rpm]
48990	Rotation speed of straw shaker [rpm]
605000	Rotation speed of fan [rpm]
605010	Rotation speed of feeder [rpm]
605020	Speed of Reel [km/h×10]
605030	Amount of cargo in the grain elevator [%×10]
605040	Time of work with threshing drum on [s]
605050	Overall time of combine work with threshing [s]
605060	Current trip distance [m]
605070	Overall distance covered during work [m]
605080	Current distance covered during work [m]
605090	Current harvest area [m²]
605100	Total harvest area [m²]
605110	Hydraulic oil pressure [bar×10]
605120	HV battery average power consumption [kWh/1000km]
605130	HV battery pack health [%×10]
605140	Electric engine work time(counted) [s]
605150	Wheel speed sensor impulses(counted) [impulses]
605160	Electric engine consumed kWh [kWh×100]
605170	Axle 1 Load [kg]
605180	Axle 2 Load [kg]
605190	Axle 3 Load [kg]
605200	Axle 4 Load [kg]
605210	Left tire pressure trailer axle 1 [bar×10]
605220	Right tire pressure trailer axle 1 [bar×10]
605230	Left tire pressure trailer axle 2 [bar×10]
605240	Right tire pressure trailer axle 2 [bar×10]
605250	Left tire pressure trailer axle 3 [bar×10]
605260	Right tire pressure trailer axle 3 [bar×10]
605270	Left tire pressure trailer axle 4 [bar×10]
605280	Right tire pressure trailer axle 4 [bar×10]
605290	CNG level [bar×100]
605300	Engine work time on CNG [s]
605310	Trailer VIN
605320	Trailer axle load sum [kg]
605330	Total treated area [ha×10000]
605340	Level of liquid in tank [l×10]
605350	Spray pressure [bar×100]
605360	Actual dose of liquid [l/ha×10]
605370	Amount of liquid used [l×10]
605380	Area treated until tank is empty [ha×10000]
605390	Distance remaining until tank empty [m×10]
605400	Engine work time on primary fuel [s]
605410	Engine work time on duel fuel [s]
605420	Total weight of spread material [kg]
605430	Weight of material for spreading [kg]
605440	Engine oil temperature [℃]
605450	Rear power take-off rotation speed [rpm]
605460	Front power take-off rotation speed [rpm]
605470	Amount of chemicals per sqr meter [g/m²×100]
605480	Spread width [m×100]
605490	Active gear number
605500	Front left tire pressure [bar×100]
605510	Front right tire pressure [bar×100]
605520	Rear left tire pressure [bar×100]
605530	Rear right tire pressure [bar×100]

Total: 187 I/O elements
