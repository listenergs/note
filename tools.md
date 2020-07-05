# 工具集

## APK 签名

### 签名工具

1. jarsigner 由 JDK 提供的针对 jar 包签名的通用工具，位于 JDK/bin/ 目录；
2. apksigner 由 Google 官方提供的针对 Android apk 签名及验证的专用工具，位于 Android SDK/build-tools/SDK 版本。

### 两种签名方案 V1、V2

1. V1 签名：来自 JDK（jarsigner） ，对 zip 压缩包的每个文件进行验证, 签名后还能对压缩包修改(移动/重新压缩文件)，对 V1 签名的 apk/jar 解压，在 META-INF 存放签名文件（MANIFEST.MF、CERT.SF、CERT.RSA），其中 MANIFEST.MF 文件保存有文件的 SHA1 指纹(除了META-INF文件), 由此可知： V1 签名是对压缩包中单个文件签名验证。
2. V2签名：来自 Google（apksigner） ，对 zip 压缩包的整个文件验证，签名后不能修改压缩包（包括 zipalign ），对 V2 签名的 apk 解压，没有发现签名文件，重新压缩后 V2 签名就失效，由此可知： V2 签名是对整个 APK 签名验证。

Android 7.0以前版本, 只能用旧签名方案 V1 scheme (JAR signing)；从 Android 7.0 开始, 谷歌增加新签名方案 V2 Scheme（APK Signature）；apksigner 工具默认同时使用 V1 和 V2 签名,以兼容 Android 7.0 以下版本。

### 签名步骤

1. 生成密钥对

    ``` shell
    keytool -genkeypair -keystore 密钥库名.jks -alias 密钥别名 -validity 天数 -keyalg RSA -storetype jks
    参数:
        -genkeypair    生成一条密钥对(由私钥和公钥组成)
        -keystore           密钥库名字以及存储位置(默认当前目录)
        -alias                  密钥对的别名(密钥库可以存在多个密钥对,用于区分不同密钥对)
        -validity           密钥对的有效期(单位: 天)
        -keyalg             生成密钥对的算法(常用RSA/DSA,DSA只用于签名,默认采用DSA)
        -storetype      密钥库类型(如果不指定，则默认jks类型，还支持 pkcs12 等)
        -delete             删除一条密钥
    TIPS：可重复使用此条命令，在同一密钥库中创建多条密钥，每次执行只需修改密钥别名就行。
    ```

2. 查看密钥库

    ``` shell
    keytool -list -v -keystore 密钥库名
    参数:
        -list 查看密钥列表
        -v    查看密钥详情
    ```

3. 签名
   1. jarsigner （只支持 v1）

        ```shell
        jarsigner -keystore 密钥库 -signedjar 签名后.apk 待签名.apk 密钥别名
        TIPS：只支持 API 4.2 以上
        ```

        ``` shell
        jarsigner -keystore 密钥库 -digestalg SHA1 -sigalg SHA1withRSA  -signedjar signed.apk unsigned.apk 密钥别名
        TIPS：兼容 API 4.2 以前
        ```

   1. apksigner （默认同时使用 v1、v2）

        ```shell
        apksigner sign --ks 密钥库 --ks-key-alias 密钥别名 -in unsigned.apk  --out signed.apk
        ```

4. 签名验证
   1. keytool （只支持 v1 签名）

        ```shell
            keytool -printcert -jarfile signed.apk
        ```

   2. apksigner （支持 v1、v2）

        ```shell
            apksigner verify -v --print-certs signed.apk
        ```

## [APKTOOL](https://ibotpeaches.github.io/Apktool/)

eg:
xxx.jks/xxx.keystore 签名文件
aliasName 签名的别名

## 查看apk代码

1. 用解压工具直接解压apk文件
2. 用dex2jar处理dex文件得到jar文件
3. 用JD_GUI查看jar文件
