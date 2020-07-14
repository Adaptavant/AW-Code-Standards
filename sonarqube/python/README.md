
## Python module to run SonarQube/SonarCloud analyses

`SonarScanner` The SonarScanner is the scanner to use when there is no specific scanner for your build system.
`sonar-scanner` Launcher to analyze a project with SonarQube


## Installation :computer:

This package is available on brew as: `sonar-scanner`
To add code analysis to your build files, simply add the package to your project dev dependencies:

```cmd 
brew install sonar-scanner
```

### Makefile Configuration 

```python
scan:
    sonar-scanner 
 ```
 **sonar-project.properties**   :page_facing_up:
Add this file to the root folder
 ```python
    sonar.projectKey=python // your project key 
    sonar.projectName=python // your project name 
    sonar.sources=dir,dir2
    sonar.login=token
    sonar.language=py
    sonar.inclusions=**/*.py
    sonar.python.pylint=/usr/local/bin/pylint 
    sonar.python.coverage.reportPaths=coverage.xml
```

### Command Line
```CLI

sonar-scanner -Dsonar.projectKey=python -Dsonar.sources=. -Dsonar.host.url=http://localhost:9000 -Dsonar.login=token
 ```

### Using make scan command can push your reports : :racehorse:

```cmd:white_check_mark:
 make scan
```

Note: Don't forget to add auto-generated files in .gitignore




