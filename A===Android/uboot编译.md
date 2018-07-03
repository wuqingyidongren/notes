# 1.uboot编译

```
cd uboot
make distclean
./mk gxl_p212_v1
```

注意： ./mk gxl_p231_v1 用的是Q201 【uboot\board\amlogic\gxl_p231_v1\README可见】

​	uboot\board\amlogic\gxl_p231_v1\Kconfig

​	uboot\board\amlogic\configs 

编译完之后执行：

```
#!/bin/bash

TARGET_DEVICE=$1
if [ ! $TARGET_DEVICE ]; then
TARGET_DEVICE=$TARGET_PRODUCT
fi

cp fip/u-boot.bin ../device/amlogic/$TARGET_DEVICE/
cp fip/u-boot.bin.sd.bin ../device/amlogic/$TARGET_DEVICE/upgrade/
cp fip/u-boot.bin.usb.bl2 ../device/amlogic/$TARGET_DEVICE/upgrade/
cp fip/u-boot.bin.usb.tpl ../device/amlogic/$TARGET_DEVICE/upgrade/
cp fip/u-boot.bin ../out/target/product/$TARGET_DEVICE/
cp fip/u-boot.bin ../out/target/product/$TARGET_DEVICE/upgrade/
cp fip/u-boot.bin.sd.bin ../out/target/product/$TARGET_DEVICE/upgrade/
cp fip/u-boot.bin.usb.bl2 ../out/target/product/$TARGET_DEVICE/upgrade/
cp fip/u-boot.bin.usb.tpl ../out/target/product/$TARGET_DEVICE/upgrade/
```

