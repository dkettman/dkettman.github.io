# Installing SecretServer

## Prerequisites

* .NET 4.8 (or later) must be installed.
  * Can be downloaded from [here](https://dotnet.microsoft.com/download/dotnet-framework/net48) (Restart required)

First on the server that will be serving the web pages, IIS must be installed. This can be done via the GUI in Server Manager or via Powershell by running:
```
Install-WindowsFeature -name Web-Server -IncludeManagementTools
```
Next, uncompress SecretServer and put it in a directory called `SecretServer` in `C:\inetpub\wwwroot`. After that has been copied, open the IIS Manager. In the IIS Manager, expand the server and right-click on `Application Pools` then choose `Add Application Pool...`. Name the Application Pool `SecretServer` and choose the latest version of .NET in the list. Make sure that the `Managed pipeline mode:` is set to `Integrated`. Right-click on the newly created Application Pool and choose `Advanced Settings`. Set the following options as indicated:

* (General)
  * Start Mode: AlwaysRunning
* Process Model
  * Identity:
    Click the three dots on the right then choose `Custom account:`. Click `Set...` then choose the service account created previously as: `DOMAIN\username`. Enter and validate the password as well. Click OK.
  * Idle Time-out (minutes): 0
* Recycling
  * Regular Time Interval (minutes): 0
  
Now we will start the install process of SecretServer. Navigate to 
