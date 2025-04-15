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
Install Active Directory Domain Services through the server manager's "add roles and features"
![Active Directory Domain Services](/images/AD%20Lab_4.png)

Promote to Domain Controller
![promote to Domain Controller](/images/AD%20Lab_5.png)

Setup a new forest and establish your domain name. It could be anything, I ultimately chose activedirectorylab.com
![setup forest](/images/AD%20Lab_6.png)

Restart and login to the newly created domain account.
![restart](/images/AD%20Lab_7.png)

Now we create an organizational unit in order to create dedicated admin accounts instead of using the built in admin account.
![OU](/images/AD%20Lab_8.png)
![admin folder creation](/images/AD%20Lab_9.png)

Add a new user to the admin OU 
![new admin account](/images/AD%20Lab_10.png)
![user info](/images/AD%20Lab_11.png)

Set the password to never expire since its a lab environment
![password](/images/AD%20Lab_12.png)

Go to new user’s properties and give them the admin role by entering "Domain Admins" for object name
![Domain Admin](/images/AD%20Lab_13.png)

restart and sign in to our newly created admin account
![restart2](/images/AD%20Lab_14.png)

## Configure RAS and NAT
Next we install a RAS/NAT to keep our client’s internet private/ within the company’s network while allowing them to connect to the internet by routing through the Domain Controller.

Install remote access through the server manager's "add roles and features"
![remote access](/images/AD%20Lab_15.png)

Select routing during role services, else go with the defaults.
(forgot to take screenshot)

Select "Routing and Remote Access" under tools in windows server manager.
Right click the domain controller, click configure, and enable "Routing and Remote Access"
Here we select NAT 
![NAT](/images/AD%20Lab_16.png)

Select the NIC that will connect us to the internet.
If nat works it should give our clients internet access
![Internet connection](/images/AD%20Lab_17.png)

## Configure DHCP and DNS
Next we want to configure the DHCP server for our domain controller. This allows us to assign our client workstations an IP address

Once more, go to "roles and features" in the widnows server manager and select DHCP Server.
![dhcp](/images/AD%20Lab_18.png)

Go with the default settings

Select DHCP under tools in windows server manager and add scope

Declare the DHCP scope/address range. Lease time depends on the service, but we'll go with the default since it's a lab.
![DHCP scope](/images/AD%20Lab_19.png)

Add our router/default gateway. In this case, it will be the address of the domain controller. With NAT, our domain controller will forward the clients to the internet.
![Router](/images/AD%20Lab_20.png)

Select the same IP for DNS

## Add users
Run windows powershell ISE as administrator
Open scripts and select the powershell file
![powershell script](/images/AD%20Lab_22.png)

We should be unable to run without enabling some settings
![powershell error](/images/AD%20Lab_23.png)

Execute the command "Set-ExecutionPolicy Unrestricted"
- note: do not do this in a corporate environement, since this is a lab it should be done for convenience.
![bypass](/images/AD%20Lab_24.png)

Change directory to the folder that has the script so it can read the text file with a bunch of names.
run the script.
![running script](/images/AD%20Lab_25.png)

Verify that the organizational unit, _Users, was created and populated. 
![user OU](/images/AD%20Lab_26.png)

## Configure a client workstation
Create the Client VM in windows 10 and name it "CLIENT1". 
If you are doing an unattended install, make sure you select Windows 10 Pro in order to join the domain. I think Windows 10 enterprise and education can join a domain as well.

If you are doing an attended install, I think you can use a windows 10 pro generic key for an in-place upgrade.

When the installation is finished, we run ipconfig to confirm that we are under the internal network.
![ipconfig](/images/AD%20Lab_27.png)

Next we test out our network by pinging a website like google.com. If we are successful that means our domain controller is applying the NAT process properly and that our DNS is working as well. 
![ping test](/images/AD%20Lab_28.png)

### Joining Domain and Changing Workstation's Name
Right click the windows icon and click on settings. In the settings, click advanced name change  under domain and click change to join our domain. 
![joining domain](/images/AD%20Lab_29.png)

input our credentials and restart
![credentials](/images/AD%20Lab_30.png)

Verify that the client has received an IP by checking the DC's DHCP server lease logs.
![workstation lease](/images/AD%20Lab_31.png)

Verify that CLIENT1 shows up in the Active Directory Users and Computers under the "Computers" folder in the domain.
![verify client](/images/AD%20Lab_32.png)

Now any account under _Users should be able to login to this workstation. 
![user login](/images/AD%20Lab_33.png)

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
