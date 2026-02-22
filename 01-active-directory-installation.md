
<br>

# **1 - Active Directory Installation**


<br>

## **1.1 Introduction**

This first chapter describes the installation and initial configuration of Active Directory Domain Services on SERVER-DC1 for centralized management of two client computers. The server is configured as a Domain Controller and a new forest with the domain name lab.local is created.

Active Directory provides centralized authentication and management of users and computers inside a network. After installation, a basic domain structure is prepared, including Organizational Units, user accounts, and a security group, which will be used for domain join and Remote Desktop access in the next chapters.


<br>

## **1.2 Topology Diagram**

![](images/Pasted%20image%2020260219021723.png)

<br>

## **1.3 Objectives**

1. Install Active Directory Domain Services (AD DS)
    
2. Configure SERVER-DC1 as a Domain Controller
    
3. Create a new forest: lab.local
    
4. Create basic Organizational Units
    
5. Create two domain user accounts
    
6. Create one security group
    
7. Verify Active Directory functionality using PowerShell




## **1.4 IP Addressing Overview**

| **Device** | **Hostname** | **IP Address** | **Subnet Mask** | **DNS Server** |
| ---------- | ------------ | -------------- | --------------- | -------------- |
| Server     | SERVER-DC1   | 10.10.10.10    | 255.255.255.0   | 10.10.10.10    |
| Client 1   | PC1          | 10.10.10.101   | 255.255.255.0   | 10.10.10.10    |
| Client 2   | PC2          | 10.10.10.102   | 255.255.255.0   | 10.10.10.10    |

<br>

## **1.5 - Active Directory Installation**

Active Directory Domain Services (AD DS) is installed using Server Manager. The role is added through the "Add Roles and Features" wizard. During installation, required management tools and DNS components are included automatically.

**In Server Manager, select:**

- Add Roles and Features
    
- Role-based or feature-based installation
    
- Select SERVER-DC1
    
- Choose Active Directory Domain Services
    
- Confirm required features
    
- Click Install
    

```plaintext
Install Active Directory Domain Services
```
![](images/Pasted%20image%2020260218232544.png)

### **Results**

The Active Directory Domain Services role is successfully installed on SERVER-DC1. The server is now ready to be configured as a Domain Controller in the next step.


## **1.6 Configure Domain Controller - Create New Forest**


This next section configures SERVER-DC1 as a Domain Controller. A new forest with the domain name lab.local is created. During this process, DNS and Global Catalog services are configured automatically. After installation, the server becomes the central authentication authority for the lab environment.

 <br>
 
```
Deployment Configuration
```
![](images/Pasted%20image%2020260219000512.png)

This step defines the deployment type of Active Directory. A new forest is created because no existing domain infrastructure is present. The root domain name lab.local is specified as the foundation of the new directory structure.

<br>

```
Domain Controller Options
```
![](images/Pasted%20image%2020260219000552.png)

In this options functional levels for the forest and domain are selected. DNS Server and Global Catalog roles are enabled automatically. A DSRM password is configured for recovery purposes. This step defines core services required for domain operation.

<br>

```
Additional Options
```
![](images/Pasted%20image%2020260219000613.png)

The NetBIOS domain name is verified. This short name (LAB) is used for legacy compatibility and internal authentication. No additional changes are required in this lab environment.

<br>

```
Paths Configuration
```
![](images/Pasted%20image%2020260219000641.png)

Default storage locations for the AD database, log files, and SYSVOL folder are confirmed. These paths define where Active Directory stores directory data and replication files. Default values are sufficient for this lab setup.

<br>

```
Prerequisites Check
```
![](images/Pasted%20image%2020260219000715.png)

In this step, the system checks if all settings are correct before installation. A warning about DNS delegation can be shown in a lab environment. This warning does not stop the installation. The installation can continue if there are no critical errors.

<br>

### **Results**

SERVER-DC1 is successfully configured as a Domain Controller for the lab.local domain. The forest is created, DNS service is installed, and the automatic reboot completes the configuration process. The domain configuration is finished and ready for further Active Directory setup.

<br>

## **1.7 Create Organizational Units**


This section creates basic Organizational Units (OU) inside the lab.local domain. Organizational Units help organize domain objects such as users and computers. In this lab, OUs are created to separate client devices and user accounts.

**Steps:**

1. Open Active Directory Users and Computers.
    
2. Right-click the domain lab.local.
    
3. Select New → Organizational Unit.
    
4. Create an OU named Clients.
    
5. Repeat the process and create an OU named Lab-Users.
    

```plaintext
Organizational Unit - Clients
Organizational Unit - Lab-Users

```
![](images/Pasted%20image%2020260219004837.png)


* The first Organizational Unit named Clients is created. This OU will be used to store client computer accounts after they join the domain.

* The second Organizational Unit named Lab-Users is created. This OU will store domain user accounts created for this lab.

