

# **3 - Remote Desktop Configuration

<br>



## **3.1 Introduction**

This last chapter focuses on configuring and testing Remote Desktop between two domain-joined client computers (PC1 and PC2) inside the lab.local domain.

The goal is to enable Remote Desktop on PC2, configure the required firewall settings, assign the correct permissions for domain users, and verify that remote access works correctly.

<br>


## **3.2 Topology Diagram**


![](images/Pasted%20image%2020260222021839.png)


<br>

## **3.3 Objectives**

1. Enable Remote Desktop on PC2
    
2. Verify Remote Desktop firewall rules (TCP 3389)
    
3. Configure Remote Desktop user permissions
    
4. Test RDP connection from PC1 to PC2
    
5. Authenticate using domain credentials
    
6. Verify active RDP session on PC2
    




<br>

## **3.4 Enable Remote Desktop**

Remote Desktop is enabled on PC2 to allow remote access from another domain-joined computer inside the lab.local environment.

<br>

### **PC2 - Enable Remote Desktop**

```plaintext
sysdm.cpl
Remote
```
![](images/Pasted%20image%2020260222013319.png)
<br>

Remote Desktop is set to "Allow remote connections to this computer" and Network Level Authentication (NLA) is enabled.

<br>

### **Results**

Remote Desktop is successfully enabled on PC2. The system automatically enables the required firewall rules for RDP communication (TCP port 3389).



<br>

## **3.5 Verify RDP Firewall Rules**

Remote Desktop firewall rules are verified using Windows Defender Firewall with Advanced Security.

### **PC2 - Firewall Verification**

**The following inbound rules are enabled:**

- Remote Desktop - User Mode (TCP-In)
    
- Remote Desktop - User Mode (UDP-In)
    

**Both rules are set to:**

- Profile: All
    
- Enabled: Yes
    
- Action: Allow

```
Win + R  
wf.msc
Inbound Rules
```
![](images/Pasted%20image%2020260222014303.png)


<br>

### **Results**

RDP firewall rules are active on PC2. The system allows inbound Remote Desktop communication on TCP port 3389 inside the domain environment.

<br>

## **3.6 Configure Remote Desktop Permissions**

Remote Desktop access rights are configured on PC2. A domain security group is added to the local "Remote Desktop Users" group to allow domain users to log in remotely.

<br>

### **PC2 - Open Local Users and Groups**

```plaintext
lusrmgr.msc
```
![](images/Pasted%20image%2020260222014724.png)

<br>

*The Local Users and Groups console is opened to manage local group membership.*

<br>

### **PC2 - Add Domain Group to Remote Desktop Users**

```plaintext
Lab-Employees
```
![](images/Pasted%20image%2020260222015049.png)

<br>

*The domain security group "Lab-Employees" is selected from the lab.local domain.*

<br>

### **PC2 - Remote Desktop Users Group Membership**

```plaintext
LAB\Lab-Employees
```
![](images/Pasted%20image%2020260222015130.png)

<br>

*The domain group "LAB\Lab-Employees" is successfully added to the local "Remote Desktop Users" group.*

<br>

### **Results**

All members of the domain security group "Lab-Employees" are granted the right to log on remotely to PC2. Remote Desktop access is now properly configured using group-based permissions.

<br>

## **3.7 Remote Desktop Connection Test**

This section verifies successful Remote Desktop communication between PC1 and PC2 inside the lab.local domain. The connection uses domain credentials and confirms that authentication, firewall configuration, and network communication function correctly.

<br>

### RDP Client - PC1

```plaintext
Remote Desktop Connection
Computer: PC2
User name: LAB\t.svobodova
```
![](images/Pasted%20image%2020260222020102.png)

<br>


The Remote Desktop client on PC1 connects to PC2 using its hostname. Domain credentials are provided during authentication.

<br>

### **Credential Authentication**

```plaintext
LAB\t.svobodova
Password: ********
```
![](images/Pasted%20image%2020260222020153.png)

<br>

Domain authentication is performed using Network Level Authentication.



<br>


## **3.8 Verification Inside RDP Session**

After successful login, verification commands confirm that the session runs on the correct machine and under the correct domain account.

<br>

### User Verification

```plaintext
whoami
hostname
ipconfig
```
![](images/Pasted%20image%2020260222021020.png)

<br>


* The output confirms that the active session uses the domain account.
* The output confirms that the session runs on PC2.
* The IP address matches the configuration of PC2 in the lab network.


<br>

## **3.9 Conclusion**

Remote Desktop communication works correctly inside the domain environment. Domain authentication functions successfully over RDP, and firewall rules allow traffic on port 3389 without issues. User permissions are configured properly through the Remote Desktop Users group, which enables secure remote login.

The remote session confirms correct identity, hostname, and network configuration, which proves that the Remote Desktop setup inside the domain is fully operational.

<br>


---

<br>


**Back to Readme**: [README](README.md)
