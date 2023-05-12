This MOD adds an external relay module to the Victron Cerbo GX to add additional relays and digital inputs. This tutorial is intended for using ethernet or wifi to connect to the relay module.

This box provides relays, that can be controlled with Modbus Serial, but also over IP and with various additional protocols: https://fr.aliexpress.com/item/4000999069820.html They have variants of 4 or 8 relays.

CONFIGURATION OF THE RELAY MODULE
Connect the ethernet interface of the module and access to its configuration page (192.168.1.100 and admin/admin by default)
you can also connect directly to the relay module wireless AP to complete the setup: AP IP = 192.168.7.1, ssid = dtrelay(xxxx), password = dtpassword
on the settings tab: you will need to set either the ETH or STA settings depending on which connection method used. Make note of the IP address.
on the relay connect tab: Configure TCP Server to Modbus-TCP and local port 502

INSTALLATION
**********************************************************************************
To reinstall after uninstalling, or after a Venus update, start from step 2
To uninstall scroll down

The below steps requires to ssh to the Cerbo GX with root access.
I suggest you download the configuration files in /data/rgpio so it remains there even after SW upgrades.

1/
cd /data/
wget github.com/thomasinaz41213/Venus_rgpio/releases/download/Latest/rgpio.tar.gz
tar -xvzf rgpio.tar.gz
rm rgpio.tar.gz

2/ Creating /dev/gpio links at boot so the bus services are automatically created 
ln -s /data/rgpio/conf/S90rgpio_pins.sh /etc/rcS.d/S90rgpio_pins.sh


3/ Modify Relaystate Python script
mv /opt/victronenergy/dbus-systemcalc-py/delegates/relaystate.py /opt/victronenergy/dbus-systemcalc-py/delegates/relaystate.py.ori
cp /data/rgpio/conf/relaystate.py /opt/victronenergy/dbus-systemcalc-py/delegates/relaystate.py


4/ Need to add Relays 3, 4, 5 and 6 in /etc/venus/gpio_list so they can be configured on the GUI
mv /etc/venus/gpio_list /etc/venus/gpio_list.ori
cp /data/rgpio/conf/gpio_list  /etc/venus/gpio_list


5/ Need to update Node-Red service for adding the 4x relays
mv /usr/lib/node_modules/@victronenergy/node-red-contrib-victron/src/services/services.json /usr/lib/node_modules/@victronenergy/node-red-contrib-victron/src/services/services.json.ori
cp /data/rgpio/conf/services.json /usr/lib/node_modules/@victronenergy/node-red-contrib-victron/src/services/services.json


6/ Reboot the Cerbo


7/ Reboot the relay module (login to the relay ui and scroll down to 'reboot')


8/ Open node red in a browser and import/deploy the Digital Inputs and Relays flows (both flows are necessary for full function of the relays)


8/ Display and configure the additional 4x relais in the Venus GUI
Name and display the additional relays from Settings / Relays in the GUI.
You can swipe to a specific page with the 6x relays, but I believe this is provided by the excellent GuiMods add-on from Kwinderm.


9/ Using Node-Red for controlling the 8x additional relays
The Relays 3, 4, 5 and 6 are normally controlled with Victron’s Relay Nodes, and their status are correctly reported on the Victron GUI
Use flow "Remote Relays 1-8 (TCP).json" as example

The additional 4x relais 7, 8, 9 and 10 are exposed only through Node-Red

10/ Additional Digital Inputs
The Dingtian relay box also offer additional Digital inputs.
The aditional digital inputs are only available through Node-Red 



**********************************************************************
UNINSTALL

The following steps will uninstall the modified files and restore the original files.
The following steps require you to SSH with root access

1/
rm /etc/rcS.d/S90rgpio_pins.sh
cp /opt/victronenergy/dbus-systemcalc-py/delegates/relaystate.py.ori /opt/victronenergy/dbus-systemcalc-py/delegates/relaystate.py
cp /etc/venus/gpio_list.ori /etc/venus/gpio_list
cp /usr/lib/node_modules/@victronenergy/node-red-contrib-victron/src/services/services.json.ori /usr/lib/node_modules/@victronenergy/node-red-contrib-victron/src/services/services.json

2/ If you wish to remove all install files (if you do this, and later want to reinstall the relay module, you will need to start installation process from step 1 above)
rm -r /data/rgpio

3/
reboot


*********************************************************************
