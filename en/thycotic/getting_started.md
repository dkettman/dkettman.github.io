# Getting Started with Thycotic Secret Server
These are the steps followed to get Thycotic Secret Server installed in a lab. 

## Requirements
You will need:

* An Active Directory domain
  * Instructions [here](general/getting_started.md)
* AD Certificate Authority
* A MS SQL server
  * Using SQL 2019 in this lab
* A server to run IIS
* A server for the Distributed Engine (DE)
* A server running RabbitMQ for messaging

Generally, each of these would be on their own servers in a production environment. If there are limited resources, these can be consolidated into fewer boxes keeping in mind that there may be resource contention.

## Pre-work
Before getting started, it will help to have a couple service accounts already created to support this installation. I have created two accounts:

* svc-sql-thycotic
  This will be used to access the SQL database

* svc-thycotic-ss
  This will be used to run the IIS-related tasks and services for Secret Server
  
The creation of these accounts can be accomplished by executing the following in a PowerShell console:
```
$SecurePass = Read-Host -Prompt "Password for service accounts" -AsSecureString

New-ADUser svc-sql-thycotic -ChangePasswordAtLogon $false -PasswordNeverExpires $true -Path "OU=Service Accounts,DC=thycotic,DC=lab" -AccountPassword $SecurePass -Enabled $true
New-ADUser svc-thycotic-ss -ChangePasswordAtLogon $false -PasswordNeverExpires $true -Path "OU=Service Accounts,DC=thycotic,DC=lab" -AccountPassword $SecurePass -Enabled $true

Add-ADGroupMember "Service Accounts" -Members @((Get-ADUser svc-thycotic-ss), (Get-ADUser svc-sql-thycotic))
```

Next, we will [Setup MSSQL](setup_mssql.md).
