# 一.905D单独编译和烧录dts

## 1.以p231 1g为例

需要已经成功编译过kernel的基础上才能单独编译dts，以下执行路径为SDK根目录

1. 编译命令

   make -C common O=../out/target/product/p231/obj/KERNEL_OBJ ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- gxl_p231_1g.dtb

2. 生成dtb文件路径

   out/target/product/p231/obj/KERNEL_OBJ/arch/arm64/boot/dts/amlogic/gxl_p231_1g.dtb

3. uboot下单独更新方法

- SD Card

  store erase dtb;fatload mmc 0 1080000 gxl_p231_1g.dtb;store dtb write 1080000

- U盘

  usb start; fatload usb 0 1080000 gxl_p231_1g.dtb;store erase dtb;store dtb write 1080000

  

## 2.以p231 2g为例

需要已经成功编译过kernel的基础上才能单独编译dts，以下执行路径为SDK根目录

1. 编译命令

   make -C common O=../out/target/product/p231/obj/KERNEL_OBJ ARCH=arm64 CROSS_COMPILE=aarch64-linux-gnu- gxl_p231_2g.dtb 

2. 生成dtb文件路径

   out/target/product/p231/obj/KERNEL_OBJ/arch/arm64/boot/dts/amlogic/gxl_p231_2g.dtb 

3. uboot下单独更新方法

- SD Card

  store erase dtb;fatload mmc 0 1080000 gxl_p231_2g.dtb;store dtb write 1080000 

- U盘

  usb start; fatload usb 0 1080000 gxl_p231_2g.dtb;store erase dtb;store dtb write 1080000 



# 二.差分升级

```
./build/tools/releasetools/ota_from_target_files -i ../dm_release/build/loc
al_sample/1.8/DM13S5TSC/SAMPLETSC/target/p231-target_files-20170626_1.8.zip ../dm_release/build/local_sample/1.7/DM13S5TSC/SAMPLETSC/target/p231-target_files-20170609_1.7.zip 8_7.zip
```

```
target_file=$project_path/out/target/product/$DEVICE_TYPE/obj/PACKAGING/target_files_intermediates/p231-target_files-20170626_1.8.zip
```



# 三.编译releasekey

要对Android系统进行签名，需要生成四种类型的key文件:

- a)releasekey （testkey）

- b)media

- c)shared

- d)platform

1. 进入/android_src/development/tools目录。 

2. 使用make_key工具生成签名文件。需要分别生成 releasekey，media，shared，platform。

   ```
   ./make_key releasekey '/C=CN/ST=JiangSu/L=NanJing/O=Company/OU=Department/CN=Your Name/emailAddress=YourE-mailAddress'
   //系统将会提示输入针对各种key的密码，按照提示输入即可，提示输入密码时直接回车,将会生成 //releasekey.pk8 和 releasekey.x509.pem文件，其中 *.pk8是生成的私钥，而*.x509.pem是公钥，生成时//两者是成对出现的.
   ```

3. demo：

   ```
   ./make_key releasekey '/C=CN/ST=Guangdong/L=Shenzhen/O=OmniRemotes/OU=Department/CN=test/emailAddress=test@omniremotes.com'
   ```

4. 在/android_src/device/amlogic/common/目录下创建文件夹keys，将生成的这8个key拷贝到android_src/device/amlogic/common/keys 目录下。

5. 在/android_src/device/amlogic/p212.mk 中添加

   PRODUCT_DEFAULT_DEV_CERTIFICATE :=device/amlogic/common/keys/releasekey

6. 修改/build/core/makefile 

   ```
   ifeq ($(DEFAULT_SYSTEM_DEV_CERTIFICATE),build/target/product/security/testkey)
   BUILD_KEYS := test-keys
   else
   - BUILD_KEYS := dev-keys   
   +BUILD_KEYS := release-keys   
   endif
   ```

7. 修改\system\sepolicy\keys.conf 文件 

   ```
   # Example of ALL TARGET_BUILD_VARIANTS
   [@RELEASE]
   ENG       : $DEFAULT_SYSTEM_DEV_CERTIFICATE/testkey.x509.pem
   USER      : $DEFAULT_SYSTEM_DEV_CERTIFICATE/testkey.x509.pem
   USERDEBUG : $DEFAULT_SYSTEM_DEV_CERTIFICATE/testkey.x509.pem
   以上三个testkey.x509.pem改成releasekey.x509.pem
   ```



# 四.HDCP2.2 KEY 

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

5. 把补丁放在相应的git仓库中 然后到那个目录下  git apply --ignore-space-change  xxx.patch就可以了 

   



