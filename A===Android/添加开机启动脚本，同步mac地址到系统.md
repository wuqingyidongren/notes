# 添加开机启动脚本，同步mac地址到系统

## 1.device\amlogic\p231\init.amlogic.board.rc    

//这里可以初始化节点权限

```
service initcmd /system/bin/initcmd.sh
class main
user root
#disabled（默认值）
group root
oneshot2.device\amlogic\p231\p231.mk
```



## 2.device\amlogic\p231\p231.mk 

复制sh文件：

PRODUCT_COPY_FILES += \

​	$(LOCAL_PATH)/initcmd.sh:system/bin/initcmd.sh



## 3.device\amlogic\p231\目录添加initcmd.sh脚本 

```
#!/system/bin/sh 
#同步mac地址
busybox ifconfig eth0 down
echo  mac > /sys/class/unifykeys/name
var=$(cat /sys/class/unifykeys/read)
#echo $var
busybox ifconfig eth0 hw ether $var
busybox ifconfig eth0 up
```

## 4.条件启动

 init.amlogic.board.rc    

```
on property:init.svc.bootanim=stopped //条件启动
    start preinstall
```

