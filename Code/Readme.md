# YAML Based Configuration

This project can now almost entirely be copied into your existing Home Assistant setup without manually having to create entries through the UI, with the exception of the dashboard. 

All of the files and directories within this folder should be copied to your home assistant "config" directory.

If you have existing template entries in your configuration.yaml file, copy these into the template.yaml and remove them from your configuration.yaml file. 

All templates and helpers required for this project are included in the associated .yaml files and no manual entries are required. 

If you have existing automations and helpers from this project manually configured via UI, you need to delete these prior to restating / reloading HA.

## configuration.yaml
The provided configuration.yaml.example should be used as a guide to modify your existing configuration.yaml.  Essentially, the changes allow the project files to be read and referenced from the solax_automation folder, and still allows configuration via the UI for unrelated items. 

Please read the comments in the configuration.yaml.example file.

## template.yaml
You need to select your inverter type.  Either "default" or "solax hybrid"

Add any template items from your configuration.yaml file into this template.yaml file. 

## Testing and Deploying Changes
Use **Developer Tools** -> **Check Configuration** to check the configuration is valid.  Correct any errors if there are any

Once configuration is validated, **Restart** -> **Quick Reload**
