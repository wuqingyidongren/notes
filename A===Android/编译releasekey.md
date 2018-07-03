# 编译releasekey

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
   ./make_key releasekey '/C=CN/ST=Guangdong/L=Shenzhen/O=xx/OU=Department/CN=test/emailAddress=test@xx.com'
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

