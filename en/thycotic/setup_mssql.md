# Setup MSSQL

Installing MSSQL is outside the scope of these instructions. What will be covered is adding the SQL service account previously created to the SQL server.

To keep it simple, install the SQL Management Studio which can be downloaded [here.](https://docs.microsoft.com/en-us/sql/ssms/download-sql-server-management-studio-ssms?view=sql-server-ver15)

Once downloaded and installed, connect to the SQL server using credentials with sysadmin rights. Once logged in, expand Security and Logins. Right-click on Logins, then choose `New Login...`.
In the `Login name:` field, click the `Search...` button and find the service account created for SQL access. If following these instructions, it will be `svc-sql-thycotic`. Click the `Server Roles` tab and then tick the checkbox next to `sysadmin`. Click `OK` and then you can close SMS.

Next, we will [Install SecretServer](install_secretserver.md).