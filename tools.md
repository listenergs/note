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
