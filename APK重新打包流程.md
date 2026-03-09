# APK重新打包流程指南

本文档详细介绍了如何使用apktool对APK进行反编译、修改和重新打包的完整流程。

## 所需工具

1. **apktool** - 用于APK的反编译和重新打包
2. **zipalign** - 用于APK的字节对齐（Android SDK工具）
3. **apksigner** 或 **jarsigner** - 用于APK签名
4. **keytool** - 用于创建签名密钥库

## 完整示例命令

以下是完整的命令序列，从反编译到签名：

```bash
# 1. 反编译
cd D:\逆向工程\工具集合\apktool
apktool.bat d gz.apk -o gz-output

# 2. 重新打包
cd D:\temp_apk
apktool.bat b csds-output -o csds-rebuilt.apk

# 3. 字节对齐
"C:\Users\ThinkPad\AppData\Local\Android\Sdk\build-tools\36.1.0\zipalign.exe" -v -p 4 app-release-unsigned.apk app-release-aligned.apk

"C:\Users\Lenovo\AppData\Local\Android\Sdk\build-tools\36.1.0\zipalign.exe"  -v -p 4  原料文件.apk          成品文件.apk
# 4. 创建密钥库（如果需要）
keytool -genkey -v -keystore my-release-key.jks -keyalg RSA -keysize 2048 -validity 10000 -alias my-alias


# 5. 签名
"C:\Users\ThinkPad\AppData\Local\Android\Sdk\build-tools\36.1.0\apksigner.bat" sign --ks my-release-key.jks --ks-key-alias my-alias --out basic_signed.apk app-release-aligned.apk
"C:\Users\Lenovo\AppData\Local\Android\Sdk\build-tools\36.1.0\apksigner.bat"  sign --ks my-key.keystore --ks-key-alias my-alias --out  wujiaxinnew.apk wujiaxinnew2.apk


检查是否签名成功
"C:\Users\ThinkPad\AppData\Local\Android\Sdk\build-tools\36.1.0\apksigner.bat" verify -v mb_bank_signed.apk
```

完成以上步骤后，`gz-aligned.apk` 就是可以安装的APK文件。
