# 开机动画&开机logo

## 1.开机动画

1. 开机动画调试：system/media/bootanimation.zip 

2. 开机动画制作：

   ![](C:\Users\SZH8007\Desktop\clipboard.png)

   <http://www.cnblogs.com/andriod-html5/archive/2012/04/05/2539383.html> 

## 2.开机Logo

开机logo调试：

- 把logo.img放到TF卡中，进uboot,执行sdc_update logo logo.img，然后reset即可。

- U盘usb_update logo logo.img 



device\amlogic\common\products\mbox

mbox.mp4就是开机视频，但需要修改/amlogic/p321/system.prop

\#add for video boot, 1 means use video boot, others not .

service.bootvideo=1