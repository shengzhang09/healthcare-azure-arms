# healthcare-azure-arms

## Deploy Azure ARM Templates via Github Actions
1. Log in Azure Portal, open Cloud Shell in bash shell.
2. Copy and paste contents in "create_service_principal_az_cli.txt" to the bash shell and hit enter to execute the script for creating the service principal.
3. Copy the JSON output and store it as a GitHub secret within your GitHub repository. To do this, from your GitHub repository, select the Settings tab. From the left menu, select the Secrets and variables drop-down button, and then select Actions.

ATTENTION: User access must be confirmed for modifying repository secrets. If you are unable to do so, please follow the instruciton of Deploy Azure ARM Templates via PowerShell terminal in Azure Cloud Shell.  

4. Enter the following values and then select Add secret:
- Name: Enter AZURE_CREDENTIALS.
- Secret: Paste the JSON output that you copied earlier.
5. Run workflow main.yaml in Actions to deploy all resources to Azure. 

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
