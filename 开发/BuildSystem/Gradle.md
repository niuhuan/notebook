Gradle
======

## 阿里云镜像

https://help.aliyun.com/document_detail/131465.html

```
https://maven.aliyun.com/repository/central
https://maven.aliyun.com/repository/jcenter
https://maven.aliyun.com/repository/google
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

PS: macos 试过各种方法，只有这个好用

```
./gradlew -Dhttp.proxyHost=127.0.0.1 -Dhttp.proxyPort=1087 -Dhttps.proxyHost=127.0.0.1 -Dhttps.proxyPort=1087 installRelease
```

PS: windows下上面的方式不好用
反而改配置文件好用
`.gradle ❯ code gradle.properties`
```
systemProp.http.proxyHost=your_proxy_host
systemProp.http.proxyPort=your_proxy_port
systemProp.http.proxyUser=your_proxy_user
systemProp.http.proxyPassword=your_proxy_password
systemProp.https.proxyHost=your_proxy_host
systemProp.https.proxyPort=your_proxy_port
systemProp.https.proxyUser=your_proxy_user
systemProp.https.proxyPassword=your_proxy_password
systemProp.http.nonProxyHosts=*.nonproxyrepos.com|localhost
systemProp.https.nonProxyHosts=*.nonproxyrepos.com|localhost
```

