To create a PowerShell script that detects and downloads all Azure Defender for Cloud alerts for all Azure subscriptions and exports the data to a spreadsheet (CSV format), you can use the Azure PowerShell module. Here's a script to accomplish this:

1. **Install the Azure PowerShell module** (if not already installed):
   ```powershell
   Install-Module -Name Az -AllowClobber -Scope CurrentUser
   ```

2. **Authenticate to Azure**:
   ```powershell
   Connect-AzAccount
   ```

3. **Script to download Azure Defender for Cloud alerts for all subscriptions**:
   ```powershell
   # Authenticate to Azure
   Connect-AzAccount

   # Get all subscriptions
   $subscriptions = Get-AzSubscription

   # Initialize an array to store the Defender for Cloud alerts
   $defenderAlerts = @()

   foreach ($subscription in $subscriptions) {
       # Set the subscription context
       Set-AzContext -SubscriptionId $subscription.Id

       # Get all Defender for Cloud alerts for the subscription
       $alerts = Get-AzSecurityAlert

       foreach ($alert in $alerts) {
           # Add the alert to the array
           $defenderAlerts += [pscustomobject]@{
               SubscriptionId     = $subscription.Id
               AlertDisplayName   = $alert.AlertDisplayName
               AlertType          = $alert.AlertType
               Severity           = $alert.Severity
               Status             = $alert.Status
               ReportedTimeUtc    = $alert.ReportedTimeUtc
               Description        = $alert.Description
               RemediationSteps   = $alert.RemediationSteps
           }
       }
   }

   # Export the Defender for Cloud alerts to a CSV file
   $defenderAlerts | Export-Csv -Path "DefenderForCloud_Alerts.csv" -NoTypeInformation

   Write-Host "Defender for Cloud alerts have been exported to DefenderForCloud_Alerts.csv"
   ```

### Explanation:

1. **Authenticate to Azure**: The script starts by authenticating to Azure using `Connect-AzAccount`.

2. **Get All Subscriptions**: It retrieves all subscriptions associated with your account using `Get-AzSubscription`.

3. **Set Subscription Context**: For each subscription, the script sets the context to that subscription using `Set-AzContext`.

4. **Get Defender for Cloud Alerts**: It retrieves all Defender for Cloud alerts for the subscription using `Get-AzSecurityAlert`.

5. **Store Alerts**: The script stores the Defender for Cloud alerts in an array.

6. **Export to CSV**: Finally, it exports the collected Defender for Cloud alerts to a CSV file named `DefenderForCloud_Alerts.csv`.

This script ensures that all subscriptions associated with your account are processed, and Defender for Cloud alerts are exported to a CSV file.
