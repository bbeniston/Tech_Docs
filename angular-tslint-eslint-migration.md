INTRODUCTION
Purpose

To see how to migrate a Angular project lint package from TSLint to ESLint.


Reason

Angular is dropping support to TSLint package as there is not going to be any further development in TSLint package.

Steps to Migrate:
Follow the below steps. But watch out for the output in the terminal for logs/errors.


Add ESLint Schematics

ng add @angular-eslint/schematics

Generate ESLint file for your project(s)

ng g @angular-eslint/schematics:convert-tslint-to-eslint {{project-name-found-in-angular.json-file}}
You might be asked the below questions:

> Would you like to remove TSLint and its related config if there are no TSLint projects remaining after this conversion? Yes
> Would you like to ignore the existing TSLint config? Recommended if the TSLint config has not been altered much as it makes the new ESLint config cleaner. No


Run Lint Command

ng lint
We are sure to get many lint errors. Fix all the errors.

But if any rule seems irrelevant to our coding standards (set for our project), we may switch off that rule in the .eslintrc.json file and rerun the lint command.



Note:

During the course of these command executions we may see changes in package.json, angular.json and other coding files based on the existing TSLint configuration.



The file .eslintrc.json holds the lint rules. We may customize the settings here based on our coding requirements.


If our project is running on NX Mono Repository Architecture, use the below link to see the migration steps:

https://dev.to/this-is-angular/the-ultimate-migration-guide-to-angular-eslint-eslint-and-nx-11-1eh2#migrating-an-existing-nx-10-angular-workspace-using-tslint
