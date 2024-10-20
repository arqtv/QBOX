**QBox Setup Guide**

These are the steps to follow to build your QBox.

TIP: Assemble and test your electronic parts on the bench BEFORE installing them into your QBox.

1. Burn the OS Image of your choice to an SD Card (For all Pi's) and/or load the image onto an NVMe SSD if you are installing one (For Pi4 or Pi5 only). If you are unfamiliar with this process a useful Guide is here.

    https://raspberrytips.com/boot-from-ssd-on-raspberry-pi/

2. Attach a keyboard, mouse and monitor to your Raspberry Pi, insert the SD Card or fit the SSD you have just installed the OS on and boot up. Complete the OS installation procedure for the OS and if all is well you should end up with a command line prompt or desktop on your HDMI monitor.

3. The Pi3A+ and Zero 2 W have only 500MB of RAM and can be somewhat sluggish particularly if using a desktop and a browser. Perfomance can be improved markedly by increasing the size of the swap file available on these platforms from the derisory fixed 200 Meg default setting to the dynamic setting. A short tutorial is here

    https://pimylifeup.com/raspberry-pi-swap-file/
       
4. Connect your Pi to the Internet and test the connection is working. Quick and easy is to ping 1.1.1.1

5. Open a terminal from your desktop if necessary and from your home directory download the "QBox_Start" zip file with

    wget https://github.com/arqtv/QBOX/blob/main/QBox_Start.zip

6. Once the zip file has downloaded you may unzip it.

7. Copy the firmware file for your particular LCD from the unzipped firmware folder to /lib/firmware if running a Raspberry Pi OS or to whatever directory is relevant for your particular OS.

8. Open your boot/config.txt or if Pi 5 open boot/firmware/config.txt file with nano or the editor of your choice and copy the config lines for your LCD from the LCDConfig file to the end of your config.tx file - typically under [all], then save the edited file and close nano.   

9. Reboot your Pi and check your LCD is displaying cli text or the Desktop you installed. If you still have your HDMI monitor attached you will see the extended desktop. Use the arandr utility to view the extended desktop and the location of the LCD display with respect to the HDMI display. The mouse pointer defaults to the LCD display area. Move your mouse to the right and down to bring it into view on the HDMI monitor.

Once you are happy everything is working as expected you can begin to assemble your QBox.  


EXAMPLE

Here is an example for LCD-3, the Waveshare 2.8inch Capacitive Touch LCD SKU configured in Landscape mode. This LCD requires the st7789vl driver. If you wanted a Portrait LCD you would use the st7789vp driver. 

1. From the command line in your home directory Download the QBox installation zip file from the git repo with

:~ $ wget https://github.com/arqtv/QBOX/raw/refs/heads/main/QBox_Start.zip

2. Unzip the zip file with

:~ $ unzip QBox-Start.zip

3. Move to the extracted panel-mipi-dbi directory

:~ $ cd panel-mipi-dbi

4. Copy the bin file for LCD-3 to the /lib/firmware directory

:~/panel-mipi-dbi $ sudo scp mipi-dbi-st7789vl.bin /lib/firmware/st7789vl.bin
   
5. Open the LCDconfig.txt file and locate the lines for your LCD, in this example LCD-3, a 2.4inch display which is using st7789vl driver. Highlight and Copy the config lines to the end of your /boot/config.txt or if Pi5 /boot/firmware/config.txt file. 

:~/panel-mipi-dbi $ cat LCD_config.txt

6. Modify your config.txt to the following (you will need to use sudo to modify the file):

      ```text
      dtparam=i2c_arm=on
      #dtparam=i2s=on
      dtparam=spi=on
      
      [all]
      
      dtoverlay=mipi-dbi-spi,spi0-0,speed=40000000
      dtparam=compatible=mipi-dbi-st7789vl\0panel-mipi-dbi-spi
      dtparam=width=320,height=240,width-mm=49,height-mm=37
      dtparam=reset-gpio=27,dc-gpio=25,backlight-gpio=18
      ```
      
 7.  If gpio 18 is needed for a HAT (EG the IQ Audio DAC+ HAT) then use backlight-gpio-23 and move the Jumper on the QBox Carrier board to the 23 position.

 8. Now, after a reboot, you should see the CLI appear on the LCD. If your OS is the Desktop version then to run the desktop environment, you can run:
      ```text
      wayfire
      ```
      The desktop environment should now be up and running! You can change the boot to cli or boot to desktop in Raspberry Pi Preferences.

