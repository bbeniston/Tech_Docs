The BDD testing approach can be achieved in Angular using Cucumber configured on top of Cypress.

Technologies used:

Angular
Cypress
Cucumber
Note: The paths may differ in different projects.

Here are the steps to follow to configure Cucumber:

1. In Command Prompt:

> npm install --save-dev @cucumber/cucumber

> npm install --save-dev cypress-cucumber-preprocessor


2. Add the below script to the package.json:

package.json
"cypress-cucumber-preprocessor": {
  "cucumberJson": {
    "generate": true,
    "outputFolder": "./dist/cypress/apps/<app-name>-e2e>/cucumber",
    "filePrefix": "",
    "fileSuffix": ".cucumber"
  },
  "nonGlobalStepDefinitions": true,
  "stepDefinitions": "./cypress/integration"
},


3. In cypress.json file add the following:

cypress.json
"testFiles": "**/*.{feature,features}",
"DEBUG": "cypress:*",
"ignoreTestFiles": ".js",
"stepDefinitions": "./cypress/integration"
Also replace support/index.ts with support/index.js.


4. In tsconfig.e2e.json or cypress/tsconfig.json file add the following:

tsconfig.e2e.json
"types": ["cypress", "node"],
Also replace support/index.ts with support/index.js.


5. Comment out the imports in cypress/support/index.js (if not required) (make sure this file is .js and not .ts):

cypress/support/index.js
// import './commands';

6. In cypress/plugins/index.js file content should look like the below code:

cypress/plugins/index.js
const cucumber = require('cypress-cucumber-preprocessor').default;
module.exports = (on) => {
  on('file:preprocessor', cucumber());
};

7. In cypress/integration/cucumber folder create the following files:

1. <featureName>.feature
2. <featureName>/<speceFilename>.js


8. Run the following commands:

> npm start
> npm run cy:open
