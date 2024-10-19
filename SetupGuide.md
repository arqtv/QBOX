**QBox Setup Guide**

These are the steps to follow to build your QBox.

TIP: Assemble and test your electronic parts on the bench BEFORE installing these into your QBox.

1. Burn the OS Image of your choice to an SD Card and/or load it onto an NVMe SSD if you are installing one. 

2. Attach a keyboard, mouse and monitor to your Raspberry Pi, insert the SD Card or fit the SSD you have just installed the OS on and boot up. Complete the OS installation procedure for the OS and if all is well you should
End up with a command line prompt or desktop on your HDMI monitor.

3. Connect your Pi to the Internet and test the connection is working.

3. Open a terminal from your desktop if necessary and then move to your root directory and download the "QBox_Startup" zip file with wget https://gitTBA and unzip it.

4. Copy the firmware file for your particular LCD from the unzipped firmware folder to /lib/firmware if running a Raspberry Pi OS or to whatever directory is relevant for your particular OS.

4. Open your boot/config.txt or if Pi 5 open boot/firmware/config.txt file with nano or the editor of your choice and copy the config lines for your LCD from the LCDConfig file to the end of your config file - typically under [all], then save the edited file and close nano.   

5. Reboot your Pi and check your LCD is displaying cli text or the Desktop you installed.

Once you are happy everything is working as expected you can begin to assemble your QBox.  


EXAMPLE

Here is an example for LCD-3, the Waveshare 2.8inch Capacitive Touch LCD SKU configured in Landscape mode. This LCD requires the st7789vl driver. If you wanted a Portrait LCD you would use the st7789vp driver. 

1. From the command line on your Pi move to the boot directory

$ cd /boot

2. Download the QBox installation zip file from the git repo with

$ wget https://gitTBA.../LCD_Install.zip 

3. Unzip the zip file with

$ unzip 

4. Move to the LCD_Install directory

$ cd LCD_Firmware

5. Open the LCDconfig.txt file and locate the lines for your LCD, in this example LCD-3. Highlight and Copy these lines to the end of your /boot/config.txt or if Pi5 /boot/firmware/config.txt file. 

$LCD_Firmware: cat LCD_config.txt







   1. Download the Raspberry Pi Image Maker from the official source
   2. Use the Image Maker to install Bookworm 64/32 bit onto an SD card
   3. Test the OS on the SD card on your Raspberry Pi
2. Create the firmware file required for your LCD
   1. The MIPI-DBI-SPI Overlay needed to use the LCD with your Raspberry Pi requires a firmware file to provide the driver with the correct boot sequence for your LCD. In this example, we are going to use the ST7789v LCD.
   2. Once you have found the correct .txt containing the boot sequence for your LCD, you can compile it into a .bin file with the following command (N.B. You will need to set up a Python Virtual Environment to use the mipi-dbi-cmd).

      ```text
      ./mipi-dbi-cmd mipi-dbi-st7789v.bin ST7789.txt
      ```
      N.B. It is important that you name your bin file something unique so it does not match names with any other drivers on the system. For example, if it is named st7789v.bin, it may clash with the fbtft_st7789v driver.
      
   3. Now you have a .bin file for your LCD, you will need to move it to the Raspberry Pi. One way to do this is via SCP:

      ```text
      sudo scp mipi-dbi-st7789.bin viota@[YOUR RASPBERRY PI SSH ADDRESS]:/home/viota/
      ```
   4. Now, on your Raspberry Pi, you can copy the new firmware file into the /lib/firmware/ folder (you will need sudo to do this):

      ```text
      sudo cp /home/viota/st7789.bin /lib/firmware/
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