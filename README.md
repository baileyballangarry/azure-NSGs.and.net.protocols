<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups and Inspecting Traffic Between Azure Virtual Machines</h1>
This lab aims to aid in our understanding of different types of web protocols and how machines interact with each other over a network. We will observe three different types of traffic in this lab being; ICMP, DHCP, and DNS. <br />

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines)
- Remote Desktop Connection
- Various Command-Line Tools
- Various Network Protocols (DNS, DHCP, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create a Resource Group. 
- Create two Virtual Machines. 
- Remote Desktop into the windows virtual machine. 
- Download and install Wireshark. 
- “Ping” to observe ICMP traffic.
- Modify incoming security rules for the Linux VM
- “Ipconfig /renew” to Observe DHCP traffic
- “nslookup” to Observe DNS Traffic
- Clean-up Azure

<h2>Actions and Observations</h2>

<b>1. Create a Resource Group.</b>
  
<p>
This is where we will store our two virtual machines. Go to “Azure -> resource groups -> create”
</p>

<p>
<img src="https://i.imgur.com/C7TmwAn.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
<br />

<b>2.	Create two Virtual Machines.</b>
  
<p>
One should be a windows 10 Virtual Machine, the other should be a Linux virtual machine. When creating these VMs, place them both inside the resource group that we just created. 

The first VM will be called “Windows-VM” and will run “Windows 10 Pro.” Username will be “Labuser” and the password will be “Password1234”
</p>

<p>
<img src="https://i.imgur.com/uv3viED.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
<br />

<p>
The second VM will be called “Linux-VM” and will run “Ubuntu Server 20.04 LTS.” Username will be “Labuser” and the password will be “Password1234” The reason we can use the same username and password on both virtual machines is because they are two separate machines, thus having their own unique IP Address when we connect to them.
</p>

<p>
<img src="https://i.imgur.com/FyKrVLk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>
Note: when we created our first virtual machine, being “Windows-VM” a virtual network was automatically created called “Windows-VM-Net”. With this in mind, when we create the second virtual machine we must go to the networking page and check that the virtual network that was created is selected. 
</p>

<p>
<img src="https://i.imgur.com/vbNeczg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
<br />

<p>
<b>3.	Remote Desktop into the windows virtual machine.</b>
  
Within Azure, navigate to your windows virtual machine and copy Its public IP address. Open Remote Desktop Connection on your PC and paste the IP address of the windows VM. Log in to the VM using the username and password that we created when initially creating the VM.
</p>

<p>
<img src="https://i.imgur.com/E4C7FDQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
<br />

<p>
<b>4.	Download and install Wireshark. </b>
  
Once you are inside your Windows VM, open a web browser and download the Windows 10 64bit Wireshark installer. Install the software from the file in your downloads folder. Once Wireshark is installed, open the program. Once inside Wireshark, select “Ethernet” and then press the blue shark fin button in the top left of Wireshark. 
</p>

<p>
<img src="https://i.imgur.com/S4f1Um7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
<br />

<p>
  <b>We will use Wireshark to observe ICMP, DHCP, and DNS traffic.</b>

We will now filter for ICMP traffic only. To do this, go to the search bar at the top of Wireshark and search for “ICMP” then press enter. This will make it so that Wireshark only shows us ICMP traffic that is happening over the network, as opposed to all traffic.
</p>

<p>
<img src="https://i.imgur.com/LDrCUDX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
<br />

<p>
It is important to remember that ICMP stands for “Internet Control Messaging Protocol” which is the protocol that ping uses. When we begin pinging another machine, we will see the ICMP traffic in Wireshark.
</p>
  
<br />

<p>
<b>5.	“Ping” to observe ICMP traffic.</b>
  
Obtain the private IP address of the linux virtual machine within azure. For this example, the private IP address of the linux virtual machine is “10.0.0.5”.
</p>

<p>
<img src="https://i.imgur.com/VM2nyzQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>
Within the Windows VM, open command prompt and ping the linux VM with the command “ping 10.0.0.5” you should automatically get a reply. If we look at Wireshark, we can see the traffic that we just generated. We see the source IP (Windows VM) the destination IP (Linux VM) the protocol (ICMP) and information (request and reply).

Note. The IP address of your virtual machine may be different to the IP address I have used in this example. 

</p>

<p>
<img src="https://i.imgur.com/1k7UcrF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>
<b>6.	Modify incoming security rules for the Linux VM</b>
  
Go back to Azure and search for network security groups via the search bar. Open the network security group for the linux virtual machine. Select inbound rules and create a new rule. This rule should be to deny inbound ICMP traffic. Select ICMP, Select Deny, Set the priority to 200, and press Add.
</p>

<p>
<img src="https://i.imgur.com/W7jdODf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>
Wait several minutes for the rule to update and return to the Windows VM. Ping the Linux VM again using “ping 10.0.0.5” we should see that the ping request to the linux machine will time out. Remembering back to before, this is because “ping” uses ICMP, (Internet control messaging protocol) so by modifying the inbound rules on the network security group of the linux virtual machine and denying inbound ICMP traffic we have effectively denied the ability for other machines to ping the linux machine.
</p>

<p>
<img src="https://i.imgur.com/FTlFLI6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>
We can revert these changes by returning to Azure and either deleting the rule we just created, or modifying it to allow incoming ICMP traffic.
</p>

<br />

<p>
<b>7.	“Ipconfig /renew” to Observe DHCP traffic</b>
  
Return to the Windows VM and open Wireshark. Search for DHCP in the search bar and press enter. 

It is important to remember DHCP stands for “Dynamic Host Configuration Protocol” which is the protocol responsible for issuing IP Addresses to computers. When a computer needs an IP address, it will send a request to the DHCP server, and the DHCP server will respond by issuing an IP address. 

We can observe this by typing the command “ipconfig /renew”
</p>

<p>
<img src="https://i.imgur.com/68XHciA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>
<b>8.	“nslookup” to Observe DNS Traffic</b>
  
Return to the Windows VM and open Wireshark. Search for DNS in the search bar and press enter.
It is important to remember DNS stands for “Domain Name System” which is the protocol responsible for matching IP addresses to websites.

DNS protocol takes a name such as “www.google.com” and will find the relevant IP address for that website by talking to a range of different servers. 

We can observe this by typing the command “nslookup” and a website name such as google, YouTube, or Disney. The full command should look like “nslookup www.google.com”
</p>

<p>
<img src="https://i.imgur.com/vv5xoRM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>
<b>9.	Clean-up Azure</b>
  
Once you have finished experimenting with different types of web traffic you should return to Azure and delete all the resources we created. Leaving them running will continue to cost money as they will run in the background without you even realising. 

In this lab, we have worked to understand diverse web protocols and scrutinize their traffic using Wireshark. Furthermore, we have acquired knowledge about how distinct security regulations on network security groups can impact web traffic by granting or denying certain forms of traffic.
</p>
