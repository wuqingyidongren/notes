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



 



