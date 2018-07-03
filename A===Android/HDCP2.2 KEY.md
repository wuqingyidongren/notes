# HDCP2.2 KEY

1. U盘： 

   ```
   usb start
   fatload usb 0 12000000 hdcp22_fw_private.bin
   keyman write hdcp22_fw_private 32 12000000
   save
   ```

2. tf卡： 

   ```
   mmcinfo
   fatload mmc 0 12000000 hdcp22_fw_private.bin
   keyman write hdcp22_fw_private 32 12000000
   save
   ```

3. fireware.le文件拷贝到 /system/etc/firmware/目录下,chmod 644  /system/etc/firmware/firmware.le

4. 验证：

   ​	cat /sys/class/amhdmitx/amhdmitx0/hdcp_mode    这个是  22

   ​	cat /sys/module/hdmitx20/parameters/hdmi_authenticated 这个是1的话 ,key就是ok