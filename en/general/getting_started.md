# Getting Started

This is going to go over the basics of setting up a domain quickly and repeatably. It will be setup in sections based on what piece is to be installed. Some sections will be optional if you dont need them, but for the most part, all are required. The installed components will be:

* Active Directory Services
* DNS
* Basic AD User/Group structure
* Group Policy for Service Accounts
* Active Directory Certificate Authority and Web Enrollment

Before starting you will need a server to house all of this. I am running on Windows 2019, so if you are on a different version, YMMV. I'm running on a 2 CPU/8GB VM and it is more than enough for what I'm doing. I could probably cut it down to 1 CPU/4GB ram.

Some housekeeping that I do to every server running Windows is the following:

* Disable IE ESC
  * It only gets in the way
* Disable all three firewalls
  * Again, only gets in the way and I'm in an isolated network
* Set the timezone correctly
* Ensure that Remote Desktop is enabled.

All of these things can be done from Server Manager and are fairly easy to accomplish without having to go into detail here.

For the Domain Controller, it is important that it has a static IP address and not a dynamic IP. So ensure that is setup as well.

## Active Directory Services and DNS
To install Active Directory Services and DNS, run the following at a PowerShell command prompt:
```
Add-WindowsFeature AD-Domain-Services -IncludeManagementTools
```

This will install both the AD and DNS services on the server. Once it is complete, open Server Manager and in the upper right, click the flag and it will have a task to complete the configuration of the domain. Follow the wizard and once it is done, you will be prompted to reboot. Congratulations! You have a (very) basic domain setup!

## Basic AD User/Group structure
There are many standards that could be followed from an AD User/Group perspective. Here it will just be kept simple. Three OUs: `Staff`, `Groups`, `Service Accounts`. One group: `Service Accounts`. A Group Policy that allows everyone in the `Service Accounts` AD Group the privilege to `Log on as a service`.

The creation of the AD OUs and Groups can be accomplished in PowerShell with:
```
New-ADOrganizationalUnit "Staff"
New-ADOrganizationalUnit "Groups"
New-ADOrganizationalUnit "Service Accounts"

New-ADGroup -Name "Service Accounts" -GroupScope Global -GroupCategory Security -Path "OU=Groups,DC=<domain>,DC=<domain>"
```
Be sure to change both of the <domain> indicators as appropriate for your domain.

## Group Policy for Service Accounts
I'm sure there is a way to configure Group Policy from the command line, but that is going to take more time than I'm willing to put into it. So here are the brief instructions on how to accomplish this from your server using the GUI:

1. Open Group Policy Management console
1. Drill down Forest => Domains => <domain>
1. Right-click on Default Domain Policy and choose Edit
1. The path you are going to follow is: 
    * Computer Configuration => Policies => Windows Settings => Security Settings => Local Policies => User Rights Assignment
1. In this list on the right, find `Log on as a service` and double-click on it
1. Tick the checkbox `Define these policy settings` and click `Add User or Group`.
1. In this window, click `Browse...` and search for the AD group created previously: `Service Accounts`
1. Cliick OK, then close the `Group Policy Management Editor` window.
1. Open a PowerShell and run `gpupdate /force` to force the policy to be re-read and applied.

