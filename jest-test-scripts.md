This article shows some tips on writing test scripts in Jest.

Jest is a Javascript unit testing framework used with the frontend programing languages. It helps the developer to create, run and structure tests. It can be set along with CICD to make sure our code don't throw any unexpected error when we deploy the changes.

Install and Config:
The installation and configuration of Jest in an Angular Application is detailed in the earlier article.

It is also recommended to install this VSCode Extension for easy running and debugging of Jest tests.

Creating Spec Files Automatically:
If we are starting to write test cases in an existing project which don't have spec files, we can use a global package to create the spec files automatically.

Command Prompt
npm install -g angular-spec-generator
 
angular-spec-generator <relative-path-to-the-folder>
This command don't overwrite the existing spec file contents.

It mainly creates spec files for component/directive/guard/pipe/service/index files.

After executing the generate command, we might have to remove some unwanted spec files which we don't need.

Sample spec file Examples
Here is a sample of *.component.ts file.

*.compnent.js Expand source


Here is a sample of *.pipe.ts file.

*.pipe.ts Expand source


Here is a sample of *.service.ts file.

*.service.ts Expand source
Note: The above three sample codes are just for reference only. There are more that could be added to those files.

Config Custom Commands:
Update the package.json with the Jest scripts.

package.json
"scripts": {
    ...
    "test": "jest",
    "test:coverage": "jest --coverage",
    "custom": "jest -- <relative-folder-to-test> --silent",
}
Run:
Run the test using Jest.

Command Prompt
npm run test
npm run test:coverage
npm run test:custom
Use "–silent" if we want to suppress the console outputs.

We can also get the coverage report in html format in the coverage folder. If there is any issue in generating the coverage report, review jest.config.js or the jest settings in the package.json file. Here is the guide for the coverage related settings.

VsCode Extension:
Here is a VsCode extension to debug/partial-run the test scripts.

com.atlassian.confluence.content.render.xhtml.XhtmlException: Missing required attribute: {http://atlassian.com/resource/identifier}value
