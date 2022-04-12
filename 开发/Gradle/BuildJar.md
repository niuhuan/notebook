
```kotlin
tasks.withType<Jar>() {
    manifest.attributes["Main-Class"] = "xxx.MoocApplicationKt"
    configurations["compileClasspath"].forEach { file: File ->
        from(zipTree(file.absoluteFile))
    }
}
```