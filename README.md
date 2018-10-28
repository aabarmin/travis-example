# Useful tips for all cases

## Switching between different JDK versions at Mac OS X

Add the following lines to .bashrc at your home directory:

```bash
alias j11="export JAVA_HOME=`/usr/libexec/java_home -v 11`; java -version"
alias j9="export JAVA_HOME=`/usr/libexec/java_home -v 9`; java -version"
alias j8="export JAVA_HOME=`/usr/libexec/java_home -v 1.8`; java -version"
```

And then if you execute `j8` you'll get the following result:

```bash
$ j8
java version "1.8.0_144"
Java(TM) SE Runtime Environment (build 1.8.0_144-b01)
Java HotSpot(TM) 64-Bit Server VM (build 25.144-b01, mixed mode)
```

## Generation of Java project with source directories with Gradle

Execute the following command to get initialized project:

```
$ gradle init --type java-library
```

It generates project with the following structure:

```
$ tree
.
├── README.md
├── build.gradle
├── gradle
│   └── wrapper
│       ├── gradle-wrapper.jar
│       └── gradle-wrapper.properties
├── gradlew
├── gradlew.bat
├── settings.gradle
└── src
    ├── main
    │   └── java
    │       └── Library.java
    └── test
        └── java
            └── LibraryTest.java

7 directories, 9 files
```

## Configuring checkstyle in Gradle project

To configure `checkstyle` plugin in Gradle project the following tasks need to be accomplished:

1. Add `checkstyle` plugin to `build.gradle`

```groovy
apply plugin: 'checkstyle'

checkstyle {
    toolVersion '8.14'
}
```

2. Add checkstyle configuration to `config/checkstyle/checkstyle.xml`. Configuration file could be
downloaded [here](https://github.com/checkstyle/checkstyle/tree/master/config).

3. Fix the following blocks:

3.1 4 spaces indentation

```xml
    <module name="Indentation">
        <property name="basicOffset" value="4"/>
        <property name="braceAdjustment" value="0"/>
        <property name="caseIndent" value="4"/>
        <property name="throwsIndent" value="4"/>
        <property name="lineWrappingIndentation" value="4"/>
        <property name="arrayInitIndent" value="4"/>
    </module>
```

3.2 Fail build on warning

```xml
<property name="severity" value="error"/>
```

3.3 Disabling XML reports

Add the following code to `build.gradle`

```groovy
tasks.withType(Checkstyle) {
    reports {
        xml.enabled false
    }
}
```

# Running tests with JUnit 5

First, you need Gradle version at least 4.6. The following lines should be added to `build.gradle`:

```groovy
test {
    useJUnitPlatform()
}
```

# Updating Gradle version

Add the following task to `build.gradle`

```groovy
task wrapper(type: Wrapper) {
    gradleVersion = '4.8'
}
```

And then execute the following command twice:

```bash
./gradlew wrapper --gradle-version=4.8
```

# Adding JaCoCo to Gradle project

1. Add `jacoco` plugin to `build.gradle`:

```groovy
apply plugin: 'jacoco'
```

2. Add plugin configuration to `build.gradle`:

```groovy
jacoco {
    toolVersion '0.8.2'
    reportsDir = file("$buildDir/reports/jacoco")
}
```

3. Add reporting configuration to `build.gradle`:

```groovy
jacocoTestReport {
    reports {
        xml.enabled false
        csv.enabled false
        html.enabled true
    }
}
```

4. Add coverage verification configuration to `build.gradle`:

```groovy
jacocoTestCoverageVerification {
    violationRules {
        rule {
            limit {
                minimum = 0.8
            }
        }
    }
}
```

5. Run the following task to get coverage report (`test` task should be executed prior):

```bash
./gradlew jacocoTestReport
```

6. Run the following task to verify the coverage:

```bash
./gradlew jacocoTestCoverageVerification
```