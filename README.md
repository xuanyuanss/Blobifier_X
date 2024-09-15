# Blobifier_X
## A variant of a Blobifier--No need for a lifting gantry

Although its physical structure is completely different from Blobifier, it only refers to its nozzle flushing idea, while making targeted modifications in the software part. For the original Blobifier, please go here.

The original Blobifier is not friendly to Voron2.4, requiring frequent lifting of the gantry, sacrificing the effective printing area of the heated bed, and there may be a risk of the print head hitting the model. Therefore, I designed Blobifier_X.

Blobifier_X is installed on the gantry, and it does not need to lower the gantry during flushing. Instead, a small stepper motor is used to achieve the lifting of the tray to facilitate blob formation.

![实物图1](https://github.com/user-attachments/assets/d1262b67-59d3-4b80-9c97-b1e5e798edf8)
![实物图2](https://github.com/user-attachments/assets/4383e479-40f2-46ec-8ea0-1e32e947f4bb)
![示意图 (2)](https://github.com/user-attachments/assets/9eef6366-32ae-49b3-8fce-8e302e3ef834)
![示意图 (5)](https://github.com/user-attachments/assets/869ce4ad-7e9c-406a-a5ad-c970504d72c1)
![示意图 (8)](https://github.com/user-attachments/assets/d8d18a2d-6b3f-4eb7-9d41-6e1db2315fcd)
![示意图 (1)](https://github.com/user-attachments/assets/b998a76f-3b49-49fa-954d-2f793d8f75f5)


# Update Description:
## 2024年9月1日
Adjust the blob ejection logic: The last blob of the current flush is ejected when the next flush stops, that is, the printing is restored immediately after the flush ends, without waiting for the Blobifier_X to return to its position, which effectively reduces the overflow of nozzle consumables. Even without a wiping mouth, the wiping tower/heated bed is also very clean.

# Bill of Materials:
* 28BYJ-48 stepper motor x1,
* TMC2209 or other drivers x1,
* ∅5x50 cylindrical pin x2,
* T5 lead screw with nut, length 40 x1,
* D2F-01L micro switch x1,
* M3x4x5 heat melt nut x8,
* M3x3 set screw x4, can also be used on BMG,
* M3x10 round head hex nut x4,
* M3x12 round head hex nut x2,
* M3x16 round head hex nut x1,15x2x75 aluminum bar x1, I used CNC TAP, the length of the aluminum bar may not be suitable for you, please adjust it yourself to extend 4-5mm in front of the nozzle, punch holes according to the figure below.
* Heated bed film 15x15mm x1, textured film, adhesive on the back, about 0.4 thickness, there are many on TB, generally black, buy the smallest specification and cut it yourself.

![托盘](https://github.com/user-attachments/assets/e8d3d922-50fd-4c07-bbfb-958ced916aef)

# Key Assembly Steps Description:
When pressing the coupler into the heat melt nut, it is recommended to assemble it with the motor and lead screw first, then press the nut in, 和 press the nut to the bottom.
Cut the motor shaft perpendicular to its wiring direction, and pre-assemble it with the main body and cylindrical pin in advance;
Assemble the lead screw, nut, coupler, and slider into a whole;
Then assemble the above two parts together, tighten the set screw at the end of the motor shaft, at this time, you may need to use pliers to rotate the lead screw so that the two corresponding set screws can be tightened.
Finally, assemble the remaining parts, and don't forget to install the bump block on the print head.

# Regarding motor wiring and driver instructions:
28BYJ-48 is a 5-line 4-phase motor, do not connect the red wire when wiring, and connect the other 4 wires according to the two-phase motor wiring.
The voltage used by 28BYJ-48 is 5V, you need to cut the Vin (VM) pin of the driver, and lead out a line to connect to 5V.

![2209](https://github.com/user-attachments/assets/0d0a6e64-b28d-4eae-8c2d-3a136b0d380e)
![28BYJ-48](https://github.com/user-attachments/assets/21adb548-b895-442d-a475-429251b0d607)

# Tuning Instructions:
Upload Blobifier_X to the printer, and include the blobifier_x.cfg file in printer.cfg, modify the motor and limit pin feet in blobifier_x_hw.cfg.
Important: Motor direction debugging, manually place the slider in the middle of its travel, run MANUAL_STEPPER STEPPER=blobifier_x MOVE=5 (the motor moves a short distance, such as 5), at this time, the slider should move down (that is, away from the motor direction), if the movement is opposite, please adjust the motor running direction.
Slider zero position adjustment: Adjust the micro switch impact screw on the slider (M3x16), so that when the micro switch is triggered, there is some gap between the slider and the main body (for example, 1mm, the gap size is not strictly required, to avoid the slider hitting the Blobifier_X main body when returning to zero). Install Blobifier_X on the printer, run MANUAL_STEPPER STEPPER=blobifier_x MOVE=5, so that the slider moves away from the micro switch, and then run BLOBIFIER_HOME, at this time, the slider should move up and stop when the micro switch is triggered, at this time, move the print head close to the tray (be careful of collision, you can move the print head manually), and observe its distance from the tray. This is a repetitive process, use MANUAL_STEPPER STEPPER=blobifier_x MOVE=5 and BLOBIFIER_X_HOME commands flexibly and adjust M3x16, so that the tray is about 0.2-0.6mm below the print head. Note: The BLOBIFIER_HOME_X command (accurately said, when the parameter STOP_ON_ENDSTOP=1 in the MANUAL_STEPPER command) needs some time (about a few seconds), at this time, the printer will not execute other commands, please wait patiently.
Finally, move the print head to the middle of the tray, and record the X, Y coordinates (the Y coordinate is generally the maximum travel of the printer Y), fill in the variable_purge_x, variable_purge_y in blobifier_x.cfg; the parameters variable_kick_ready_x, variable_kick_x are the X coordinates where the print head is ready to kick off the blob and after the blob is kicked off, which can be set according to the actual situation, generally respectively Purge_x+20, Purge_x+50.

The provided configuration files are mainly used in conjunction with ERCF and HAPPY HARE, and the specific parameter settings in HAPPY HARE should refer to the HAPPY HARE and BLOBIFIER documents.

# Printing Suggestions:
Appropriately increase the number of outer walls and top and bottom surfaces to give Blobifier_X enough strength.
Keep the cylindrical pin installation holes of the main body and slider in the same direction to ensure that they have the same size accuracy as much as possible.
If the slider is too tight, you can appropriately enlarge the hole with a drill, but don't be too loose, so that Blobifier_X has good rigidity and can slide smoothly, it is recommended to apply some low-viscosity grease.
The slider CAD file is provided, which can be modified according to your own situation.
Two types of bump blocks are provided, and the V2 version is closer to the heated bed, avoiding the problem of not being able to kick off when the blob is small.
The provided material bucket CAD file is only suitable for my own printer, mainly for your reference, and can be modified or designed by yourself.

# Finally:
At present, there is no perfect solution for waste material processing of VORON2 structure printers, and the current plan basically meets the usage. As the printing progresses, the gantry gradually rises, and there is basically no risk of Blobifier_X hitting the waste material.
Thank the Blobifier open source project, the author's creativity is really great.
