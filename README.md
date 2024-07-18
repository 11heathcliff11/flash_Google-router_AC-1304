# flash_openwrt_on_google-router_AC-1304

Flash OpenWRT on Google Wifi router AC-1304

Here are the steps to flash a new ROM on the Google Wifi router AC-1304
******
Before you start:
* Flashing OpenWRT image
* OpenWRT image name may vary, adjust dd comand accordingly
* Router IP address may vary
* SW7 button highlighted in attached image: opened_AC-1304.jpg
* SW7 button is the developer mode button
* I used an Anker USB C Hub A8346, connected it to the USB C port on the router and connected USB pendrive to this dock
* Powered the hub via a powerbank, any powerbank will suffice
* Any other USB hub or dock will work as long it connects via USB C and has power
* Ensure the hub or dock has power because the Google Wifi router AC-1304 only has one USB C port for both power and data


******
Part I: Restoring Stock Firmware
******
* Download the stock firmware recovery image from Google.
* Create a USB recovery drive using the OnHub Recovery Utility tool.
* Follow these steps to enter recovery mode:
* Hold the Reset button on the front of the case
* Connect power to the device
* After around 16 seconds, the LED will blink orange. Release the Reset button.
* Plug in the USB drive containing the recovery image
* The LED will turn off, and recovery will begin
* Wait around 5-6 minutes for the device to automatically reboot. The LED will pulse blue when recovery is complete.

******
Part II: Flashing OpenWrt
******
* Download the OpenWrt factory image for your Google Wifi model.
* Flash the factory image to a USB drive:
* On Windows: Use dd to write the image, e.g., sudo dd if=openwrt-image.bin of=/dev/sdX bs=1M
#### Enter developer mode:
* Locate the single screw at the base plate, unscrew and gently pry and remove the base plate
* Hold the Reset button and connect power
* After 16 seconds, release Reset when LED blinks orange
* After 3 seconds, the LED will pulse orange and amber. Press SW7.
* Device will blink purple and restart
#### Boot from the USB drive:
* Wait for device to reboot (pulsing blue LED)
* When blinking purple, plug in the USB drive with OpenWrt
* Press SW7 again. LED will turn off, and it will boot from USB.
* Wait 30 seconds and check if it worked by pinging 192.168.1.1 from a LAN port.

******
Part III: Installing OpenWrt to Internal Storage
******
* Connect LAN cable from PC to router via ethrnet port
#### Transfer the OpenWrt factory image to the router's temporary storage:
* SSH into the router: ssh root@192.168.1.1
* On Windows: pscp openwrt-image.bin root@192.168.1.1:/tmp/
* On Linux/macOS: scp openwrt-image.bin root@192.168.1.1:/tmp/
#### Write the image to internal storage:
* dd if=/dev/zero bs=512 seek=7634911 of=/dev/mmcblk0 count=33
* dd if=/tmp/openwrt-image.bin of=/dev/mmcblk0

* Reboot the device, and it should now be running OpenWrt from internal storage.
* Access the OpenWrt web interface at https://192.168.1.1


******
Optional: Upgrading OpenWrt
******
* To upgrade an existing OpenWrt installation:
* In the web interface, go to System > Backup/Flash Firmware
* Under "Flash new firmware image", select the OpenWrt sysupgrade file
* Upload and flash the new firmware to complete the upgrade
* After any changes, ensure the case is properly reassembled.
