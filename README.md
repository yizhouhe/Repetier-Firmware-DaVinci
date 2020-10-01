## Da Vinci Firmware based on Repetier (1.0.4)
============================


Build Status: [Build without error with Arduino IDE]    
    
      

This firmware is based on the popular repetier firmware 1.0.4 and incoprorate changes from LUC's modification to make it work for Da Vinci <B>1.0/A, 2.0 single fan, 2.0/A dual fans and also AiO</B> (NB:scanner function is not supported so AiO will work like an 1.0A)   

## Do not use it on Da Vinci 1.0 Color, Super, PRO, Plus, Jr, Mini or Mano.    

Be noted original Repetier FirmWare is not compatible wih Davinci boards.   
 
If you changed the stock board with RAMPS or any other none stock board, Use the orginal Repetier Firmware https://github.com/repetier/Repetier-Firmware or Marlin https://github.com/MarlinFirmware/Marlin.

YOU MIGHT DAMAGE YOUR PRINTER OR VOID YOUR WARRANTY (if there is any), DO IT ON YOUR OWN RISK. 
It is possible to revert to stock firmware but require additional guides and tools, so be sure of what you are doing.

***
AiO scanner is not currently supportted.

The board can be easily exposed by removing the back panel of the printer secured by two torx screws.  Supported boards have a jumper labeled JP1 (1.0, 2.0, AIO), second generation boards (1.0A and 2.0A) have a jumper labeled J37. 
***

Here are just a few of the benefits of using this firmware:

