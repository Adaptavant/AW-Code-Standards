# AW Javascript Style Guide

This style guide is a set of standards(extended from [Airbnb style guide](https://github.com/airbnb/javascript)) that define how Javascript should be written. Build with [ESLint](https://eslint.org/).

## Install

To Install the correct versions of each package, which are listed by the command:

  ```sh
  npx install-peerdeps --dev eslint-config-aw@latest
  ```

  If using **yarn**, you can also use the shortcut described above if you have npm 5+ installed on your machine, as the command will detect that you are using yarn and will act accordingly.
  Otherwise, run `npm info "eslint-config-aw@latest" peerDependencies` to list the peer dependencies and versions, then run `yarn add --dev <dependency>@<version>` for each listed peer dependency.


  If using **npm < 5**, Linux/OSX users can run

  ```sh
  (
    export PKG=eslint-config-aw;
    npm info "$PKG@latest" peerDependencies --json | command sed 's/[\{\},]//g ; s/: /@/g' | xargs npm install --save-dev "$PKG@latest"
  )
  ```
  Which produces and runs a command like:

  ```sh
    npm install --save-dev eslint-config-aw eslint@^#.#.# eslint-plugin-import@^#.#.#
  ```

  If using **npm < 5**, Windows users can either install all the peer dependencies manually, or use the [install-peerdeps](https://github.com/nathanhleung/install-peerdeps) cli tool.

  ```sh
  npm install -g install-peerdeps
  install-peerdeps --dev eslint-config-aw
  ```

  The cli will produce and run a command like:

  ```sh
  npm install --save-dev eslint-config-aw eslint@^#.#.# eslint-plugin-import@^#.#.#
  ```

## Usage

Please refer the [link](https://eslint.org/docs/user-guide/configuring) to learn more about ESLint configuration and [extending](https://eslint.org/docs/user-guide/configuring#extending-configuration-files).

We recommend to use .eslintrc file, which helps us to extend multiple config incase of if have both es5 and es6 code base. [For more details](https://eslint.org/docs/developer-guide/shareable-configs#using-a-shareable-config)

Eg. folder Structure: 

    ├── .app                  
      ├── v1                  # Source files 
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

## Step 1:

### We export two set of ESLint configuration for you usage.

 - [Stage 1](#stage-1) - **Which will show warnings**.
   -  [Legacy](#stage-1-legacy)
   -  [Default](#stage-1-default)
 - [Default]()   
   -  [Legacy](#legacy)
   -  [Default](#default)
 
### Stage 1 Legacy

  Legacy configuration for ES5 and JQuery code base and it will only show warnings.

    Add "extends": "aw/stage-1/legacy" to your .eslintrc.

### Stage 1 Default
  Default configuration for ES6 code base and it will only show warnings.

    Add "extends": "aw/stage-1" to your .eslintrc.

## Step 2:

  Ways To run eslint.

### NPM script package.json

      Add "lint" : "eslint js/*.js"

and 
 
 ```sh
  npm run lint 
 ```

