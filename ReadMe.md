# ESP32 3D Printer Controller

This is based on https://github.com/felixstorm/Creality_Ender_3_ESP32_Board.git with the following changes

- Laid out for 2-sided PCB fabrication instead of PCB routing.  2-sided PCBs are available at very low cost from various prototyping shops, and provide much better grounding and power distribution.
- Can use either Pololu stepper driver modules or external drivers, selected by solder jumpers.
- The microstepping ratio can be selected by solder jumpers, or those pins can be connected to SPI for fine-grained configuration of TMC modules.  The X Y and Z axes all use the same microstepping/SPI settings, whereas the extruder can be configured independently of X, Y, Z.
- The I/O voltage to the shift registers and stepper modules can be either 3V3 or 5V, selected by solder jumper.  3V3 is appropriate for TMC modules where the SPI output goes back to the ESP32.  5V is best for external drivers whose optocoupler inputs work best at 5V.  Non-TMC stepper modules can use either.
- The micro SD card socket uses a Wemos D1 Mini shield which has a smaller footprint than the one on the original design, permitting all the drivers to be placed along one edge of the board.
- The connectors along the bottom edge of the board, and the power input connector, are all pluggable terminal blocks, thus permitting easy disconnection of the signals for assembly and maintenance.  The layout permits each signal pair to be on its own plug, or everything on one longer plug, or any combination thereof, at the users discretion.
- For the steppers, you can choose either 0.1" pitch connectors - e.g. pin headers with Dupont connectors - or 0.15" pitch pluggable terminal blocks (or 0.15" screw terminals, but pluggable is easier to wire and maintain).
- The fuse holders were spread out to make it easier to get your fingers in to grab the fuses.

The design passes acceptance checks for the aisler.net entry level prototyping service.

Update: The board is fully functional.  If you would like to build it, contact me and I will upload the revised version that fixes a few silkscreen errors and provides better clearance between some components that were a little crowded.

*Yet another Update:  Do not use the fan outputs; they are broken.  If you connect a fan it is likely to blow up the ULN2803A chip.  The problem - which I inherited from the base design and neglected to check - is that you cannot put ULN2803A channels in parallel and get them to share current.  ULN2803A use bipolar junction transistors - BJTs.  BJTs have a positive temperature coefficient, which means that, as the temperature increases, the collector current also increases.  When you put them in parallel, if one of them starts drawing a little more current than its neighbor, it gets hotter, and starts drawing even more current.  This is called "thermal runaway". Instead of sharing the load, one will take it all, then overheat and blow out, then the neighbor will take all the current and blow out, etc.  It is possible to make current-sharing circuits with BJTs, but ULN2803A does not have that kind of circuit.  The solution to the problem is to replace the ULN2803A with MOSFETs like that one that controls the extruder temperature.*

The board works with Marlin 2.0 for printer applications and with [Grbl_ESP32](https://github.com/bdring/Grbl_Esp32) for general CNC.

![ESP32PrinterBoard](https://user-images.githubusercontent.com/4861133/81722069-964f5c80-941c-11ea-9717-9a32ae165790.jpg)
