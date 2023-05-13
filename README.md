This MOD adds an external relay module to the Victron Cerbo GX to add additional relays and digital inputs. This tutorial is intended for using ethernet or wifi to connect to the relay module.

This box provides relays, that can be controlled with Modbus Serial, but also over IP and with various additional protocols: https://fr.aliexpress.com/item/4000999069820.html They have variants of 4 or 8 relays.

CONFIGURATION OF THE RELAY MODULE
Connect the ethernet interface of the module and access to its configuration page (192.168.1.100 and admin/admin by default)
you can also connect directly to the relay module wireless AP to complete the setup: AP IP = 192.168.7.1, ssid = dtrelay(xxxx), password = dtpassword
on the settings tab: you will need to set either the ETH or STA settings depending on which connection method used. Make note of the IP address.
on the relay connect tab: Configure TCP Server to Modbus-TCP and local port 502

INSTALL/UNINSTALL/FULL REMOVAL
**********************************************************************************
VENUS OS UPDATES WILL REQUIRE YOU TO RE-INSTALL THIS PACKAGE. SEE 'RE-INSTALL' BELOW
REQUIRES SSH WITH ROOT ACCESS
**PLEASE READ ALL STEPS BEFORE PROCEEDING**

NEW INSTALL STARTS HERE (COMPLETE ALL STEPS)

cd /data/
wget github.com/thomasinaz41213/Venus_rgpio/releases/download/Latest/rgpio.tar.gz
tar -xvzf rgpio.tar.gz
rm rgpio.tar.gz
***RE-INSTALL JUST NEEDS TO START HERE***
chmod a+x /data/rgpio
/data/rgpio/install.sh

Open node red in a browser and import/deploy the Digital Inputs and Relays flows (both flows are necessary for full function of the relays)

Display and configure the additional 4x relais in the Venus GUI
Name and display the additional relays from Settings / Relays in the GUI.
You can swipe to a specific page with the 6x relays, but I believe this is provided by the excellent GuiMods add-on from Kwinderm.

Using Node-Red for controlling the 8x additional relays
The Relays 3, 4, 5 and 6 are normally controlled with Victron’s Relay Nodes, and their status are correctly reported on the Victron GUI
Use flow "Remote Relays 1-8 (TCP).json" as example

The additional 4x relais 7, 8, 9 and 10 are exposed only through Node-Red

10/ Additional Digital Inputs
The Dingtian relay box also offer additional Digital inputs.
The aditional digital inputs are only available through Node-Red 



**********************************************************************
UNINSTALL
THIS WILL RESTORE THE ORIGINAL FILES, AND LEAVE THE RGPIO DIRECTORY INTACT

chmod a+x /data/rgpio
/data/rgpio/uninstall.sh


*********************************************************************
PERMANENTLY REMOVE RGPIO DIRECTORY AND IT'S CONTENTS
WARNING: IF YOU DO THIS YOU WILL NEED TO RUN THE FULL INSTALL INSTRUCTIONS
         ABOVE IF YOU WISH TO REINSTALL AT A LATER DATE

chmod a+x /data/rgpio
/data/rgpio/remove.sh

**********************************************************************
