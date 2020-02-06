# 工具集

## APKTOOL

><https://ibotpeaches.github.io/Apktool/>

## jarsigner （给APK签名）

```shell
jarsigner -verbose -keystore xxx.jks/xxx.keystore -signedjar result.apk source.apk aliasName
```

eg:
xxx.jks/xxx.keystore 签名文件
aliasName 签名的别名

## 查看apk代码

1. 用解压工具直接解压apk文件
2. 用dex2jar处理dex文件得到jar文件
3. 用JD_GUI查看jar文件
