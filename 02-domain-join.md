

# **2 - Domain Join**


<br>

## **2.1 Introduction**

This chapter focuses on joining two Windows 11 client computers (PC1 and PC2) to the lab.local domain. The goal is to connect both clients to the Domain Controller (SERVER-DC1) and integrate them into the Active Directory environment.

After the domain join process, domain user authentication is tested to confirm that centralized login works correctly. This step verifies communication between the clients and the Domain Controller.



<br>

## **2.2 Topology Diagram**

![](images/Pasted%20image%2020260222011733.png)

<br>


## **2.3 Objectives**


1. Rename the Domain Controller hostname to SERVER-DC1
    
2. Verify network connectivity between clients and SERVER-DC1
    
3. Confirm correct DNS configuration on PC1 and PC2
    
4. Join PC1 to the lab.local domain using GUI
    
5. Restart PC1 and test domain user login
    
6. Join PC2 to the lab.local domain using GUI
    
7. Restart PC2 and test domain user login
    
8. Verify domain membership and Active Directory computer objects



<br>


## **2.4 Domain Controller Renaming**

In this chapter, the Domain Controller is renamed to SERVER-DC1 for clear and simple naming.

In real environments, the server name should be set before installing Active Directory. Renaming a Domain Controller after installation is possible, but it is not the best practice.

*In this lab, the rename process is done correctly using PowerShell.*



### **SERVER-DC1 - Rename Command**

```plaintext
Rename-Computer -NewName SERVER-DC1 -Restart
```
![](images/Pasted%20image%2020260221235531.png)

This command renames the Domain Controller and automatically restarts the server to apply changes.



### **Results:**  

The Domain Controller hostname is successfully updated to SERVER-DC1. After restart, DNS records and Active Directory metadata reflect the new server name. The environment is ready for further verification and domain join operations.



<br>

## **2.5 Network Verification**

Before joining the clients to the domain, ICMP Echo Request (IPv4) inbound firewall rules are enabled on all three devices: SERVER-DC1, PC1, and PC2.

The rule "Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)" is set to Enabled and the Profile is configured to All. This ensures that ICMP traffic is allowed on Domain, Private, and Public profiles.

This configuration allows successful connectivity testing between all devices before the domain join process.

<br>

### **Firewall Configuration - SERVER-DC1**

```plaintext
Inbound Rule:
Core Networking Diagnostics - ICMP Echo Request (ICMPv4-In)
Profile: All
Enabled: Yes
```
![](images/Pasted%20image%2020260221232530.png)

<br>

### **Results**  

ICMP inbound rule is successfully enabled on SERVER-DC1.

*The same firewall configuration is applied on PC1 and PC2.*

<br>

### **Connectivity Verification**

ICMP connectivity is verified using the ping command on each device.

<br>

#### **SERVER-DC1**

-> Ping to PC1 (10.10.10.101)

```plaintext
ping 10.10.10.101
```
![](images/Pasted%20image%2020260221232646.png)

<br>

-> Ping to PC2 (10.10.10.102)

```plaintext
ping 10.10.10.102
```
![](images/Pasted%20image%2020260221232705.png)

<br>

#### **PC1**

-> Ping to SERVER-DC1 (10.10.10.10)

```plaintext
ping 10.10.10.10
```
![](images/Pasted%20image%2020260221232829.png)

<br>

-> Ping to PC2 (10.10.10.102)

```plaintext
ping 10.10.10.102
```
![](images/Pasted%20image%2020260221232841.png)

<br>

#### **PC2**

-> Ping to SERVER-DC1 (10.10.10.10)

```plaintext
ping 10.10.10.10
```
![](images/Pasted%20image%2020260221232913.png)

<br>

-> Ping to PC1 (10.10.10.101)

```plaintext
ping 10.10.10.101
```
![](images/Pasted%20image%2020260221232952.png)


<br>

### **Results**  

Successful replies confirm that ICMP communication works correctly between SERVER-DC1, PC1, and PC2. Network connectivity is fully operational before the domain join process.


<br>

## **2.6 DNS Verification**

Before joining the clients to the domain, DNS configuration must be verified. Active Directory relies on DNS for domain name resolution. If DNS is not configured correctly, the domain join process fails.

Both PC1 and PC2 must use the Domain Controller (10.10.10.10) as their primary DNS server.

<br>

### **PC1 - DNS Check**

-> Verify full network configuration

```plaintext
ipconfig /all
```
![](images/Pasted%20image%2020260221233418.png)

The output shows that the DNS Server is correctly configured as 10.10.10.10.

<br>

-> Verify domain name resolution

```plaintext
nslookup lab.local
```
![](images/Pasted%20image%2020260221233705.png)

<br>

-> Verify Domain Controller hostname resolution

