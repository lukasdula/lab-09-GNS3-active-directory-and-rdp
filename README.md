
<br>

# **Active Directory and Remote Desktop Protocol**


<br>

## **Introduction and Objectives**

This lab demonstrates the deployment of a Windows domain environment using Windows Server as a Domain Controller and two Windows 10 Enterprise client computers. The project focuses on centralized authentication, DNS-based domain resolution, and secure Remote Desktop communication inside the lab.local domain.

The goal is to install and configure Active Directory Domain Services, create a new forest and domain structure, integrate client systems, apply firewall rules, and test secure remote access with domain user accounts and group access.


<br>

## **Topology Diagram**

![](images/Pasted%20image%2020260219024033.png)


<br>



## **The objectives**

- Install and promote Active Directory Domain Services
    
- Create a new forest and domain (lab.local)
    
- Configure Organizational Units, domain users, and security group
    
- Join Windows 10 Enterprise clients to the domain
    
- Configure firewall rules for ICMP and RDP (TCP 3389)
    
- Enable and test Remote Desktop using domain authentication
    
- Verify domain functionality using PowerShell commands
    


<br>


## **Lab Structure**

1. [Active Directory Installation](01-active-directory-installation.md)
    
2. [Domain Join](02-domain-join.md)
    
3. [Remote Desktop Configuration](03-remote-desktop-configuration.md)
    


<br>

## **Key Features**

- Active Directory deployment with new forest (lab.local) and Domain Controller
    
- Structured domain design using Organizational Units and security group (Lab-Employees)
    
- DNS-based domain resolution integrated with the Domain Controller
    
- Windows 10 Enterprise clients joined with verified domain authentication
    
- Firewall configuration for ICMP and Remote Desktop (TCP 3389)
    
- Remote Desktop secured through Network Level Authentication and group-based access control
    


<br>

## Author's Note

This project is the first time I build a working Active Directory domain from start to finish. I install the Domain Controller, join the clients, and test domain login successfully.

It is also my first Remote Desktop setup inside a domain with correct firewall rules and group access. This lab gives me strong basics for a more complex Active Directory project in the future.

<br>

---


© 2026 - Lukas Dula | Home Network Lab & Portfolio
