# Useful tips for all cases

## Switching between different JDK versions at Mac OS X

Add the following lines to .bash_profile at your home directory:

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