## SonarScanner for Gradle

The SonarScanner for Gradle provides an easy way to start SonarQube analysis of a Gradle project. 

## Prerequisites
   - Gradle versions 3+
   - At least the minimal version of Java supported by your SonarQube server is in use 


## Configure the Scanner
Installation is automatic, but certain global properties should still be configured. A good place to configure global properties is `~/.gradle/gradle.properties` .

```
    # gradle.properties
    systemProp.sonar.host.url=http://localhost:9000
 
    #----- Token generated from an account with 'publish analysis' permission
    systemProp.sonar.login=<token>

```


## To Install

