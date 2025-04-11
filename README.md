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

Under settings, make sure the Domain Controller (Windows Server 2025) has two network adapters. One adapter should be NAT by default and the other should be enabled and set to Internal.
For the Clients (normal Windows 10 machines) make sure to change the network adapter settings to Internal. 
- On VirtualBox, make sure to turn off unattended install in order to download Windows 10 Pro without a product key. 
This serves to simulate a corporations internal network.

## Install and Configure Active Directory Domain Services

## Configure DHCP, DNS, and RAS

## Add users

## Configure a client workstation

## Troubleshooting
- I initially misconfigured the second NIC, which caused the DHCP and DNS services to break. As a result the internal server would have an APIPA address.
    - After inputting the correct settings for the second NIC, I had to reconfigure the DNS and DHCP services. Then I logged back into the client workstation to check if I was under the correct domain.

## Extra notes
Not sure why but for my installation, I needed to go into my virtual box file through the command line and enter this command "./VBoxManage modifyvm "DC" --hpet on" in order for the VM to not boot instantly and hang.