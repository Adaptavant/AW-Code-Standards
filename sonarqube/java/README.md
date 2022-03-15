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
To analyze, include the scanner plugin in buildscript:

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
### Workflow file Configuration
 To configure the sonarqube scanner for pull request in Github actions or to run in local feature branch, you can add the following configuration.
   open `.github/workflows/{actions.yml}` and paste the environment variables given below under jobs -> env in `.yml` file.

```cmd
env:
   BASE_BRANCH: ${{github.event.pull_request.base.ref}}
   HEAD_BRANCH: ${{github.event.pull_request.head.ref}}
   PR_ID: ${{github.event.number}}
   IS_PULL_REQUEST_MERGED: ${{github.event.pull_request.merged}}
```      

 - BASE_BRANCH is the name of the branch that the PR is targeting.
 - HEAD_BRANCH is the name of the branch that the PR is currently on. 
 - PR_ID is the ID of the PR.
 - IS_PULL_REQUEST_MERGED is a boolean value that is true if the PR is merged.
  set the sonarqube properties in build file, as below.

```

 static gitBranch() {
    def branch = ""
    def proc = "git rev-parse --abbrev-ref HEAD".execute()
    proc.in.eachLine { line -> branch = line }
    proc.err.eachLine { line -> println line }
    proc.waitFor()
    branch
 }

 sonarqube {

    properties {

        property "sonar.projectKey", "fullmetrics"  //your project-key
        property "sonar.projectName", "fullmetrics" //your project-name
 
        property "sonar.host.url", "https://sonar.anywhere.co"   
        property "sonar.login", "<your token>"  // You can create project and generate token here https://sonar.anywhere.co

        // for checkstyle reportspath
        property "sonar.java.checkstyle.reportPaths", "build/reports/checkstyle/checkstyle-main.xml"

        // for pmd reportspath
        property "sonar.java.pmd.reportPaths", "build/reports/pmd/pmd-main.xml"

        // for findbugs reportspath
        property "sonar.java.findbugs.reportPaths", "build/reports/findbugs/findbugs-main.xml"

        // for junit reportspath
        property "sonar.junit.reportPaths", "build/reports/tests/test"

        // for jacoco code coverage reportpath
        property "sonar.coverage.jacoco.xmlReportPaths", "./build/reports/jacoco/test/jacocoTestReport.xml"

        if (System.getenv("CI")){
          if (System.getenv("IS_PULL_REQUEST_MERGED").equals("true")) {
		    property "sonar.branch.name", System.getenv("BASE_BRANCH")
	      } else {
            property "sonar.pullrequest.key", System.getenv("PR_ID")
            property "sonar.pullrequest.base", System.getenv("BASE_BRANCH")
            property "sonar.pullrequest.branch", System.getenv("HEAD_BRANCH")
          }
        } else {
            def currentBranch = gitBranch()
            property "sonar.branch.name", currentBranch
        }
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
        property "sonar.login", "<your token>"  // You can create project and generate token here https://sonar.anywhere.co
        
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

Note: Don't forget to add auto-generated files in .gitignore
