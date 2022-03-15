
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

### nodejs.yml file Configuration
 To configure the scanner to run on your pull request or feature branch, you can add the following configuration.
   open .github/workflows/nodejs.yml and paste the environment variables below into your `nodejs.yml` file. 

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

const options = {
	'sonar.projectKey': 'anywhere-hours',
	'sonar.projectName': 'Anywhere-hours',
	'sonar.sources': 'server',
	'sonar.tests': 'spec',
	'sonar.test.inclusions': 'spec/**/*.test.jsx,src/**/*.spec.jsx,src/**/*.test.js,src/**/*.test.jsx',
	'sonar.testExecutionReportPaths': 'test-report.xml',
	//'sonar.eslint.reportPaths': 'eslint-report.json', //if your are using eslint reports then add or else ignore this.
};

if (process.env.CI) {
	if (process.env.IS_PULL_REQUEST_MERGED === 'true') {
		options['sonar.branch.name'] = process.env.BASE_BRANCH;
	} else {
		options['sonar.pullrequest.key'] = process.env.PR_ID;
		options['sonar.pullrequest.base'] = process.env.BASE_BRANCH;
		options['sonar.pullrequest.branch'] = process.env.HEAD_BRANCH;
	}
} else {
	const getCurrentBranchName = require('node-git-current-branch');
	options['sonar.branch.name'] = getCurrentBranchName();
}

scanner({
		serverUrl: 'https://sonar.anywhere.co', // hosted url for sonar 
		token: '<<your project token >>', // your project token
		options,
	},

	() => process.exit(),
);
```


### Using npm scan script can push your reports : :racehorse:

```cmd:white_check_mark:
 npm run scan
```

Note: Don't forget to add auto-generated files in .gitignore

#### If you are using jest :point_down:
[Please refer the docs:jest-sonar-reporter](https://www.npmjs.com/package/jest-sonar-reporter)


