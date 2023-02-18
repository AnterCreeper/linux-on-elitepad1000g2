# linux-on-elitepad1000g2
## try to install linux on hp elitepad 1000 g2

#### The problem it has:
1. Display on 1920x1200 10 inch screen is not very good. You may need to enable HiDPI(fractional scale) and customize the user interface by modifing the theme(GNOME etc).

2. The SD Card is Read-only(you can look at the schematic and find out that SD Write protect is connnected to the lid detect.). I have contacted the maintainer of the kernel, but i don't have enough time to fix it.
You can have a try, modify the sdhci.c(something like sdhci, hci, etc.) and add a dmi entry to avoid it.

3. The two cameras cannot work. The atomisp is still under development and unusable.(Today is 2021 but the baytrail platform is released in 2014-2015? It has been years since that and I don't know whether it will be supported anymore.) I don't see any work for imx175 on the TODO list of atomisp, only the ov2722 and lm3455.
What you first need to do is modifying the BIOS value by setup_var of GRUB. You need to change the atomisp from subdevice of IGD to PCI. This setting is not exposed in the BIOS setup menu.(UEFITool, IFR Extractor)

4. (obsoleted, patched in kernel upstream) Sound Card Fixing: https://bugzilla.kernel.org/show_bug.cgi?id=213415
   The Headphone is just the lineout, with a extra headphone amplifier. It really improve the quality of the audio.
   Some packs have been uploaded to this repo.
   a. A driver in dkms type. The official driver will be merged to the mainline soon.
   b. alsa-ucm-conf debian package modified from the ubuntu source.
   
   Install the things above and put "snd_soc_sst_bytcr_rt5640.quirk=0x6400002" to the /etc/default/grub, then the jack audio is ok.

5. The wwan is not work out-of-box and need to be set manually . You can google to find the solution.

6. Pay attention that 5G wifi must be enable by the US(United States) firmware. You can get nvram from hp.

6. The rotate lock key cannot work because it isn't registered. No program know what to do when receiving the event.

7. The power comsumption of usb device is very high compared to linux and the usb hub chip on the keyboard (productive kit) is very hot. Also the keyboard always wake it by mistake.
Fix: make a script: a is to fix the power comsuption problem and b is to disable wake up on keyboard.
  a. echo 0 > /sys/bus/usb/devices/*the keyboard or the usb hub*/port/power/pm_qos_no_power_off
  b. echo disabled > /sys/bus/usb/devices/*the keyboard*/power/wakeup

8. The support for psr on VLW Platform(search PSR VLV Nuke) is abandoned by i915. which is required for Opportunistic S0ix which can save a lot of energy.

9. The realtime performance is much worse than windows so the programs behave laggy. The HPET is disable due to the acurracy reason. 

some information here: https://blog.mynoee.com/archives/180/
