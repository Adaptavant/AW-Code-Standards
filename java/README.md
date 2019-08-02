
## Java Style Guide
    
#### For Code Formatting:

[Java: Google-Java-Format](https://github.com/google/google-java-format)

Note: This [plugin](https://github.com/google/google-java-format#intellij-android-studio-and-other-jetbrains-ides.) will be helpful to auto fix most of the CheckStyle warnings. **But remember this will modify every line, So I hope you aware that will lead to more conflicts for the team uses multiple git branches. Recommend to plan well before using this formattor.**

#### For Static Code Analysis:

We think this plugin covers most of our minimal expectation on code style and static code analysis.  

[https://plugins.gradle.org/plugin/com.monits.staticCodeAnalysis](https://plugins.gradle.org/plugin/com.monits.staticCodeAnalysis) 

Static Code Analysis wraps around [Checkstyle](http://checkstyle.sourceforge.net/), [Findbugs](http://findbugs.sourceforge.net/), [PMD](https://pmd.github.io/) and CPD(**Copy/Paste Detector** is an add-on to PMD), offering new features and extensions to the encapsulated plugins, making it easier to use them and providing better results with minimum effort.

**XML Configurations:**
 - Checkstyle
	 - [For src](config/v0.0.1/checkstyle/checkstyle-main.xml)
	 - [For test](config/v0.0.1/checkstyle/checkstyle-test.xml) 
 -  Findbugs
	 - [For src](config/v0.0.1/findbugs/excludeFilter-main.xml)
	 - [For test](config/v0.0.1/findbugs/excludeFilter-test.xml)
 - PMD
	- [For src](config/v0.0.1/pmd/pmd-main-pmd-6.xml)
	- [For test](config/v0.0.1/pmd/pmd-test-pmd-6.xml)

> **Note:** FindBugs is revamped as SpotBugs, so we'll be migrating in future to avail new updates.

#### `build.gradle` Gradle Config:

```groovy
buildscript {
  repositories {
    maven {
      url "https://plugins.gradle.org/m2/"
    }
  }
  dependencies {
    classpath "com.monits:static-code-analysis-plugin:2.6.10"
  }
}

apply plugin: "com.monits.staticCodeAnalysis"


tasks.withType(FindBugs) {
    reports {
        xml.enabled = false
        html.enabled = true
    }
}

staticCodeAnalysis {
    findbugsExclude = "https://raw.githubusercontent.com/Adaptavant/AW-Code-Standards/master/java/config/v0.0.1/findbugs/excludeFilter-main.xml"
    checkstyleRules = "https://raw.githubusercontent.com/Adaptavant/AW-Code-Standards/master/java/config/v0.0.1/checkstyle/checkstyle-main.xml"
    pmdRules = ["https://raw.githubusercontent.com/Adaptavant/AW-Code-Standards/master/java/config/v0.0.1/pmd/pmd-main-pmd-6.xml"]
}

```

In case of Your using Gradle 5 :

```groovy
plugins {
  id "com.monits.staticCodeAnalysis" version "2.6.10"
}
```

The plugin will add the task to run each analyser individually and also hooked to be run as part of the `check` task of the Java Plugin.

Run "gradle check" from the root of the project to run the analyzers:
```
$ gradle check
```

To execute individual plugins, type `gradle tasks --all` to see all possible tasks.  Here are examples of individual code quality tasks:

```
checkstyle
checkstyleMain - Run Checkstyle analysis for main classes
checkstyleTest - Run Checkstyle analysis for test classes

cpd

findbugs
findbugsMain - Run FindBugs analysis for main classes
findbugsTest - Run FindBugs analysis for test classes
pmd
pmdMain - Run PMD analysis for main classes
pmdTest - Run PMD analysis for test classes
```

For instance, to run only the cpd plugin, you would type:

```
$ gradle cpd   # replace cpd with any of the above tasks.
```


