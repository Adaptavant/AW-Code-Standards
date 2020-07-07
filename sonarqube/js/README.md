
## jest-sonar-reporter

jest-sonar-reporter is a custom results processor for Jest. The processor converts Jest's output into Sonar's generic test data format.


## Installation 

Using npm:

``` $ npm i -D jest-sonar-reporter ```

Using yarn:

``` $ yarn add -D jest-sonar-reporter ```


### JSON Configurtion 

``` //json

 "test:report": "jest --coverage",

 "lint:report": "eslint ./server ./spec --fix -o ./eslint-report.json",

 "scan": "npm run test:report && npm run lint:report && node sonarqube.js"  // we need to create sonarqube.js file 

 ```

 **sonarqube.js** file 

 ```
   const scanner = require('sonarqube-scanner');

   scanner(
   {
     serverUrl: 'https://sonar.anywhere.co',
     token: '<<your project token >>',
     options: {
     'sonar.projectKey': 'aw-hours',
     'sonar.projectName': 'Anywhere-Hours',
     'sonar.sources': 'server',
     'sonar.tests': 'spec',
     'sonar.test.inclusions': 'spec/**/*.test.jsx,src/**/*.spec.jsx,src/**/*.test.js,src/**/*.test.jsx', 
     'sonar.testExecutionReportPaths': './test-report.xml',
     'sonar.eslint.reportPaths': './eslint-report.json',
     },
   },

  () => process.exit(),
);

```


#### Usage 

 - Run Jest to execute your tests.

Using npm:

``` $ npm run test ```

Using yarn:

``` $ yarn run test ```

 - Run sonar-scanner to import the test results.

``` $ sonar-scanner ```



[For more details please refer the docs:jest-sonar-reporter](https://www.npmjs.com/package/jest-sonar-reporter)


