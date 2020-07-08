## Sonarqube Configuration 

The SonarScanner for Gradle provides an easy way to start SonarQube analysis of a Gradle project.

The ability to execute the SonarQube analysis via a regular Gradle task makes it available anywhere Gradle is available (developer build, CI server, etc.), without the need to manually download, setup, and maintain a SonarQube Runner installation. The Gradle build already has much of the information needed for SonarQube to successfully analyze a project. By preconfiguring the analysis based on that information, the need for manual configuration is reduced significantly.

 - [Gradle](#Gradle)
   - [For Single Module](#To-Configure-the-Scanner-Properties-for-Single-Module-Gradle)
   - [For Multi Module](#To-Configure-Multi-Module-Project)

 - [Maven](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-maven/)




# Gradle

## Prerequisites
   - Gradle versions 3+
   - At least the minimal version of Java supported by your SonarQube server is in use,`SonarQube` scanners require version `8` or `11` of the `JVM` and the SonarQube server requires version `11`


## Analyzing
To analyze first, include the scanner in your build in build.gradle:

### Using the plugin DSL (above gradle version 5)

```
plugins {
  id "org.sonarqube" version "3.0"
}
```

### Using legacy plugin application (below gradle version 5) 
```
buildscript {
    repositories {
        jcenter()
    }

    dependencies {
    
        classpath "org.sonarsource.scanner.gradle:sonarqube-gradle-plugin:3.0"
    }
}

apply plugin: "org.sonarqube"

```

## To Configure the Scanner Properties for Single Module Gradle.
  
  set the sonarqube properties in build file, as below.

```
 sonarqube {

    properties {

        property "sonar.projectKey", "fullmetrics"  //your project-key
        property "sonar.projectName", "fullmetrics" //your project-name
 
        property "sonar.host.url", "https://sonar.anywhere.co"   
        property "sonar.token", "<your token>"  // You can create project and generate token here https://sonar.anywhere.co

        // for checkstyle reportspath
        property "sonar.java.checkstyle.reportPaths", "build/reports/checkstyle/checkstyle-main.xml"

        // for pmd reportspath
        property "sonar.java.pmd.reportPaths", "build/reports/pmd/pmd-main.xml"

        // for findbugs reportspath
        property "sonar.java.findbugs.reportPaths", "build/reports/findbugs/findbugs-main.xml"

        // for junit reportspath
        property "sonar.junit.reportPaths", "build/reports/tests/test"

    }
}

```
[For more info](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-gradle/)

## To Configure Multi-Module Project.

```
// build.gradle

sonarqube {

    properties {

        property "sonar.projectKey", "fullmetrics"  //your project-key
        property "sonar.projectName", "fullmetrics" //your project-name
 
        property "sonar.host.url", "https://sonar.anywhere.co"   
        property "sonar.token", "<your token>"  // You can create project and generate token here https://sonar.anywhere.co
        
    }
}

//  Put sonarqueb properties within subprojects block, which applies for each module automatically.
subprojects {

    sonarqube {

        properties {

            property "sonar.sources", "src" 
            
            // for checkstyle reportspath
            property "sonar.java.checkstyle.reportPaths", "build/reports/checkstyle/checkstyle-main.xml"

            // for pmd reportspath
            property "sonar.java.pmd.reportPaths", "build/reports/pmd/pmd-main.xml"

            // for findbugs reportspath
            property "sonar.java.findbugs.reportPaths", "build/reports/findbugs/findbugs-main.xml"

            // for junit reportspath
            property "sonar.junit.reportPaths", "build/reports/tests/test"

        }
    }
}

```

#### Task dependencies :paperclip:

All tasks that produce output that should be included in the SonarQube analysis need to be executed before the sonarqube task runs. Typically, these are compile tasks, test tasks, and code coverage tasks.

Starting with v3.0 of the SonarScanner for Gradle, task dependencies are no longer added automatically. Instead, the SonarScanner plugin enforces the correct order of tasks with mustRunAfter. You need to be either **manually** run the tasks that produce output before sonarqube, or you can add a dependency to the build script:


```
// build.gradle

    project.tasks["sonarqube"].dependsOn "test"

```

