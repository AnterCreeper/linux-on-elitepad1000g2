# linux-on-elitepad1000g2
## try to install linux on hp elitepad 1000 g2

#### The problem it has:
1. Display on 1920x1200 10 inch screen is not very good. You may need to enable HiDPI(fractional scale) and customize the user interface by modifing the theme(GNOME etc).

2. The IRQ for the sensor is high so that power consumption is a bit high. You can disable it by adding blacklist for hid_sensor_hub

3. The two cameras cannot work. The atomisp is still under development and unusable.(Today is 2021 but the baytrail platform is released in 2014-2015? It has been years since that and I don't know whether it will be supported anymore.) I don't see any work for imx175 on the TODO list of atomisp.
What you need do is to modify the BIOS value by setup_var of GRUB. You need to change the atomisp from subdevice of IGD to PCI. This setting is not exposed in the BIOS setup menu.(UEFITool, IFR Extractor)

4. The sound card works partly. The speaker is ok but the internal MEMS digital mic and the jack don't work. You can try to use a usb sound card now. But since the usb hub uses a module to turn from 3.3v to 5.1v, using too much usb device will cause battery problem.

5. The wwan is not work out-of-box. You can google to find the solution.

6. The rotate lock key cannot work because it isn't registered. No program know what to do when receiving the event.
