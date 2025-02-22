## DEPRECATED INFO
# This custom template doesn't work after ESPHOME 2025.2 and will be no longer maintained. Feel free to make a PR if you are interested to support this project.

# LD2411S
[HiLink LD2411S](https://www.hlktech.net/index.php?id=1152) 24GHz mmwave Static and Motion Sensor. ESPHome Integration

![LD2411S](https://www.hlktech.net/res/_cache/auto/14/1459.png)

## Installation
* Copy the .yaml and the .h files in your ESPHome config. 
* Set wifi_ssid, wifi_password, encryption_key, ota_update and fallback_pass in your secrets.yaml from ESPHome.
* Change the ESP board settings and uart_tx_pin, uart_rx_pin to match your setup
* Compile and upload

## Wiring
LD2411S | ESP | 
-------- | -------- |
Vin | 5V | 
GND | GND |
TX | RX |
RX | TX  | 

## Entities Exposed
* Distance (in cm from target)
* Motion (true if target moves)
* Presence (true if target present)
* Bluetooth (turn on-off the bluetooth module)
* Min Motion Distance (Minimum distance to recognise moving targets, adjustable range: 30-735cm)
* Max Motion Distance (Maximum distance to recognise moving targets, adjustable range: 30-735cm. 600cm is the actual maximum value)
* Min Presence Distance (Minimum distance to recognise static (micromotion) targets, adjustable range: 30-425cm )
* Max Presence Distance (Maximum distance to recognise static (micromotion) targets, adjustable range: 30-425cm. 350cm is the actual maximum value)
* Unoccuppied Time (Delay in seconds from occupied to unoccupied, adjustable range: 0-6553 seconds)
* Set Parameter (Button to set the new values for Motion, Presence recognition and Unoccupied Time)
* Reboot Module (Reboot only the LD2411S)
* Factory Reset (Return to default LD2411S settings)
* Restart MCU (Restart the ESP)

## Description
The sensor recognises motion and presence of targets which are in between the distance values given. To change the values, edit the number entities and hit Set Parameter. 
It includes a Bluetooth module, and it's compatible with the [HLKRadarTool](https://www.pgyer.com/Lq8p) app.
OTA updates available also via Bluetooth.
