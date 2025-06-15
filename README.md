# Lab 2 – Threat Detection with Azure Monitor

## Objective
Deploy a Flask-based login application to Azure App Service, collect logs using Azure Monitor, and detect brute-force login failures using KQL and alert rules.

## Components Used
- Azure App Service (Linux)
- Azure Log Analytics Workspace
- Azure Monitor Alert Rules
- Python (Flask)
- VS Code REST Client

## Step-by-Step Process

### 1. Flask Login App
- Created `app.py` to handle login POST requests.
- Logs successful and failed login attempts using Python logging module.

### 2. Deploy to Azure
- Created Azure Resource Group and App Service Plan.
- Deployed the Flask app using ZIP deployment method:
  ```
  az webapp deployment source config-zip --resource-group <rg> --name <app-name> --src deploy.zip
  ```

### 3. Enable Diagnostic Logs
- Enabled `AppServiceConsoleLogs` to stream to Log Analytics workspace:
  ```
  az monitor diagnostic-settings create --name enable-logs --resource <app-id> --workspace <workspace-id> --logs '[{"category": "AppServiceConsoleLogs", "enabled": true}]'
  ```

### 4. Query Failed Logins with KQL
KQL used to detect failed logins:
```
AppServiceConsoleLogs
| where TimeGenerated > ago(5m)
| where ResultDescription has "FAILED"
```

### 5. Simulate Brute-Force Attacks
- Used VS Code REST Client to send multiple failed login requests.
- Confirmed failure logs appeared in Azure Monitor.

### 6. Create Alert Rule
- Created alert rule when failed login logs exceed threshold.
- Logic: More than 4 failures in 5 minutes triggers alert.
- Action Group configured to send email notification.
### Vedio Link: https://youtu.be/-wnqegTtktY
## Files Included
- `app.py`: Flask login service
- `requirements.txt`: Python dependencies
- `test-app.http`: REST Client requests
- `deploy.zip`: Deployment package (if applicable)
- `README.md`: This file

## Reflection
This lab demonstrates the power of real-time monitoring and alerting for detecting potential security threats in cloud-hosted web applications.

