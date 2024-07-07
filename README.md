# Robotic Printing
###### A controller box and mounts to get started with the Massive Dimension thermoplastic extruder MDPH2 on UR10e

1.
![RobotPrint_Ajustable_Speed](Pictures/RobotPrint_Ajustable_Speed.jpg)

2.
![RoboticPrinting_Basic_Script](Pictures/RoboticPrinting_Basic_Script.jpg)

3.
![RoboticPrinting_Jump_Between_Curves](Pictures/RoboticPrinting_Jump_Between_Curves.jpg)

4.
![RoboticPrinting_Non-Planar_Extrusion](Pictures/RoboticPrinting_Non-Planar_Extrusion.jpg)

5.
![RoboticPrinting_Open_Curve](Pictures/RoboticPrinting_Open_Curve.jpg)

6.
![RoboticPrinting_Seam_Ajustment](Pictures/RoboticPrinting_Seam_Ajustment.jpg)

## Overview
This repository contains everyting needed to get started with robotic thermoplastic printing with the [MDPH2 from Massive Dimension](https://massivedimension.com/products/mdphe-v1-pellet-head-extruder-system) and the [Universal Robot UR10e](https://www.universal-robots.com/products/ur10-robot/) (or any UR model capable of handeling the weight).

Included in the repository are:

- Controller Box CAD Files: For 3D printing the structure and laser cutting the acrylic walls of the controller box,
- Mounts CAD Files: For 3d printing mounts to secure the clay tank, extruder, and pressure gauge,
- Scripts: Arduino code for the controller and Grasshopper toolpath example.

## Note on the Signal Workflow

The workflow begins with the Grasshopper script embedding the extrusion commands (rate and direction) into the URP file. This file instructs the UR10e to output two analog voltage signals (0-5V) on AO0 (Pul) and AO1 (Dir). These signals are read by the Arduino on pins A0 (Pul) and A1 (Dir), which translates them into digital signals on pins D2 (Pul) and D3 (Dir) for the ClearPath servo motor of the MDPH2.

The analog signals can also be overwritten in real-time using the UR Teaching Pendant. 

**Grasshopper (Extrusion Command Embeded in URP file/G-Code ) ⟶ UR10e (Analog Output 0-5V) ⟶ Arduino (Analog Input) ⟶ MDPH2's Servo Motor (Digital Signal)**

**Note:** The temperature is not controlled by the Grasshopper script. The temperature must be set manually on the PID controller. Allow the extruder to warm up to the desired temperature before starting extrusion to prevent motor damage due to high torque.  
For PLA, we set the temperature to 190°C. It is recommended to stay on the lower end of the melting temperature spectrum to maintain high viscosity, reduce stringing, and minimize cooling time.

## Components

- Arduino Nano
- LM2596 Stepdown reducer 24v to 12v
- Rocker Switch
- Inkbird ITC-100 PID Temperature Controller (comes with the MDPH2)
- IPC-5 75v Power Supply (comes with the MDPH2)
- Solid State Relay (comes with the MDPH2)
- 24v Power Supply
- M3 and M4 Hex Socket Head Screws
- 3mm Acrylic Sheet
- Electrical Connectors (JST XH in our case)
- 18 and 24awg cables 

## Setup and Installation

### Prerequisites

- UR10e Robotic Arm (or any UR models),
- Massive Dimension MDPH2 Thermoplastic extruder,
- Rhino Grasshopper: For generating toolpath commands.

### Wiring Diagram

Refer to the following wiring diagram for the electrical connections.

![MDPH2_on_UR10e_Wire_Diagram](Controller_Box/Wiring_Diagram/MDPH2_on_UR10e_Wire_Diagram.svg)
Diagram made with the open-source tool [Fritzing](https://fritzing.org/).

### Controller Box and Mount CAD Files

- [MDPH2 Extruder Mount](Mount/Print_MD_Extruder_Mount.stl) 3D printing file,
- [Structure](Controller_Box/CAD/Print_MD_Skeleton.stl) 3D printing file. Skeleton inside de box holding all the components in place,
- [Corner brackets](Controller_Box/CAD/Print_MD_Corner_Bracket.stl) 3D printing file,
- [3MM acrylic wall](Controller_Box/CAD/Cut_MD_3MM_Walls.AI) Laser cutting file.

### Scripts

#### [Arduino Script](Arduino/Stepper_PulseDir_MD)

Upload the provided code to the Arduino Nano. Ensure all connections are made according to the wiring diagram.

#### [Grasshopper Definition](Grasshopper/Basic_Robotic_Extrusion_MDPH2.gh)

The [Robots](https://www.food4rhino.com/en/app/robots) plugin is necessary. The extrusion rate is set in RPM and is converted to steps per second with the following equation:
$$y = \left( \frac{x \times 800}{60} \right)$$
Where 800 represents the number of steps per revolution for the stepper motor, and 60 is used to convert minutes to seconds, resulting in the conversion of RPM (x) to steps per second (y).

The actual RPM can be checked through [ClearPath MSP](https://www.teknic.com/files/downloads/motor_setup.zip) via the micro USB cable on top of the servo motor.

The mass and center of gravity can be found in the Grasshopper script. These settings are very important for this extruder as it is reaching the load capacity of the UR10e. 

## Photos and Videos

https://github.com/cxlso/MDPH2_on_UR10e/assets/29285706/ac5a9cc8-03f6-4395-85b4-7b0d291706e9

https://github.com/cxlso/MDPH2_on_UR10e/assets/29285706/749127e9-4e9b-446a-b5af-80f2f0c1dffd

<img src="https://github.com/cxlso/MDPH2_on_UR10e/assets/29285706/128c2434-52cd-4ef8-8e8a-50a991110c6a" width="30%"></img> <img src="https://github.com/cxlso/MDPH2_on_UR10e/assets/29285706/effd8bc1-c4e3-410f-8999-9d7a34db1e4b" width="30%"></img> <img src="https://github.com/cxlso/MDPH2_on_UR10e/assets/29285706/a2818543-1c64-4062-9cc6-14fa658f7270" width="30%"></img> <img src="https://github.com/cxlso/MDPH2_on_UR10e/assets/29285706/daca66d9-8102-4669-9728-286e8a8b9e8d" width="30%"></img> <img src="https://github.com/cxlso/MDPH2_on_UR10e/assets/29285706/bd144e61-af86-4097-93f5-d0e02794067f" width="30%"></img> 

## Contributing

Contributions are welcome! Please submit a pull request or open an issue to discuss any changes or improvements.

## License

[![CC BY-NC-SA 4.0][cc-by-nc-sa-shield]][cc-by-nc-sa]

This work is licensed under a
[Creative Commons Attribution-NonCommercial-ShareAlike 4.0 International License][cc-by-nc-sa].

[![CC BY-NC-SA 4.0][cc-by-nc-sa-image]][cc-by-nc-sa]

[cc-by-nc-sa]: http://creativecommons.org/licenses/by-nc-sa/4.0/
[cc-by-nc-sa-image]: https://licensebuttons.net/l/by-nc-sa/4.0/88x31.png
[cc-by-nc-sa-shield]: https://img.shields.io/badge/License-CC%20BY--NC--SA%204.0-lightgrey.svg

## Acknowledgements

Special thanks to [Luigi Pacheco](https://luigipacheco.com/) and [Bhavleen Kaur](https://bhavleenkaurnarula.wordpress.com/) from the [FIU RDFlab](https://rdflabfiu.github.io/labwiki/) for the tips.