```plaintext
nslookup SERVER-DC1.lab.local
```
![](images/Pasted%20image%2020260222000220.png)

<br>

### **PC2 - DNS Check**

-> Verify full network configuration

```plaintext
ipconfig /all
```
![](images/Pasted%20image%2020260221233826.png)

The output shows that the DNS Server is correctly configured as 10.10.10.10.

<br>

-> Verify domain name resolution

```plaintext
nslookup lab.local
```
![](images/Pasted%20image%2020260221233855.png)

<br>

-> Verify Domain Controller hostname resolution

```plaintext
nslookup SERVER-DC1.lab.local
```
![](images/Pasted%20image%2020260222000334.png)

<br>


### **Results**  

Successful name resolution confirms that DNS is configured correctly on PC1 and PC2. Both clients can resolve the lab.local domain and the SERVER-DC1 hostname. The environment is ready for the domain join process.


<br>

## **2.7 Join PC1 to lab.local Domain**

This next section documents the domain join process for PC1. The computer is renamed to a structured hostname and joined to the lab.local Active Directory domain using the domain administrator's **username and password**..

<br>
### **Configure Computer Name and Domain**


```plaintext
sysdm.cpl
```
![](images/Pasted%20image%2020260222001336.png)


_The computer name is changed to PC1 and the domain lab.local is specified. This prepares the workstation for domain membership._

<br>


```plaintext
Enter Domain Administrator Account
```
![](images/Pasted%20image%2020260222001456.png)


_Domain administrator credentials (LAB\Administrator) are entered to authorize the computer to join the domain._

<br>


```plaintext
Domain User Login
```
![](images/Pasted%20image%2020260222002926.png)


_After restart, the login screen shows that the system is signing in to the LAB domain. The user j.novak authenticates against Active Directory._


<br>

### **PowerShell Verification**

```plaintext
whoami
hostname
systeminfo | findstr /B /C:"Domain"
```
![](images/Pasted%20image%2020260222003133.png)

_PowerShell commands confirm that the logged-in user belongs to the lab domain, the hostname is PC1, and the system domain is correctly set to lab.local._


<br>

### Results

PC1 is successfully joined to the lab.local domain. Domain authentication works correctly, the hostname is properly configured, and the workstation is fully integrated into the Active Directory environment.


<br>

## **2.8 Join PC2 to lab.local Domain**

This section documents the domain join process for PC2. The workstation is renamed according to the lab naming standard and joined to the lab.local Active Directory using the domain administrator's **username and password**..

<br>

### **Configure Computer Name and Domain**


```plaintext
sysdm.cpl
```
![](images/Pasted%20image%2020260222005010.png)


_The computer name is changed to PC2 and the domain lab.local is specified. This prepares the workstation for domain membership._

<br>

```plaintext
Enter Domain Administrator Account
```
![](images/Pasted%20image%2020260222005044.png)


_Domain administrator credentials (LAB\Administrator) are entered to authorize PC2 to join the domain._

<br>

```plaintext
Domain User Login
```
![](images/Pasted%20image%2020260222005651.png)


_After restart, the login screen confirms that the system is signing in to the LAB domain. The user t.svobodova authenticates against Active Directory._

<br>


### **PowerShell Verification**


```plaintext
whoami
hostname
systeminfo | findstr /B /C:"Domain"
```
![](images/Pasted%20image%2020260222005951.png)

<br>

_PowerShell commands confirm that the logged-in user belongs to the lab domain, the hostname is PC2, and the system domain is correctly set to lab.local._

<br>

## Results

PC2 is successfully joined to the lab.local domain. Domain authentication works correctly, the hostname is properly configured, and the workstation is fully integrated into the Active Directory environment.



<br>

## **2.9 Server Final Verification**


<br>

### **Active Directory Computer Objects**

This last section verifies from the server side that both workstations are properly joined to the lab.local domain. The verification is performed using Active Directory Users and Computers.

<br>

```plaintext
Active Directory - Computer Objects Verification
```
![](images/Pasted%20image%2020260222010513.png)


_The Active Directory console shows that both PC1 and PC2 are present as computer objects inside the domain. This confirms successful domain join and secure channel creation between the clients and the Domain Controller._

<br>

### **Results**

Both PC1 and PC2 are successfully registered in Active Directory. The domain environment is fully operational, and all client machines are correctly integrated into the lab.local domain.


<br>



## **Conclusion**

In this chapter, the Domain Controller is renamed and DNS is verified. Both PC1 and PC2 are successfully joined to the **lab.local** domain. Domain user login and PowerShell verification confirm that the environment is working correctly.

Active Directory now contains both computer objects, and the domain is fully operational. The next chapter focuses on configuring and testing Remote Desktop between domain-joined systems.

<br>


---

<br>

**Final Chapter:**


