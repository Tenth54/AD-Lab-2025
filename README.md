# AD-Lab-2025
 My experience learning active directory using Josh Madakor's video.
 
This lab is just a demonstration of implementing active directory using virtual machines.

## Technology
- VirtualBox
- Active Directory Domain Services
- Powershell

## Operating Systems
- Windows Server 2025
- Windows 10 

## Deployment

- Under settings, make sure the Domain Controller (Windows Server 2025) has two network adapters. One adapter should be NAT by default and the other should be enabled and set to Internal.
- For the Clients (normal Windows 10 machines) make sure to change the network adapter settings to Internal. 
    - This serves as a representation of a corporate's internal network.


## Install and Configure Active Directory Domain Services

## Configure DHCP, DNS, and RAS

## Add users
Run windows powershell ISE as administrator
Open scripts and select the powershell file
[picture]

We should be unable to run without enabling some settings
[picture]

Execute the command "Set-ExecutionPolicy Unrestricted"
- note: do not do this in a corporate environement, since this is a lab it should be done for convenience.

Change directory to the folder that has the script so it can read the text file with a bunch of names.
run the script
[picture]

Verify that the organizational unit, _Users, was created and populated. 
[picture] 

## Configure a client workstation
Create the Client VM in windows 10 and name it "CLIENT1". 
If you are doing an unattended install, make sure you select Windows 10 Pro in order to join the domain. I think Windows 10 enterprise and education can join a domain as well.

If you are doing an attended install, I think you can use a windows 10 pro generic key for an in-place upgrade.

When the installation is finished, we run ipconfig to confirm that we are under the internal network.
[picture]

Next we test out our network by pinging a website like google.com. If we are successful that means our domain controller is applying the NAT process properly and that our DNS is working as well. 
[picture]

### Joining Domain and Changing Workstation's Name
Right click the windows icon and click on settings. In the settings, click advanced name change  under domain and click change to join our domain. 

[picture]

input our credentials and restart
[picture]

Verify that th eclient has received an IP by checking the DC's DHCP server lease logs.
[picture]

Verify that CLIENT1 shows up in the Active Directory Users and Computers under the "Computers" folder in the domain.
[picture]

Now any account under _Users should be able to login to this workstation. 
[picture]

## Troubleshooting
- I initially misconfigured the second NIC, which caused the DHCP and DNS services to break. As a result the internal server would have an APIPA address.
    - After inputting the correct settings for the second NIC, I had to reconfigure the DNS and DHCP services. Then I logged back into the client workstation to check if I was under the correct domain.

## Extra notes
Not sure why but for my installation, I needed to go into my virtual box file through the command line and enter this command "./VBoxManage modifyvm "DC" --hpet on" in order for the VM to not boot instantly and hang.

## Resource used
[https://youtu.be/MHsI8hJmggI?si=zPfmWqB8cfqgEOk8](Josh Madakor's lab)
[https://www.virtualbox.org/wiki/Downloads](VirtualBox)
[https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2025](Windows 2025 Server ISO)
[https://www.microsoft.com/en-us/software-download/windows10](Windows 10 Media Creation)
