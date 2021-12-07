Though Puppeteer is an alternative to Cypress, in LD projects it is used as an bridge to fill the gap which Cypress has. Some background tasks can be initiated from Cypress to run the Puppeteer script. The Puppeteer script can then return the results for the Cypress scripts to continue in its flow.



Here are the steps to follow to configure Puppeteer:

1. In the Command prompt:

npm install --save-dev puppeteer



2. In the Cypress spec file provide the below lines among the other Cypress codes:



some.spec.ts
const options =  {
  appUrl; '<web-app-url>',
  textContent: 'textcontenthere'
};
 
cy.task('<taskName>', options).then(response => { console.log(response.localStorageData); });




3. In cypress/plugins/index.js add the necessary line of codes:



cypress/plugins/index.js
const { scriptName } = require('./puppeteer-script-file');
 
module.exports = (on, config) => {
  ...
  on('<taskName>', { scriptName });
};




4. The sample code in puppeteer-script-file.js may look like the following:



puppeteer-script-file.js
const puppeteer = require('puppeteer');
 
module.exports = {
 
async scriptName(options = {}) {
  const appUrl = options.appUrl;
  const browser = await puppeteer.launch({
    headless: false, // make this true to make the test run in the background
  });
 
  const context = await browser.createIncognitoBrowserContext();
  const page = await context.newPage();
 
  // Go to the application and click the button
  await page.goto(appUrl);
  await page.click('#buttonId');
  await page.waitForNavigation();
 
  // Enter text content
  await page.waitForSelector("[name=textboxName]");
  await page.focus("[name=textboxName]");
  await page.type("[name=textboxName]", options.textContent, { delay: 100 });
  await page.keyboard.press('Enter');
 
  await page.waitForTimeout(5000);
 
  // Check for the identifier is found
  await page.waitForSelector("#htmlIdentifier");
 
  // Get the local storage
  const result = await page.evaluate(() => {
    let json = {};
    json['res'] = localStorage.getItem('itemDetails');
    return json;
  });
 
  // Close the page
  await page.close();
  return { localStorageData: result };
},
 
};
