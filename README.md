# Klipper Config

Repo for the Klipper firmware config files for my Prusa MK3S+ based printer with Einsy Rambo MCU board.

The current relevant differences between my printer and a stock MK3S+:

  * Frame: Bear Upgrade 2.1
  * X Carriage: BearExxa
  * Extruder: BearExxa with "Bowtie" 38:12 reduction pulleybox and Lin Engineering 417-09-18 stepper



## Adapting this config to your printer

**NOTE:** I do not guarantee this is a complete list, nor would I call this a reliable set of "instructions". I suggest reading through all .cfg files to understand what they do before using them.

My advice for using this config on your own printer would be as follows:


### Prerequisites

  * A roughly Prusa-MK3(S)(+)-shaped printer with an Einsy Rambo MCU board (the stock board included with Prusa MK3 printers).
  * Your Einsy's 32u2 usb-to-serial chip flashed with [faster firmware](https://github.com/PrusaOwners/prusaowners/wiki/hoodloader2).
  * Klipper already up and running on your choice of host. I use [FluiddPi](https://github.com/cadriel/FluiddPI) running on a Raspberry Pi 4.


### Config Changes

  * In [`printer.cfg`](/printer.cfg), make the following changes:
    * Under `[mcu]`, verify `serial:` matches your Einsy's serial id so the Klipper host can talk to it.
    * Comment out `[include cfg/resonance_comp.cfg]` and `[include cfg/pi_mcu.cfg]`. These are related to [Klipper's resonance compensation features](https://www.klipper3d.org/Resonance_Compensation.html). You can add them back in later, but you should set up and calibrate these yourself. The input shaper settings that are tuned for my hardware are likely to make prints worse on your hardware.
    * Change which extruder config file is included to match your hardware, or make your own. Stock Prusa or BearExxa extruder should start with [`stock_extruder_0.4.cfg`](cfg/stock_extruder_0.4.cfg).
    * In my active extruder config, [`pulleybox_extruder_0.4_lin_pt100.cfg`](cfg/pulleybox_extruder_0.4_lin_pt100.cfg), the following has been adjusted from [`stock_extruder_0.4.cfg`](cfg/stock_extruder_0.4.cfg) settings. The klipper extruder config reference can be found [here](https://www.klipper3d.org/Config_Reference.html#extruder).
      * `gear_ratio`
      * `microsteps`
      * `full_steps_per_rotation`
      * `run_current`
      * `sensor_type`


### Printer Setup and Initialization

  * **IMPORTANT:** Use [`PROBE_CALIBRATE`](https://www.klipper3d.org/Probe_Calibrate.html#calibrating-probe-z-offset) to calibrate your base probe offset (the z-offset from the probe trigger point to the actual tip of the nozzle) so you don't crash your nozzle/damage the bed. Do not assume the current value in this config matches your printer.
  * Check and calibrate [`rotation_distance`](https://www.klipper3d.org/Rotation_Distance.html) if you want, but I found it to be close enough for my stock MK3S+ Bondtech drive gears that I didn't bother.
  * Initialize your flexplate array using `INIT_PLATE_ARRAY`, and add new plates if needed. See [`flexplate.cfg`](cfg/flexplate.cfg) for instructions and relevant macros.


### Klipper Config Reference

[Klipper Configuration Reference Docs](https://www.klipper3d.org/Config_Reference.html)
