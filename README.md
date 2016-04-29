#测试批量签名打包apk

**后面都是对app/build.gradle进行修改**

**1. 在manifest加入渠道名称变量，编译时会写入渠道名的值**

<meta-data android:value="${CHANNEL_VALUE}" android:name="CHANNEL"/>



**2. 加入支持的渠道**

可以定义某个渠道的applicationId，也就是说，在不修改源码的包名applicationId情况下，可以在这里打包定义不同的applicationId。


```java

productFlavors {

    myapk{

    //指定一些参数

    signingConfig signingConfigs.release

    minSdkVersion 15

    applicationId 'com.qq.test'

    targetSdkVersion 21

}

    xiaomi {}

    yingyongbao {}

    baidu {}

    wandoujia {}

}



productFlavors.all {

    flavor -> flavor.manifestPlaceholders = [CHANNEL_VALUE: name]

}

```


**3. 定义签名**

```java

signingConfigs {

debug {

storeFile file(project.SIGNINGCONFIGS_STORE_FILE)

storePassword project.SIGNINGCONFIGS_STORE_PASSWORD

keyAlias project.SIGNINGCONFIGS_KEY_ALIAS

keyPassword project.SIGNINGCONFIGS_KEY_PASSWORD

}

release {

storeFile file(project.SIGNINGCONFIGS_STORE_FILE)

storePassword project.SIGNINGCONFIGS_STORE_PASSWORD

keyAlias project.SIGNINGCONFIGS_KEY_ALIAS

keyPassword project.SIGNINGCONFIGS_KEY_PASSWORD

}

}
```

这里签名的密码什么的都是写在gradle.properties文件：

```java
SIGNINGCONFIGS_STORE_FILE=mytest.jks

SIGNINGCONFIGS_STORE_PASSWORD=123456

SIGNINGCONFIGS_KEY_ALIAS=mytest

SIGNINGCONFIGS_KEY_PASSWORD=123456

```

SIGNINGCONFIGS_STORE_FILE这个文件路径，这里是跟build.gradle同一个目录（也就是app目录）。


**4. 设置使用签名和代码混淆**

```java

buildTypes {

    release {

        signingConfig signingConfigs.release

        minifyEnabled true

        proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'

    }

}

```





