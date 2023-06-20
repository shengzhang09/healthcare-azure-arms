# healthcare-azure-arms 

## Deploy Azure ARM Templates via Github Actions
1. Log in Azure Portal, open Cloud Shell in bash shell.
2. Copy and paste contents in "create_service_principal_az_cli.txt" to the bash shell and hit enter to execute the script for creating the service principal.
3. Copy the JSON output and store it as a GitHub secret within your GitHub repository. To do this, from your GitHub repository, select the Settings tab. From the left menu, select the Secrets and variables drop-down button, and then select Actions.

ATTENTION: User access must be confirmed for modifying repository secrets. If you are unable to do so, please follow the instruciton of Deploy Azure ARM Templates via PowerShell terminal in Azure Cloud Shell.  

4. Enter the following values and then select Add secret:
- Name: Enter AZURE_CREDENTIALS.
- Secret: Paste the JSON output that you copied earlier.

ATTENTION: If the secret is already exist, update the existing secret with the latest JSON output.

5. Run workflow main.yaml in Actions to deploy all resources to Azure.

ATTENTION: Please ensure to change app server variables to avoid failure on conflict server names.

## Deploy Azure ARM Templates via PowerShell terminal in Azure Cloud Shell
1. Log in Azure Portal, open Cloud Shell in the PowerShell terminal.
2. Upload following ARM templates and corresponding parameters via the terminal.
  - service_app_1_parameters.json
  - service_app_2_parameters.json
  - service_app_template.json
  - frontdoor_parameters.json
  - frontdoor_template.json
3. Copy and paste the script in powershell_cmds.txt to the terminal.
4. Click 'Enter' to run the script.

## Deploy Node Web Application to App Service using GitHub Actions
You can quickly get started with GitHub Actions by using the App Service Deployment Center. This will automatically generate a workflow file based on your application stack and commit it to your GitHub repository in the correct directory.

  1. Navigate to your webapp in the Azure portal
  2. On the left side, click Deployment Center
  3. Under Continuous Deployment (CI / CD), select GitHub
  4. Next, select GitHub Actions
  5. Use the dropdowns to select your GitHub repository, branch, and application stack
  If the selected branch is protected, you can still continue to add the workflow file. Be sure to review your branch protections before continuing.
  6.On the final screen, you can review your selections and preview the workflow file that will be committed to the repository. If the selections are correct, click Finish
  This will commit the workflow file to the repository. The workflow to build and deploy your app will start immediately.
  7. Please be aware that there are changes need to be made to the workflow file generated from the previous step. An example of executable workflow file is shown below.
  
```
name: Build and deploy Node.js app to Azure Web App - SJEHHC-RG01-WebApp01-F001

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v2 # Must v2 

      - name: Set up Node.js version
        uses: actions/setup-node@v3 # Must v3
        with:
          node-version: '18.x'

      - name: npm install, build, and test
        run: |
          npm install
          # Remove other npm build commands
          npm run build --if-present
      - name: Upload artifact for deployment job
        uses: actions/upload-artifact@v3 # Must v3
        with:
          name: node-app
          path: .
  deploy:
    runs-on: ubuntu-latest
    needs: build
    environment:
      name: 'Production'
      url: ${{ steps.deploy-to-webapp.outputs.webapp-url }}

    steps:
      - name: Download artifact from build job
        uses: actions/download-artifact@v3 # Must v3
        with:
          name: node-app

      - name: 'Deploy to Azure Web App'
        id: deploy-to-webapp
        uses: azure/webapps-deploy@v2
        with:
          app-name: 'SJEHHC-RG01-WebApp01-F001' # Your app name
          slot-name: 'Production'
          publish-profile: ${{ secrets.AZUREAPPSERVICE_PUBLISHPROFILE_62CA1228018F4D04AFDBF4CB7BE91C1B }}
          package: .  
```
