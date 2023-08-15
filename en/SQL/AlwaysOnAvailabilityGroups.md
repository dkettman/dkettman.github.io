# Introduction
Following these instructions should get an Always On Availability Group created between two SQL servers. 

## Assumptions
I will be assuming that you already have SQL2022 installed on two machines. Install your application on your "primary" SQL server. This is where we will pick up our instructions.

## Notes prior to getting started
Through my attempts, I was not able to get a listener working. While this is not required for the AG to function, in more complex setups, it could be required for routing of traffic (Read-only replicas, etc). 

Additionally, I am NOT a SQL-ologist, nor did I major in DBA-studies. What I have found to work for me here is probably missing a TON of things that even a mildly-seasoned DBA would see and be repulsed. While functionally, this may work as an Always On Availability Group, do not think about using any of this production. I really wouldn't even use it in a lab!

# First SQL Server
Once SQL is installed on your first server, go ahead and create whatever databases are needed for the project. If this is for an application, go ahead and install the application fully. I can wait...

...

Oh, you're done? Fantastic!

Now, we first need to make sure that a few things are in place before we move on. First, we need to make sure that the MSSQLSERVER service is setup to support Always on Availability. To do this, open the 'SQL Server Configuration Manager' panel, and on the 'SQL Server Services' section, right-click on 'SQL Server (MSSQLSERVER)' or whatever the appropriate instance is and choose properties. On the "Always On Availability Groups" tab, make sure the 'Enable Always On Availablity Groups' box is checked. If it wasn't, you will be prompted to restart the MSSQL server service. This can be done from the Configuration Manager by right-clicking on the MSSQLSERVER (or other appropriate service) and choose restart.

Back in SQL Management Studio, open a new query on your primary SQL server. Enter the following queries substituting [dbname] for your actual database name:

```sql
ALTER DATABASE [dbname] SET RECOVERY FULL;
BACKUP DATABASE [dbname] TO DISK = 'NUL';
GO
```

This sets the Recovery model for the database to "FULL" which is required for the Availability Groups to function. This also kicks off a "backup". I put that in quotes because it is backing up the database to "NUL" which is the equivalent to `/dev/null` in Linux. This essentially sets the "Backup" flag on the database so it knows it has been backed up at least once. 

Now that all of that is out of the way....

1. In SQL Management Studio, Right-click on 'Always On High Availability Groups' and choose 'New Availablity Group Wizard...'. If the Introduction page pops up, go ahead and click Next. Here, we will name our availability group. It should be a name that makes sense to you and others who may see it. Set the Cluster Type to NONE. Click Next.
2. Here, this is where you will select which databases will be added to the group. If you did not execute either of the T-SQL commands previously, the database(s) you want to add to the AG will not be available. Tick the Checkboxes of the databases you want in the AG and click Next.
3. Now you will choose your Replicas. These are the other SQL servers that will participate in the AG. Your current SQL server is already added. Click 'Add Replica...' and it will pop up a login screen for SMS. Enter in the credentials to login to your other database server. If all goes right, it should add it as a second line in the 'Availability Replicas:' portion of the screen.
4. Next, click on the Listener tab. Take a moment and go back and re-read my notes at the top of this page. I never got the Listener to work. It is more than likely a skill issue, but the wizard will issue a warning if a listener isn't setup. Choose 'Create an availability group listener' and then put in a DNS name (limited to 15 characters) and a port. At the bottom, click 'Add...' and add the IPv4 address to use for the listener. The DNS name and IP should not be in use currently in your domain. Once this is done, go ahead and click 'Next'
5. On the 'Select Data Synchronization' screen, you can leave 'Automatic Seeding' selected. It will automatically seed each new replica as they are added. Click 'Next'.
6. This is the Validation page, it will run thru some initial tests to determine whether or not the AG is ready to setup. So long as there are no errors, click 'Next'. 
7. A summary screen is displayed, go ahead and progress the wizard to complete the setup of the AG.