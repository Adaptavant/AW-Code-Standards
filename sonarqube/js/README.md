
## NPM module to run SonarQube/SonarCloud analyses

`sonarqube-scanner` makes it very easy to trigger SonarQube / SonarCloud analyses on a JavaScript code base, without needing to install any specific tool or (Java) runtime.


## Installation :computer:

This package is available on npm as: `sonarqube-scanner`
To add code analysis to your build files, simply add the package to your project dev dependencies:

```cmd 
npm install -D sonarqube-scanner 
```

To install the scanner globally and be able to run analyses on the command line:

```cmd 
npm install -g sonarqube-scanner 
```


### JSON Configuration 

```json

 // script to generate report 
 "test:report": "jest --coverage",

 // script to generate report
 "lint:report": "eslint ./server ./spec --fix -o ./eslint-report.json",

 // script to scan and publish the reports. 
 "scan": "npm run test:report && npm run lint:report && node sonarqube.js"  // we need to create sonarqube.js file 
 or 
 // Just to run only scan without test and lint report. 
 "scan": "node sonarqube.js"  // we need to create sonarqube.js file 

 ```

 **sonarqube.js**   :page_facing_up:

 ```js
   const scanner = require('sonarqube-scanner');

   scanner(
   {
     serverUrl: 'https://sonar.anywhere.co',  // hosted url for sonar 
     token: '<<your project token >>',  // your project token
     options: {
     'sonar.projectKey': 'aw-hours',  // your project key 
     'sonar.projectName': 'Anywhere-Hours', // your project name 
     'sonar.sources': 'server',
     'sonar.tests': 'spec',
     'sonar.test.inclusions': 'spec/**/*.test.jsx,src/**/*.spec.jsx,src/**/*.test.js,src/**/*.test.jsx', 
     'sonar.testExecutionReportPaths': './test-report.xml',
     //'sonar.eslint.reportPaths': './eslint-report.json', //if your are using eslint reports then add or else ignore this.
     },
   },

  () => process.exit(),
);

```


### Using npm scan script can push your reports : :racehorse:

```cmd:white_check_mark:
 npm run scan
```

Note: Don't forgot add auto generated files in .gitignore

#### If you are using jest :point_down:
[Please refer the docs:jest-sonar-reporter](https://www.npmjs.com/package/jest-sonar-reporter)


