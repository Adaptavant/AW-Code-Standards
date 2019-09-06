# AW Javascript Style Guide

This style guide is a set of standards(extended from [Airbnb style guide](https://github.com/airbnb/javascript)) that define how Javascript should be written. 

It's build with [ESLint](https://eslint.org/) tool.

## Rules

[check here](https://github.com/airbnb/javascript#table-of-contents)

Note: this guide assumes you are using Babel or polyfills in your app for older browsers to support the ES6 and ES7 features.

## To Install

  [![npm version](https://badge.fury.io/js/eslint-config-aw.svg)](https://badge.fury.io/js/eslint-config-aw)  [check here.](./package/eslint-config-aw-base/README.md)

## Usage

Please refer the [link](https://eslint.org/docs/user-guide/configuring) to learn more about ESLint configuration and [extending](https://eslint.org/docs/user-guide/configuring#extending-configuration-files).

We recommend using .eslintrc file, which helps us to extend multiple configs, in case of if we have both **ES5** and **ES6** code base. [For more details](https://eslint.org/docs/developer-guide/shareable-configs#using-a-shareable-config)

Eg. folder Structure: 

    ├── .app                  
      ├── v1                        # Source files 
        ├── legacy                  # Legacy files 
          ├── code.js               # Old code base
          ├── .eslintrc             # Configuration file 
        ├── latest                  
          ├── code.js               # New code base
          ├── .eslintrc             # Configuration file

Eg .eslintrc 
```json
{
    "extends": "aw"
}
```

# Step 1:

## We export two sets of ESLint configuration for your usage.

 - [Stage 1](#stage-1) - **Which is intended to show only warnings**.
   -  [Legacy](#stage-1-legacy)
   -  [Default](#stage-1-default)
 - [Default]()   
   -  [Legacy](#legacy)


[Auto Fix](#fixing-problems)

## Stage 1

  **Stage 1** is meant for migration propose and especially for our legacy codebase to migrate gradually into our new code standards. Which will not stop your build process even if it has any errors. We **highly recommend** to use **[default](#default)** for any **new module/project/feature**.
 
### Stage 1 Legacy

  Legacy configuration for **ES5** and **JQuery** code base and it will only show warnings.

    Add "extends": "aw/stage-1/legacy" to your .eslintrc.

### Stage 1 Default
  The stage 1 default configuration for **ES6** code base and it will only show warnings.

    Add "extends": "aw/stage-1" to your .eslintrc.

## Default

**Default** is intended for those who migrated and especially for our any new codebase.

 The default configuration for **ES6** code base and it will throw an error for any violation.

    Add "extends": "aw" to your .eslintrc.

### Default Legacy

  Legacy configuration for **ES5** and **JQuery** code base and it will also throw an error for any violation.

    Add "extends": "aw/legacy" to your .eslintrc.


# Step 2:

  Ways to run ESlint.

### NPM script

Add script in your package.json.

      Add "lint" : "eslint js/*.js"

### Fixing problems: 

       --fix                          Automatically fix problems
       --fix-dry-run                  Automatically fix problems without saving the changes to the file system
       --fix-type Array               Specify the types of fixes to apply (problem, suggestion, layout)

For more available options in ESlint: [check here](https://eslint.org/docs/user-guide/command-line-interface#options)


eg: 

```json
{
  "name": "BigQuery",
  "version": "1.0.0",
  "description": "To generate BigQuery sample data ",
  "main": "index.js",
  "scripts": {
    "start": "node index.js",
    "test": "echo \"Error: no test specified\" && exit 1",
    "lint" : "eslint js/*.js --fix-dry-run"
  },
  "author": "bala-raj",
  "license": "ISC"
}
```

**Note:** Please pass the argument based on your use case and **we hope you understand what you are doing when you are using auto fix**, the reason this will do changes in source files so be cautious.

then,  
 
 ```sh
  npm run lint 
 ```

Note: Will update soon to hook along with webpack and gradle, Also React.js spec.


## Recommended Editor [VS Code](https://code.visualstudio.com/download)

### Extensions for faster development.  
  - [ESLint](https://marketplace.visualstudio.com/items?itemName=dbaeumer.vscode-eslint)
  - [Code Spell Checker ](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker)
  - [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
  - [Import Cost](https://marketplace.visualstudio.com/items?itemName=wix.vscode-import-cost)
  - [npm Intellisense](https://marketplace.visualstudio.com/items?itemName=christian-kohler.npm-intellisense)
  - [JS Refactor](https://marketplace.visualstudio.com/items?itemName=cmstead.jsrefactor)
  - [ES6 Mocha Snippets](https://marketplace.visualstudio.com/items?itemName=spoonscen.es6-mocha-snippets)
  - [ES7 React/Redux/GraphQL/React-Native snippets](https://marketplace.visualstudio.com/items?itemName=dsznajder.es7-react-js-snippets)
  - [Git History](https://marketplace.visualstudio.com/items?itemName=donjayamanne.githistory)
