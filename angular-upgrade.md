INTRODUCTION
Purpose

For the reader to gain some insights on what to expect and how to deal with certain issues arise while upgrading. (This is not a step by step instruction for upgrading Angular)

ANGULAR UPGRADE
Do one major version upgrade at a time. (Say, don't do the upgrade of version 6 to 10 in a single go)


Link to Guide

Use this link to know the changes that needs to be done from version to version: https://update.angular.io/


What can be expected

During the upgrade process, the following happens:

* Packages gets upgraded

* New configuration files gets created

* The source codes also gets migrated to adapt to the new version

UPGRADING PACKAGES
When you upgrade the ng, the other packages you have installed also need to be upgraded (otherwise it will throw dependency error).


Deprecation

Watch out for deprecated packages(unmaintained packages). The corresponding coding implementation might have to be changed/rewritten/amended to keep that feature in the project working.



Unused/Missing Dependencies

Install depcheck or npx globally:  npm install -g npx or npm install -g depcheck

Run depcheck or npx to get the dependency analysis: depcheck or npx depcheck


Code Fixing

Compile/build/auto-unit-test the code and fix the errors thrown.


NGCC

Add '"postinstall": "ngcc", to the scripts in the package.json. This is the angular compatibility compiler which is executed by the cli commands as needed.


Outdated Versions

Run npm outdated to find the outdated packages and update them.


Check for Vulnerability

Run npm audit to find out ways to fix the vulnerabilities (You may auto fix it by running npm audit fix)


Minor Updates

Run ng update to make sure the application is uptodate.

Run npm update to make sure the packages are uptodate. (Make careful use of ^ and ~ symbols in the package.json)


Workaround

Use --allow-dirty to allow uncommitted migration or --create-commits to create a git commit for individual migration.

Use --force to force the update.



Example: ng update ** --allow-dirty --force

TROUBLESHOOTING
Need to set some configurations in order to have a smooth transition. This may differ based on the need for the application.



When going for upgrade you might required to add this in tsconfig.app.json

tsconfig.app.json
"compilerOptions": {
  ...
  "allowSyntheticDefaultImports": true, // When using default imports
  "resolveJsonModule": true // For json data import
},


If you use automated test, you may have to include the below in tsconfig.spec.json (say, 'mixpanel-browser' js package needs it)

tsconfig.spec.json
"compilerOptions": {
  ...
  "esModuleInterop": true,
  "allowJs": true // if you want to allow JS package
},


If you are using common js dependencies you can add the below option in angular.json file:

angular.json
"builder": "@angular-devkit/build-angular:browser",
"options": {
  "allowedCommonJsDependencies": [
    "<module-name>"
  ]
  ...
}


Upgrade Angular in NX Mono Repository
To upgrade the Angular packages related to the NX mono repository, the following steps could be followed.

1. Run npm run nx migrate latest (This may take some time. This command also upgrades the package.json)

2. a. Run npm install (Execute this command only after verifying the changes in package.json)

    b. Run npm run nx migrate --run-migrations=migrations.json (If this command don't work properly, put this command as script in the package.json and then execute it)

    c. Remove migrations.json file

or

2. Run npm install --legacy-peer-deps (If there is no migration file created, this command can be executed directly)

3. Run npm outdated to check for other packages to be updated.

