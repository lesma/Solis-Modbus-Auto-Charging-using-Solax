# Installation

If you are installing HA for the very first time and are on the onboarding screen, you can click "Alternatively, you can restore from a previous backup" and follow the instructions to install.  If you already have HA installed, do the following:

1. Download the below .tar file (Right Click and choose "save link as")
2. In Home Assistant, go to Settings > System > Backups
3. Hit the three dots top right and choose "Upload Backup"
4. Navigate to the .tar file and upload it
5. The backup file will now appear in your HA list. Double click on it, choose "full backup", then hit "Restore". The process will install all code.
6. You will need to alter a couple of settings to suit your own installation.
* Solcast API Key
* Openweathermap API Key
* Solax Integration settings (you must enter the correct IP address of your Modbus device which connects to your inverter)

HA Credentials:

* user: default_user
* password: abcde12345
* Please change these...

-----

After Installation please read the [User Guide](https://github.com/jevburchell/Solis-Modbus-Auto-Charging-using-Solax/blob/main/README.md#user-guide) carefully to understand what each control does and how the dashboard works.

-----

# .tar File

To download, right-click and choose "save link as"

[Download Here](https://www.dropbox.com/scl/fi/3ukvgizdqn8eqj1schhkp/Jev_Solis_Auto_charging_V3.2.tar?rlkey=zcmp6sjenppd0n0l6v9he9gk5&dl=0)

Note: this file is hosted on Dropbox as it exceeds the 25Mb file limit size imposed by Github.
