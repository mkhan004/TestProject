# atom-lightning-bolt

[![NPM](https://nodei.co/npm/practicekhan.png)](https://nodei.co/npm/practicekhan/)
[![Build
Status](https://travis-ci.org/vlucas/frisby.png?branch=master)](https://travis-ci.org/vlucas/frisby)

## What is this?
atom-lightning-bolt is a jasmine-node based API test framework using Frisby.

## Features
* Support custom request in YAML file.
* Ability to process miltiple request files.
* Can validate Headers, Response Code and Content (Positive and Negative).
* Supports command line switch of Test Environment (default QA).
* Can write and delete Test Data in s3 Bucket.
* Outputs responses, body, errors
* Supports JWTs

## Regression Suite Folder Structure

    .
    ├── ...
    ├── config                            # Config files for different test env.
    │   ├── qa.yaml                       # Config for QA test env.
    │   ├── stg.yaml                      # Config for Staging test env.
    │   ├── prod.yaml                     # Config for Production test env.
    │   ├── gateway-qa.yaml               # Config for Gateway QA test env.
    │   ├── gateway-stg.yaml              # Config for Gateway Staging test env.
    │   ├── gateway-prod.yaml             # Config for Gateway Production test env.
    │   └── ...
    ├── data                              # Test data files for s3 bucket [optional]
    │   └── ...
    ├── doc                               # Files contain data for auto generated Readme.md file
    │   ├── setup.yaml                    # Contain Business Rules and Run command
    │   └── ...
    ├── request                           # Request files (YAML) for API cals
    │   ├── requestOne.yaml               # YAML file contains api request parameters (id, method, path, profile...)
    │   ├── requestTwo.yaml               # YAML file contains api request parameters (id, method, path, profile...)
    │   └── ...
    ├── s3config                          # Contains s3 bucket config files, required for Test Data Write
    │   ├── default.yaml                  # s3 bucket config for default env (QA)
    │   ├── qa.yaml                       # s3 bucket config for QA env
    │   ├── stg.yaml                      # s3 bucket config for STG env
    │   ├── prod.yaml                     # s3 bucket config for PROD env
    │   └── ...
    ├── schema                            # Contains JSON schema files, used for response validation
    │   ├── schema.json
    │   └── ...
    ├── profile                           # Contains JSON user-profile files, used for JWT creation
    │   ├── user-profile.json
    │   └── ...
    ├── plugins                           # Contains plugins JS files, used for custom response validation
    │   ├── plugins.js
    │   └── ...
    └── ...
    

## Supported API
* ProducConfig
* Jasper

## Integration & Setup Instructions for Supported API
* Collect latest Matching Regression Test suite from [Atom.Api.TestSuites] (https://github.com/KaplanTestPrep/Atom.Api.TestSuites).
* For First Time Setup
    * Do `npm login` using `abot` credentials.
    * Locally install `@abot/atom-lightning-bolt` and save as devDependency.
    ```
    npm install @abot/atom-lightning-bolt --save
    ```
    * Add following Script argument in package.json
    ```
    "scripts": {
        "test": "./node_modules/.bin/atom-lightning-bolt . --config folder :regressionSuitePath --config testEnv :targetTestEnv"
    }
    ```
* For Existing Setup
    * Just run `npm install`
* To Execute Test `npm test`.

## Run Test Locally From [Atom.Api.TestSuites] (https://github.com/KaplanTestPrep/Atom.Api.TestSuites)
* Clone [Atom.Api.TestSuites] (https://github.com/KaplanTestPrep/Atom.Api.TestSuites)
* `cd` to [Atom.Api.TestSuites] (https://github.com/KaplanTestPrep/Atom.Api.TestSuites)
* To run test using `npm`
    * Update package.json Script's `test` argument value for target API
    ```
    "scripts": {
        "test": "./node_modules/.bin/atom-lightning-bolt . --config folder :regressionSuitePath --config testEnv :targetTestEnv"
    }
    ```
    * To run test
    ```
    npm install -g jasmine-node
    npm install
    npm test
    ```
* To run test from Command Line:
    * To run test
    ```
    npm install -g jasmine-node
    npm install
    ./node_modules/.bin/atom-lightning-bolt . --config folder :regressionSuitePath --config testEnv :targetTestEnv
    ```

## TeamCity Setup
There is two steps in setup and execute test in TeamCity. In `first step`: We need to create a TeamCity project for `atom-lightning-bolt` then setup build step to create a docker image. In `second step`: Create Build Configuration for each test environment for any new API test.

### Step 1: Docker Image Build
* Create a TeamCity project for `atom-lightning-bolt` using this [Atom.Api.TestSuites] (https://github.com/KaplanTestPrep/Atom.Api.TestSuites) repo.
* Create a `Command Line` Build Step using following command:
```
docker build -t lightningbolt .
```

### Step 2: Create Build Configuration
* Create a new Build Configuration in `atom-lightning-bolt` project for target API and Test Environment.
* Build Configuration Naming Convention
    * `:regressionSuiteName :targetTestEnv`
    * Example:
        * `productConfig gateway-qa`
        * `productConfig qa`
* Create a `Command Line` Build Step using following command:
```
docker run -v $PWD/testOutput:/root/atom.api.testsuites/testOutput -e testFolder={regressionSuiteName} -e environment={targetTestEnv} lightningbolt
```
## Run Test directly from `atom-lightning-bolt`
* Clone this [atom-lightning-bolt] (https://github.com/KaplanTestPrep/atom-lightning-bolt) repo.
* `cd` within `atom-lightning-bolt`.
* To Run Test
```
npm install -g jasmine-node
npm install
npm link
atom-lightning-bolt . --config folder :regressionSuitePath --config testEnv :targetTestEnv
```


## bolt-data-writer
If you want to publish fresh copy of your Test Data in s3 cucket then first you need to `delete` existing Test Data then `write` new data.
<b>Note:</b> You may need to wait untill default caching time is over to get new data in API response.

### To Write Test Data
```
bolt-data-writer :regressionSuitePath :env write
```
### To Delete Test Data
```
bolt-data-writer :regressionSuitePath :env delete
```
