# Databaseinfo
**Steps to integrate DB with Cypress**
1. Sign in to the Azure portal: Go to Azure Portal and sign in with your Azure account.
2. Create a new SQL Database:
    * In the Azure portal, click on "Create a resource" in the left-hand menu.
    * Search for "SQL Database" and select it.
    * Click on "Create" to start the creation process.
3. Configure the SQL Database:
    * Subscription: Select your Azure subscription.
    * Resource group: Create a new resource group or select an existing one.
    * Database name: Enter a name for your database.
    * Server: Create a new server or select an existing one. Provide a server name, admin login, and password if creating a new server.
    * Compute + storage: Click "Configure database" and select the "Basic" service tier, free for 12 months with limited resources.
4. Review and create:
    * Review the configuration and click on "Create" to deploy the database.
Once the deployment is complete, you will have a free SQL Database that you can use for your applications.



**Integrate SQL db with cypress**
- Cypress doesn’t support sql db so we need to add a plugin 
- Access https://www.npmjs.com/package/cypress-sql-server

    1. Copy and paste this command in VSC terminal  npm install --save-dev cypress-sql-server - Cypress sql server gets installed and we can see it as one of the dev dependency added in package.json file i.e.., "cypress-sql-server": "^1.0.0"
    2. How to make this plugin activate in cypress ? we need to register the plugin in cypress.config.js file in setup node method  Add these 2 lines of code in the setup node method   
 tasks = sqlServer.loadDBPlugin(config.db);
    on('task', tasks);

    - Add this package  in the same cypress.config.js file
 const sqlServer = require('cypress-sql-server');
   3. Now, add all the user names and password and other details by adding this code in cypress.config.js file in setupnodevents method by creating an object as

      config.db = {
     "userName": "",
     "password": "",
     "server": "",
     "options": {
          "database": "",
          "encrypt": true,
          "rowCollectionOnRequestCompletion" : true
     }
 }
 4. We need to load custom commands  add these 2 lines of code in e2e.js file in support folder  this code can retrieve the data from the database   import sqlServer from 'cypress-sql-server';
     sqlServer.loadDBCommands();

5. create a testcase and retrive the data from database 
example: 
describe('example to-do app', () => {
    it('displays two todo items by default', () => {

        cy.sqlServer("SELECT * FROM Persons").then(function(result)) 
            console.log(result[0][1])
        })
    }
Output: it displays the 0th row, 1st column data present in database in Persons table
**Note: we need to create data in database prior doing the above step**
