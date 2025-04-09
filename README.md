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
This is to simulate a corporations internal network

## Extra notes
Not sure why but for my installation, I needed to go into my virtual box file through the command line and enter this command "./VBoxManage modifyvm "DC" --hpet on" in order for the VM to not boot instantly and hang.