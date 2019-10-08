# Using Apache Spark - an example

Spark is a general-purpose distributed data processing engine that is suitable for use in a wide range of circumstances. On top of the Spark core data processing engine, there are libraries for SQL, machine learning, graph computation, and stream processing, which can be used together in an application.

The below figure shows the core architecture that `spark` provides us.

![Spark Core Components](/images/spark.svg)

## **1** - Creating a maven project

We will be coding with java and use maven as our build tool. The below code file shows the maven dependency file `pom.xml`. Just copy and paste the contents of the file into your `pom.xml` file that is generated.

Before that let's create a barebone maven project. I prefer using `Intellij IDEA Community Edition` on my Ubuntu Mate Desktop. The below series of pictures shows creating a successful maven project.

> Fig : Creating a maven project - step1
![Creating a maven project](/images/1/project1.png)

> Fig: Creating a maven project - step2
![Creating a maven project](/images/1/project2.png)

> Fig: Creating a maven project - step3
![Creating a maven project](/images/1/project3.png)

> Fig: Enable auto import to avoid manual imports everytime
![Creating a maven project](/images/1/mainproj.png)


After successful creation of project copy the below dependencies to your `pom.xml` file.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>shashank.spark</groupId>
    <artifactId>shashank.spark</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>jar</packaging>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <java.version>1.8</java.version>
        <scala.version>2.11</scala.version>
        <spark.version>2.3.1</spark.version>
        <maven.compiler.source>1.8</maven.compiler.source>
        <maven.compiler.target>1.8</maven.compiler.target>
    </properties>

    <dependencies>
        <!-- Spark -->
        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-core_${scala.version}</artifactId>
            <version>${spark.version}</version>
        </dependency>

        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-sql_${scala.version}</artifactId>
            <version>${spark.version}</version>
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-simple</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <groupId>org.apache.spark</groupId>
            <artifactId>spark-mllib_${scala.version}</artifactId>
            <version>${spark.version}</version>
            <exclusions>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-log4j12</artifactId>
                </exclusion>
                <exclusion>
                    <groupId>org.slf4j</groupId>
                    <artifactId>slf4j-simple</artifactId>
                </exclusion>
            </exclusions>
        </dependency>

        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>

    </dependencies>

    <build>

        <plugins>

            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-dependency-plugin</artifactId>
                <executions>
                    <execution>
                        <id>copy-dependencies</id>
                        <phase>prepare-package</phase>
                        <goals>
                            <goal>copy-dependencies</goal>
                        </goals>
                        <configuration>
                            <outputDirectory>
                                ${project.build.directory}/libs
                            </outputDirectory>
                        </configuration>
                    </execution>
                </executions>
            </plugin>


            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <executions>
                    <execution>
                        <goals>
                            <goal>repackage</goal>
                        </goals>
                        <configuration>

                            <mainClass>shashank.spark.Application</mainClass>

                        </configuration>
                    </execution>
                </executions>
            </plugin>

        </plugins>

    </build>

</project>
```

Maven might take some time depending on your internet connection to download the required dependencies for running spark.

Our basic structure of project might look like the one showed below.

```c
.
├── pom.xml
├── src
│   ├── main
│   │   ├── java
│   │   └── resources
│   └── test
└── target
```

We now create a main class named `Application.java` under `java` directory.
This will be the starting point of our application.

```c
.
├── pom.xml
├── src
│   ├── main
│   │   ├── java
│   │   │   └── Application.java
│   │   └── resources
│   └── test
└── target
```