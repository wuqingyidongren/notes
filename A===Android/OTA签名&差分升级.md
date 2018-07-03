# 1.OTA签名

```
java –jar out/host/linux-x86/framework/signapk.jar –w gingerbread/build/target/product/security/testkey.x509.pem                                      gingerbread/build/target/product/security/testkey.pk8 update.zip update_signed.zip

//update.zip 是我们已经打好的包，update_signed.zip包是命令执行完生成的已经签过名的包

mmm build/tools/signapk/ -B  就会在生成out/host/linux-x86/framework/signapk.jar 
```



# 2.差分升级差分包制作

```
./build/tools/releasetools/ota_from_target_files -i p231-target_files-20170626_1.8.zip p231-target_files-20170609_1.7.zip 8_7.zip

target_file=$project_path/out/target/product/$DEVICE_TYPE/obj/PACKAGING/target_files_intermediates/p231-target_files-20170626_1.8.zip
```

