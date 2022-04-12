Maven - 兼容Lombok和Kotlin
=========================

Maven会先delombok给kotlin用, 解决 mvn package 时 Lombok和kotlin不兼容的问题, 但是IDEA的编译过程没有使用maven, 点击图标编译仍然会提示方法不存在

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

    <modelVersion>4.0.0</modelVersion>

    <groupId>com.ericytsang</groupId>
    <artifactId>playground</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>com.ericytsang playground</name>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <kotlin.version>1.2.50</kotlin.version>
        <junit.version>4.12</junit.version>
        <lombok.version>1.16.8</lombok.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.jetbrains.kotlin</groupId>
            <artifactId>kotlin-stdlib</artifactId>
            <version>${kotlin.version}</version>
        </dependency>
        <dependency>
            <groupId>org.jetbrains.kotlin</groupId>
            <artifactId>kotlin-test-junit</artifactId>
            <version>${kotlin.version}</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <version>${lombok.version}</version>
        </dependency>
    </dependencies>

    <build>
        <plugins>

            <plugin>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok-maven-plugin</artifactId>
                <version>${lombok.version}.0</version>
                <executions>
                    <execution>
                        <id>delombok</id>
                        <phase>generate-sources</phase>
                        <goals>
                            <goal>delombok</goal>
                        </goals>
                        <configuration>
                            <sourceDirectory>src/main/java</sourceDirectory>
                            <outputDirectory>${project.build.directory}/delombok-main</outputDirectory>
                            <addOutputDirectory>false</addOutputDirectory>
                        </configuration>
                    </execution>
                    <execution>
                        <id>delombok-test</id>
                        <phase>generate-test-sources</phase>
                        <goals>
                            <goal>delombok</goal>
                        </goals>
                        <configuration>
                            <sourceDirectory>src/test/java</sourceDirectory>
                            <outputDirectory>${project.build.directory}/delombok-test</outputDirectory>
                            <addOutputDirectory>false</addOutputDirectory>
                        </configuration>
                    </execution>
                </executions>
            </plugin>

            <plugin>
                <groupId>org.jetbrains.kotlin</groupId>
                <artifactId>kotlin-maven-plugin</artifactId>
                <version>${kotlin.version}</version>
                <executions>
                    <execution>
                        <id>compile</id>
                        <phase>process-sources</phase>
                        <goals>
                            <goal>compile</goal>
                        </goals>
                        <configuration>
                            <sourceDirs>${project.build.directory}/delombok-main</sourceDirs>
                        </configuration>
                    </execution>
                    <execution>
                        <id>test-compile</id>
                        <phase>test-compile</phase>
                        <goals>
                            <goal>test-compile</goal>
                        </goals>
                        <configuration>
                            <sourceDirs>${project.build.directory}/delombok-test</sourceDirs>
                        </configuration>
                    </execution>
                </executions>
            </plugin>

        </plugins>
    </build>

</project>
```

