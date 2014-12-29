---
layout: post
title: "Install CyanogenMod to Xperia"
date: 2014-06-27 13:35:50 +0600
comments: true
categories: [ Android, Xperia, CyanogenMod ]
---

Today I am going to share how to flash a Sony Xperia device with CyanogenMod. I have tried on my Xperia SP, and its fairly easy I assure you!! ;) <!--more-->

<b>Step 1:</b> You need to get your bootload unlocked before flashing ROM. Get your key from <a href="http://unlockbootloader.sonymobile.com/instructions">sonymobile</a>. Follow the instructions to get your keys and check wheather your bootloader can be unlocked.

<b>Step 2:</b> Use windows 7 or lower. Windows 8 or higher editions have driver signature problem. I don't like hassle so I went for windows 7, but if you insist on using windows 8/8.1 to unlock bootloader, follow <a href="http://www.fotoclubinc.com/blog/how-to-disable-driver-signature-enforcement-to-allow-installation-of-windows-7-printer-drivers-on-windows-8/">this instructions</a>.

<b>Step 3:</b> Dowload <a href="http://forum.xda-developers.com/showthread.php?t=2335555">Fasttool</a>. Download fasttool drivers from  <a href="http://forum.xda-developers.com/showthread.php?t=2635830">here</a>. Install them. Now download adb package from <a href="http://developer.android.com/tools/help/adb.html">here</a> (download, unzip, update Google drivers) or <a href="http://forum.xda-developers.com/showthread.php?t=2588979">here</a>( easier to use)

<b>Step 4:</b> Open Flashtool, click on BLU icon marked like here:

{% img https://dl.dropboxusercontent.com/u/235131545/myblog/flashtool.JPG %}

Shut down the phone, then while plugging the usb cable, press volume down button( For Xperia SP, or Xperia Z, back button for xperia s etc). You will see another pop up, which will say shut down device. Now plug USB cable and <a href="http://unlockbootloader.sonymobile.com/fastboot-buttons">fastboot button</a> at same time, The device will be loaded as flashmode. Now Flashtool will ask for unlock code, which you will get from `Step One`. Thus the device will be unlocked.

Alternate option: follow instructions from this  <a href=" http://unlockbootloader.sonymobile.com/instructions">sonymobile</a> where you can unlock the device from command prompt. 

PS: If you see logs in Flashtool like: 'your device can't be unlocked', don't believe it. You just haven't installed the correct drivers yet.
Rooting device is NOT required to flash ROM. 

<b>Step 5:</b> Hard part is done, now here comes the easy works. Download <a href="https://sites.google.com/site/projectfreexperia/download">CyanogenMod</a>. Download a ROM which is preferred by you(Kitkat, JB, or ICS) also compatible for your device. Download GAPPS(Google app packages) from this <a href="http://forum.xda-developers.com/showthread.php?t=2523640">thread</a>. Put them in SD card root directory. 

<b>Step 6:</b> This is Copy-pasted from <a href="http://wiki.cyanogenmod.org/w/Install_CM_for_huashan">CyanogenMod website</a>.(For Xperia SP or any device)

+ Download CyanogenMod you wish to install.

+ Extract the boot.img from the zip to your computer, you will need this file for fastboot.

+ Place the CyanogenMod rom .zip file on the root of the SD card. Optional: Place any supplemental packages' .zip file on the root of the SD card. Put the phone into fastboot mode.</li>
+ Open a terminal on the PC and enter the following:
        fastboot -i 0xfce flash boot boot.img
        fastboot -i 0xfce reboot 

+ While the device reboots, press the Volume rockers(Volume up down keys randomly) a few times to load recovery.

+ Once the device boots into the ClockworkMod Recovery, use the physical volume buttons to move up and down. On most devices, the power button is used to confirm your selection, although for some devices the power button is used as a "back" button to go up one level, in which case the home button is used to confirm the selection.

  Optional/Recommended: Select backup and restore to create a backup of the current installation on the Xperia SP.

+ Select the option to wipe data/factory reset. 

+ Select Install zip from sdcard.

+ Select Choose zip from sdcard.

+ Select the CyanogenMod file you placed on the sdcard. You will then need to then confirm that you do wish to flash this file. Optional: Install any additional packages you wish using the same method. Once the installation has finished, return back to the main menu, and select the reboot system now option. The Xperia should now boot into CyanogenMod.

PS: If your device blacks out( I mean backlit on but no display, no need to panic. Your phone is in recovery mode. You can either remove battery or force restart like I did in my Xperia SP: <a href="http://userguide.sonymobile.com/referrer.php?region=global-en&amp;product=xperia-sp#!Turning-on-or-off-the-device---heading-only.html">LINK</a>).