### **Results**

Two Organizational Units are successfully created in the lab.local domain. The domain structure is now organized and ready for user and computer object placement.

<br>

## **1.8 Create Domain User Accounts**

This section creates two domain user accounts inside the Lab-Users Organizational Unit. Domain users are required for authentication inside the Active Directory environment. These accounts will later be used for domain login and Remote Desktop testing.

**Steps**:

1. Open Active Directory Users and Computers.
    
2. Navigate to the Lab-Users OU.
    
3. Right-click and select New → User.
    
4. Enter user information and define the logon name.
    
5. Set a password and configure password options.
    
6. Repeat the process for the second user account.
    

<br>

```plaintext
Create User - Jan Novak
```
![](images/Pasted%20image%2020260219005703.png)


User information such as first name, last name, and user logon name is defined. The account is created inside the Lab-Users Organizational Unit.

<br>

```plaintext
Set Password - Jan Novak
```
![](images/Pasted%20image%2020260219005930.png)


A password is configured for the user account. In this lab, the option "User must change password at next logon" is not selected.


>**Notes:** In this lab, the option "User must change password at next logon" is not selected for simplicity. In real environments, this option is usually enabled so that the user sets a personal password during the first login.


<br>

```plaintext
Account Summary - Jan Novak
```
![](images/Pasted%20image%2020260219005951.png)

The summary screen confirms the full name and user logon name before finishing the account creation.

<br>


**The same procedure is repeated to create the second domain user account inside the Lab-Users OU.**

```plaintext
Active Directory Users Overview
```
![](images/Pasted%20image%2020260219011928.png)


Both domain user accounts are visible inside the Lab-Users Organizational Unit.

<br>

### **Results**

Two domain user accounts are successfully created inside the Lab-Users Organizational Unit. 


<br>

## **1.9 Create Security Group**


This section creates a security group inside the Lab-Users Organizational Unit. Security groups are used to manage permissions for multiple users at once. In this lab, both domain users are added to a single group for simplified management.

**Steps**:

1. Open Active Directory Users and Computers.
    
2. Navigate to the Lab-Users OU.
    
3. Right-click and select New → Group.
    
4. Enter the group name: Lab-Employees.
    
5. Select Group scope: Global.
    
6. Select Group type: Security.
    
7. Open group Properties.
    
8. Add Jan Novak and Tereza Svobodova as members.
    

```plaintext
Create Security Group - Lab-Employees
```
![](images/Pasted%20image%2020260219013212.png)


A new security group named Lab-Employees is created inside the Lab-Users OU. The group scope is set to Global and the type is set to Security.

```plaintext
Group Properties - Members Configuration
```
![](images/Pasted%20image%2020260219014337.png)


Both domain users are added to the Members tab of the group properties. This allows centralized permission assignment for both accounts.



>**Notes:** The Global group scope is selected because it is suitable for grouping users within the same domain. In most environments, Global groups are used to collect user accounts and then assign permissions to resources.

<br>

### **Results**

The Lab-Employees security group is successfully created. Both domain user accounts are added as members. The environment is now prepared for permission-based access management in the next steps.


<br>

## **1.10 Active Directory Verification (PowerShell)**


PowerShell allows fast verification of Active Directory configuration without using the graphical interface. These commands confirm that the domain exists, users are created, and group membership is correctly configured.

<br>


- **The first command displays domain information for lab.local.**

```powershell
Get-ADDomain
```
![](images/Pasted%20image%2020260219015944.png)


*The output confirms that the domain **lab.local** is created and working correctly. The server **WIN-AG6DJPLM59A** is configured as the Domain Controller. All main domain roles (FSMO roles) are assigned to this server. The forest name and NetBIOS name are correctly set to **lab.local** and **LAB**.*



<br>



- **The second command lists all domain user accounts.**

 ```
 Get-ADUser -Filter * | Select-Object Name
 ```
![](images/Pasted%20image%2020260219015717.png)

<br>


- **The third command confirms that Jan Novak and Tereza Svobodova are members of the Lab-Employees group.**

 ```
 Get-ADGroupMember -Identity "Lab-Employees" | Select-Object Name
 ```
![](images/Pasted%20image%2020260219020207.png)

<br>

### Results

Successful output confirms that Active Directory Domain Services is configured correctly.


<br>





## **1.11 Conclusion**

This first chapter describes the installation and configuration of Active Directory Domain Services on SERVER-DC1. The server is promoted to a Domain Controller and a new forest with the domain name **lab.local** is created. Basic Organizational Units, two domain user accounts, and one security group are configured. PowerShell commands confirm that the domain, users, and group membership are working correctly.

The Active Directory environment is now prepared for client integration. In the next chapter, both Windows 11 client computers join the **lab.local** domain and domain user authentication is tested.

<br>


---


<br>


**Next Chapter:** [Domain Join](02-domain-join.md)
