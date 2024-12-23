<p align="center">
  <img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this home lab experiment, I analyzed network traffic patterns across multiple protocols including ICMP, SSH, RDP, and DNS. Using Wireshark for packet capture and PowerShell for automation, I observed network behaviors between Windows and Linux virtual machines..<br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDP, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used</h2>

- Windows 10 (21H2)
- Ubuntu Server 20.04

<h2>Actions and Observations</h2>

<h3>Part 1: Create Our Resources</h3>
1. Create a Resource Group.
2. Create a Windows 10 Virtual Machine (VM).
3. While creating the VM, select the previously created Resource Group:  

![image](https://github.com/user-attachments/assets/408e9ccb-623c-4b7f-9bf3-0a4b57164720)
 
   
4. Allow the creation of a new Virtual Network (Vnet) and Subnet:  

![image](https://github.com/user-attachments/assets/bb67a3a3-49c4-4607-82b0-88c8873fbf23)

  
5. Create a Linux (Ubuntu) VM:

![image](https://github.com/user-attachments/assets/b4881d6e-98ba-4207-8107-d6cd46166c17)

  
6. While creating the VM, select the previously created Resource Group and Vnet:  
 
![image](https://github.com/user-attachments/assets/3751d075-3f68-48d4-a14c-51038ba0228c)

![image](https://github.com/user-attachments/assets/368338ec-7942-45e1-bfc4-23fdf555416e)

   
7. Observe your Virtual Network within Network Watcher.

<h3>Part 2: Observe ICMP Traffic</h3>
1. Use Remote Desktop to connect to your Windows 10 Virtual Machine.
  
  ![image](https://github.com/user-attachments/assets/fe36038c-337f-4ff9-a410-9a62dcbe5094)

2. Install Wireshark on the Windows 10 VM.
 
![image](https://github.com/user-attachments/assets/8d815bbb-f070-4810-901b-5792d531b4c7)

3. Open Wireshark and filter for ICMP traffic only.  
  
![image](https://github.com/user-attachments/assets/1c21c157-d05f-4875-a63e-e65ef2b48f74)

4. Retrieve the private IP address of the Ubuntu VM and attempt to ping it from within the Windows 10 VM.
   - Observe ping requests and replies within Wireshark.  
 
![image](https://github.com/user-attachments/assets/14535019-3bd2-438c-a930-a4b83b950e54)
 
   
5. Initiate a perpetual/non-stop ping from your Windows 10 VM to your Ubuntu VM.  

 ![image](https://github.com/user-attachments/assets/95c30475-ce66-4c4e-b3b8-6480fd88f129)

   - Open the Network Security Group (NSG) associated with your Ubuntu VM and disable incoming (inbound) ICMP traffic.  
  
![image](https://github.com/user-attachments/assets/74c652bc-8e40-4ed2-b50b-9129651c2b5f)

![image](https://github.com/user-attachments/assets/7f008296-aedb-4d4e-81e3-0c5c15812843)


   - Back in the Windows 10 VM, observe the ICMP traffic in Wireshark and the command-line ping activity.
   - Re-enable ICMP traffic for the Network Security Group your Ubuntu VM is using.
   - Back in the Windows 10 VM, observe the ICMP traffic in Wireshark and the command-line ping activity (it should resume).
   - Stop the ping activity.

<h3>Part 3: Observe SSH Traffic</h3>
1. In Wireshark, filter for SSH traffic only.
2. From your Windows 10 VM, "SSH into" your Ubuntu VM (via its private IP address).
   - Type commands (username, password, etc.) into the Linux SSH connection and observe SSH traffic in Wireshark.
   - Exit the SSH connection by typing 'exit' and pressing [Enter].  
  
   ![image](https://github.com/user-attachments/assets/5c49d46c-6f7d-429b-a15b-c49e6a2811b7)

<h3>Part 4: Observe DHCP Traffic</h3>
1. In Wireshark, filter for DHCP traffic only.
2. From your Windows 10 VM, attempt to issue your VM a new IP address using the command `ipconfig /renew` and observe the DHCP traffic in Wireshark.  

![image](https://github.com/user-attachments/assets/9b6dce5b-c7ed-4cce-bb36-7363c033c943)

<h3>Part 5: Observe DNS Traffic</h3>
1. In Wireshark, filter for DNS traffic only.
2. From your Windows 10 VM, within the Command Line, use `nslookup` to resolve a website's IP address and observe the DNS traffic in Wireshark.  
  
![image](https://github.com/user-attachments/assets/9f4346ee-74f8-425d-9479-487e584e02ce)

<h3>Part 6: Observe RDP Traffic</h3>
1. In Wireshark, filter for RDP traffic only (`tcp.port == 3389`).
2. Observe the immediate non-stop traffic. Why is it non-stop versus only showing traffic when an activity occurs?

![image](https://github.com/user-attachments/assets/1caacf25-bad0-4e0c-b2dc-08ce01b14416)
   
   RDP traffic is constant because it streams the live display of one computer to another. Unlike SSH, where traffic is generated only when keystrokes are sent, RDP transmits a constant stream of data for the remote session.  

   

<h2>Takeaways and Key Skills Developed</h2>
In this lab, I explored network security and traffic monitoring between Azure VMs using Wireshark and PowerShell. I set up both Windows and Ubuntu VMs to observe ICMP, SSH, DNS, RDP, and DHCP traffic. Through Wireshark's filtering capabilities, I learned how different protocols behave—watching ICMP during ping tests, SSH during remote connections, and RDP during remote desktop sessions. I also worked with Network Security Groups (NSGs) to control traffic flow, including blocking ICMP and monitoring its effects in real time. This hands-on experience deepened my understanding of network security practices, traffic analysis, and cloud security configuration.