* It works with host software such as [repetier host](http://repetier.com) and [OctoPrint](http://octoprint.org/), [Cura](https://ultimaker.com/software/ultimaker-cura), Simplify3D, etc... giving you full control of your hardware.
* It works stand alone (not connected to PC) if you use a WIFI SD Card or SD card extender. 
* It allows the use of clear ABS (by disabling optical sensors), as well as other arbitrary filament brands/types as temperatures can be controlled freely and there is no requirement for chiped cartridges. 

The current firmware is based on:
 
[Repetier Firmware (1.0.4 )](https://github.com/repetier/Repetier-Firmware)
 
[LUC Repetier-Firmware-4-Davinci (0.92)](https://github.com/luc-github/Repetier-Firmware-4-Davinci)    

[bgm370 Da Vinci 1.0 FW (0.91)](https://github.com/bgm370/Repetier-Firmware)

***
## Current Status
#### Alpha - It complied fine, have not tested on printer

***
## Installation
1. With the machine off remove the back panel and short the jumper JP1 or J37 depending on model.  Some Boards do not have jumper pins exposed but can still be shorted with a conductive wire.
2. Turn the machine on and wait a few seconds then turn it off again.  The machine will have been flashed removing the current stock firmware and allowing it to be detected as a normal arduino DUE. NOTE: Windows users may need to install drivers to detect the board.  Consult the Voltivo forums.   
Note : points 1 and 2 are only needed to wipe the stock fw or a corrupted fw, for update they are not necessary.
Note 2: remove the jumper before flashing if still there
3. Use an arduino IDE supporting arduino DUE, [1.8.13](https://www.arduino.cc/en/Main/Software) with Arduino SAM Boards (32-bits ARM Cortex-M3) 1.6.12 Arduino Due module from board manager 
4. Update variants.cpp and USBCore.cpp arduino files under your user folder with the ones present in "\src\ArduinoDUE\AdditionalArduinoFiles\Da Vinci 1.0 - Arduino IDE 1.8.13 -Due 1.8.3\Arduino15\packages\arduino\hardware\sam\1.6.12". For windows 10, the target file to be replaced is located under C:\Users\[Your username]\AppData\Local\Arduino15\packages\arduino\hardware\sam\1.6.12 
5. Open the project file named repetier.ino located in src\ArduinoDUE\Repetier directory in the arduino IDE. 
6. Modify the DAVINCI define in Configuration.h file to match your targeted Da Vinci.  See below.
7. Under the tools menu select the board type as Arduino DUE (Native USB Port) and the proper port you have connected to the printer.  NOTE: You can usually find this out by looking at the tools -> port menu both before and after plugging in the printer to your computer's USB.
8. Press the usual arduino compile and upload button.    
If done correctly you will see the arduino sketch compile successfully and output in the log showing the upload status.
9. Once flash is done : restart printer         
<H3> If you have black bars and printer is not detected properly, it means you did not do the point 4 properly, so go back to point 4. and make sure you copy the two file to right directory.    

10. After printer restarted <B>do not forget to send G-Code M502 then M500 </B>from repetier's Print Panel tab <B>or from the printer menu "Settings/Load Fail-Safe"</B> and accept to save the new eeprom settings.    
11. When update is complete <B>you must calibrate your bed height!</B>Use manual bed leveling in menu   
12. Next you can calibrate your filament as usual, and second extruder offset if you have.   

Do not forget to modify the configuration.h to match your targeted Da Vinci: 1.0, 2.0 SF or 2.0.   
for basic installation just change :   
'<code>#define DAVINCI 1 // "1" For DAVINCI 1.0, "2" For DAVINCI 2.0 with 1 FAN, "3" For DAVINCI 2.0 with 2 FANS, 4 For AiO （no scanner)</code>'    
  0 for not Davinci board (like DUE/RADDS)    
  1 for DaVinci 1.0 (1Fan, 1 Extruder)   
  2 for DaVinci 2.0 SF (1Fan, 2 Extruders)   
  3 for DaVinci 2.0  (2Fans, 2 Extruders)    
  4 for DaVinci AiO 

Support for 1.0A and 2.0A:  need to change <CODE>#define MODEL 0</CODE>  to  <CODE>#define MODEL 1</CODE>

To repurpose the main Extruder cooling fan to be controlled VIA G-Code instructions M106/M107 as by default fan is automaticaly controled by temperature and not by commands:   
Set REPURPOSE_FAN_TO_COOL_EXTRUSIONS to 1, do not forget to add a fan with power source to cool extruder permanently if you use this option.     

Another excellent tutorial for flashing and installation is here from Jake : http://voltivo.com/forum/davinci-peersupport/501-da-vinci-setup-guide-from-installation-to-wireless-printing but Arduino ide version described is not correct for latest firmware version, use the one mentioned above.   

Or a great video done by Daniel Gonos: https://www.youtube.com/watch?v=rjuCvlnpB7M but Arduino ide version described is not correct for latest firmware version, use the one mentioned above.   


## Implemented
* 1.0.4 [Repetier](https://github.com/repetier/Repetier-Firmware) based   
* Standard GCODE commands   
* Single/Dual extruders support DaVinci 1.0/A, 2.0/A all generations, AiO but no scanner support because no application
* Single Fan / Dual fans support according printer configuration
* Repurpose of second fan usage to be controlled by M106/M107 commands on Da Vinci 2.0/A, 1.0/A need additional fan
* Sound and Lights management, including powersaving function (light can be managed remotely by GCODE)
* Cleaning Nozzle(s) by menu and by GCODE for 1.0, 2.0 and AiO
* Load / Unload filament by menu
* Filament Sensor support (auto loading / alert if no filament when printing)
* Auto Z-probe / average Z position calculation
* Manual Leveling
* Dripbox cleaning for 1.0/2.0
* Customized Menu UI with Advanced/Easy mode (switch in "Settings/Mode" or using Up key/Right key/ Ok key in same time)
* Loading FailSafe settings
* Emergency stop (Left key/Down key/Ok key  in same time)
* Increase extruder temperature range to 270C and bed to 130C
* Add temperature control on extruder to avoid movement if too cold
* Add fast switch (1/10/100mm) for manual position/extrusion
* Command for bed down
* Several fixes from original FW   
* Watchdog   
* Basic Wifi support for module ESP8266 (https://github.com/luc-github/ESP8266/blob/master/README.md#result-of-esp12e-on-davinci) 
* Customized thermistor tables for bed and extruder(s) as Davinci boards do not follow design of others 3D printer boards so standard tables do not work properly [check here](http://voltivo.com/forum/davinci-firmware/438-repetier-91-e3d-v6-extruder#3631)  
* Multilanguage at runtime (EN/FR/GE/IT/NL/SW) more to come if get help : check [here](https://github.com/luc-github/Repetier-Firmware-0.92/issues/123)   
* More to come ....

***
## How to test Watchdog ?
* Connect repetier host and send M281 command.    
This will generate a timeout  after showing "Triggering watchdog. If activated, the printer will reset." in serial terminal.    
If watchdog is enabled properly and working the printer will reset and restart.    
If not, you should have "Watchdog feature was not compiled into this version!" in serial terminal and printer will not automaticaly restart.   


***
## Current menu (not up to date):
Easy: <img src='https://cloud.githubusercontent.com/assets/8822552/4748170/bfa0b7e8-5a69-11e4-80b7-02b9c99fe122.png'>   
Advanced :  <img src='https://cloud.githubusercontent.com/assets/8822552/4748932/bebab9e2-5a7c-11e4-8fea-cdbe3d70820c.png'>   

## Donation:
Every support is welcome: 

<form action="https://www.paypal.com/cgi-bin/webscr" method="post" target="_top">
<input type="hidden" name="cmd" value="_donations" />
<input type="hidden" name="business" value="NCXU8VCD8KCBL" />
<input type="hidden" name="currency_code" value="USD" />
<input type="image" src="https://www.paypalobjects.com/en_US/i/btn/btn_donateCC_LG.gif" border="0" name="submit" title="PayPal - The safer, easier way to pay online!" alt="Donate with PayPal button" />
<img alt="" border="0" src="https://www.paypal.com/en_US/i/scr/pixel.gif" width="1" height="1" />
</form>
   
Especially if need to buy new printer to add Firmware support.
