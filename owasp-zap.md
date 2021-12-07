Installation of OWASP ZAP
Install Java:

It requires JVM 8+. We can download the latest version of Java SE Development Kit (JDK) from https://www.oracle.com/java/technologies/downloads/

Download ZAP:

Download OWASP ZAP from  https://www.zaproxy.org/download/

Install ZAP:

Double click on the installation file and perform a standard installation.

Launch ZAP:

Click on ZAP application to launch the application. Select "No, I don't want to persist the session.....".

Test using OWASP ZAP
Simple Testing:

Click on "Automated Scan" in "Quick  Start" Tab.
Provide the url of the web application.
Click "Attack".
Note: We may also choose "Manual Explore" to do the testing manually proxied through ZAP.

Rest Api Testing:

Download the swagger.json from the project's Swagger page.
Do necessary modifications like default value, type, etc.
In ZAP click Import → "Import an OpenApi from...."
Choose/Provide the path for OpenApi json file & provide the "Target Url" as the application's base url.
Click "Import" to import the apis into the ZAP tool.
From the section in the left, go to Site → <Application-url>
Right click on the <Application-url> and select "Active Scan".
Results:

We can see the results in the tabs in the lower half of the application.
Check for suggestions to fix in the "Alerts" tab.
Reports:

Go to Reports → Generate Report → Change values if required → Click "Generate Report"
Open the generated file in the browser.
