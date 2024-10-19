**QBox Setup Guide**

These are the steps to follow to build your QBox.

TIP: Assemble and test your electronic parts on the bench BEFORE installing them into your QBox.

1. Burn the OS Image of your choice to an SD Card (All Pi's) and/or load it onto an NVMe SSD if you are installing one (QBOX4/5 only). If you are unfamiliar with this process a useful Guide is here.

    https://raspberrytips.com/boot-from-ssd-on-raspberry-pi/

2. Attach a keyboard, mouse and monitor to your Raspberry Pi, insert the SD Card or fit the SSD you have just installed the OS on and boot up. Complete the OS installation procedure for the OS and if all is well you should end up with a command line prompt or desktop on your HDMI monitor.

3. Connect your Pi to the Internet and test the connection is working.

4. Open a terminal from your desktop if necessary and then move to your root directory and download the "QBox_Start" zip file with
5. wget https://github.com/arqtv/QBOX/blob/main/QBox_Start.zip Once the zip file has downloaded you may unzip it.

6. Copy the firmware file for your particular LCD from the unzipped firmware folder to /lib/firmware if running a Raspberry Pi OS or to whatever directory is relevant for your particular OS.

7. Open your boot/config.txt or if Pi 5 open boot/firmware/config.txt file with nano or the editor of your choice and copy the config lines for your LCD from the LCDConfig file to the end of your config.tx file - typically under [all], then save the edited file and close nano.   

8. Reboot your Pi and check your LCD is displaying cli text or the Desktop you installed.

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
   
5. Open the LCDconfig.txt file and locate the lines for your LCD, in this example LCD-3 which is using st7789vl driver. Highlight and Copy the config lines to the end of your /boot/config.txt or if Pi5 /boot/firmware/config.txt file. 

:~/panel-mipi-dbi $ cat LCD_config.txt


 TO BE EDITED BELOW
 ```
   5. Once the firmware file is in the correct place, you can modify your config.txt to the following (you will need to use sudo to modify the file):

      ```text
      dtparam=i2c_arm=on
      #dtparam=i2s=on
      dtparam=spi=on
      
      [all]
      
      dtoverlay=mipi-dbi-spi,spi0-0,speed=32000000
      dtparam=compatible=mipi-dbi-st7789v\0panel-mipi-dbi-spi
      dtparam=width=320,height=240
      dtparam=reset-gpio=27,dc-gpio=25,backlight-gpio=18
      ```
   6. Now, after a reboot, you should see the CLI appear on the LCD. To run the desktop environment, you can run:
      ```text
      wayfire
      ```
      The desktop environment should be up and running!
