Setup SonarQube:

Install Jdk 11.x (for your operating system)
Download & Unzip Sonarqube 
https://www.sonarqube.org/downloads/ (Download the community version)
Change the value in conf/wrapper.conf
wrapper.java.command=C:\ProgramFiles\Java\jdk-11.0.11\bin\java (version no. may change)
Start the SonarQube: <folder>/bin/windows-x86-64/startSonar.bat
Watch the "logs" folder for troubleshooting
Change the value in conf/sonar.properties
sonar.search.port=<different-value> (do this only if it shows that the address is already in use)
Browse through http://127.0.0.1:9000 (admin / admin)
Create a new project in the above Sonarqube portal


Configure Angular Project:

Add the sonar-project.properties file to the root of the Angular application (And set the paths correctly)
Install the Sonar Scanner in Angular project
npm i sonar-scanner --save-dev
Run the unit testing in Angular project
"npm run test:coverage" (This may change according to what you set in the package.json file)
Add "sonar":"sonar-scanner" to the scripts in the package.json file
Run "npm run sonar" in the terminal of the Angular project


Sample sonar-project.properties file:

sonar-project.properties
sonar.host.url=http://localhost:9000 
sonar.login=admin
sonar.password=admin
sonar.projectKey=project-key-given-in-the-sonarqube-portal
sonar.sourceEncoding=UTF-8
sonar.sources=apps,libs
sonar.tests=apps,libs
sonar.exclusions=**/node_modules/**
sonar.test.inclusions=**/*.spec.ts
sonar.typescript.lcov.reportPaths=coverage/lcov.info
