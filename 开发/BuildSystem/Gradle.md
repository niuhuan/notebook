Gradle
======

## 阿里云镜像

https://help.aliyun.com/document_detail/131465.html

https://maven.aliyun.com/repository/central
https://maven.aliyun.com/repository/jcenter
https://maven.aliyun.com/repository/google

## 设置代理

在Gradle中build.gradle文件中group后面配置如下：

```
System.setProperty("socksProxyHost","127.0.0.1")
System.setProperty("socksProxyPort","1081")
```

修改GRADLE_HOME下的gradle.properties文件 (重启)
```
org.gradle.jvmargs=-Xmx2048m -XX:MaxPermSize=512m -XX:+HeapDumpOnOutOfMemoryError -Dfile.encoding=UTF-8 -DsocksProxyHost=127.0.0.1 -DsocksProxyPort=1081
```

## BuildBootJar


```kotlin
tasks.withType<Jar>() {
    manifest.attributes["Main-Class"] = "xxx.MoocApplicationKt"
    configurations["compileClasspath"].forEach { file: File ->
        from(zipTree(file.absoluteFile))
    }
}
```

## 排除子依赖

build.gradle
```
    implementation('org.telegram:telegrambots-abilities:6.7.0', {
        exclude group: 'org.mapdb', module: 'mapdb'
    })
```

## 依赖子模块

settings.gradle
```
include ':mapdb'
```

build.gradle
```
implementation project(':mapdb')
```

## 代理

PS: 试过各种方法，只有这个好用

```
./gradlew -Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=1087 -Dhttps.proxyHost=127.0.0.1 -Dhttps.proxyPort=1087 installRelease
```

