# atom-lightning-bolt

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
* Collect or update Matching Regression Test suite from [Atom.Api.TestSuites] (https://github.com/KaplanTestPrep/Atom.Api.TestSuites).
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
## To Run Test
```
npm install -g jasmine-node
npm install -g @abot/atom-lightning-bolt
atom-lightning-bolt --config folder :regressionSuitePath --config testEnv :targetTestEnv
```
