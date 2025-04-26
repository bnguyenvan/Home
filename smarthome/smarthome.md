# Smart Home

## Sonoff
1. Upgrade tasmota firmware
- Access to web interface at: http://IP-ADDRESS
- Search for password in Bitwarden with keyword 'Sonoff-Password'
- Firmware file: https://ota.tasmota.com/tasmota/release/

    Note that Sonoff basic has Flash Memory: 1MB
    + English version: http://ota.tasmota.com/tasmota/release/tasmota.bin.gz
    + Vietnamese version: http://ota.tasmota.com/tasmota/release/tasmota-VN.bin.gz

![alt text](/images/SonoffBasic.png)

From Main Menu > Firmware Upgrade > Upgrade by web server > OTA Url, past link of version:

![alt text](/images/SonoffBasicTasmotaFirmwareUpgrade.png)

Device will restart. When you login and see this screen, just waiting for a few minutes as firmware is upgrading:
![alt text](/images/SonoffBasicTasmotaFirmwareUpgrade1.png)

If device could not upgrade, try to download file .bin to local and upgrade via file, if get error "Not enough space":

- Main Menu > Console:
- Enter: otaurl http://ota.tasmota.com/tasmota/release/tasmota.bin.gz
- Then, Enter: upload 1

Set timezone: https://tasmota-tz.cloudfree.io/
Copy command to console: Backlog Latitude 10.822; Longitude 106.6257; TimeZone +07:00

2. Kết nối cho Sonoff basic bản nội địa:

- Bên trái: IN
- Bên phải: OUT
- Trên: N (Trung tính)
- Dưới: L (Dây nóng)

![alt text](/images/SonoffBasicChina.png)

3. Sonoff Basic Tasmota Console command
```
PowerOnState 0 => OFF = keep relay(s) OFF after power up
PowerOnState 1 => ON = turn relay(s) ON after power up
```
The PowerOnState device configuration parameter is applied when the device is initially powered up. It does not apply to device warm restarts.
Further information could be found here: https://tasmota.github.io/docs/PowerOnState
