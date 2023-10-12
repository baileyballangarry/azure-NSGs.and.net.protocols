<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups and Inspecting Traffic Between Azure Virtual Machines</h1>

The objective of this lab is to enhance our comprehension of various web protocols and the mechanisms through which machines communicate over a network. In this lab, we will observe and analyze three distinct forms of network traffic, specifically ICMP, DHCP, and DNS.<br />

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
For this setup, you should create two virtual machines - one running Windows 10 and the other running Linux. Ensure that both of these VMs are placed within the resource group we've just established.

The first VM, named "Windows-VM," will run "Windows 10 Pro." Set the username as "Labuser," with the password specified as "Password1234."
</p>

<p>
<img src="https://i.imgur.com/uv3viED.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
<br />

<p>
The second virtual machine should be named "Linux-VM" and will operate on "Ubuntu Server 20.04 LTS." The username to access this machine will be "Labuser," and the password will be set as "Password1234." It's worth noting that using the same username and password for both virtual machines is feasible because they are distinct machines, each having its unique IP address when accessed.
</p>

<p>
<img src="https://i.imgur.com/FyKrVLk.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>
Please take note that when we initially set up our first virtual machine, "Windows-VM," an associated virtual network named "Windows-VM-Net" was automatically generated. Therefore, when creating the second virtual machine, it's important to navigate to the networking settings and confirm that the previously created virtual network is selected.





</p>

<p>
<img src="https://i.imgur.com/vbNeczg.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
<br />

<p>
<b>3.	Remote Desktop into the windows virtual machine.</b>
  

In the Azure portal, locate your Windows virtual machine and copy its public IP address. Launch Remote Desktop Connection on your personal computer and paste the IP address of the Windows VM. Sign in to the VM using the username and password established during the initial VM setup.
</p>

<p>
<img src="https://i.imgur.com/E4C7FDQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
<br />

<p>
<b>4.	Download and install Wireshark. </b>
  
After accessing your Windows VM, use a web browser to obtain the Windows 10 64-bit Wireshark installer. Install the software from the downloaded file located in your downloads folder. Once Wireshark has been successfully installed, launch the program. While inside Wireshark, select "Ethernet," and then click the blue shark fin icon located in the top-left corner of the Wireshark interface.
</p>

<p>
<img src="https://i.imgur.com/S4f1Um7.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
<br />

<p>
  <b>We will use Wireshark to observe ICMP, DHCP, and DNS traffic.</b>

Now, let's set up a filter to display only ICMP traffic. To achieve this, navigate to the search bar located at the top of Wireshark and search for "ICMP," then hit the Enter key. This action will filter Wireshark to display exclusively the ICMP traffic occurring over the network, as opposed to all traffic.
</p>

<p>
<img src="https://i.imgur.com/LDrCUDX.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>
  
<br />

<p>
It's crucial to keep in mind that ICMP stands for "Internet Control Messaging Protocol," the protocol used by the "ping" command. When we initiate pinging another machine, we will observe the ICMP traffic within Wireshark.
</p>
  
<br />

<p>
<b>5.	“Ping” to observe ICMP traffic.</b>
  
Retrieve the private IP address of the Linux virtual machine within Azure. In this illustration, the private IP address of the Linux virtual machine is designated as "10.0.0.5."
</p>

<p>
<img src="https://i.imgur.com/VM2nyzQ.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>
Inside the Windows VM, launch the command prompt and initiate a ping to the Linux VM by entering the command "ping 10.0.0.5." You should receive an immediate reply. If we examine Wireshark, we can observe the traffic generated. It displays the source IP (Windows VM), destination IP (Linux VM), protocol (ICMP), and relevant information (request and reply).

Please be aware that the IP address of your virtual machine may differ from the one used in this example.

</p>

<p>
<img src="https://i.imgur.com/1k7UcrF.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>
<b>6.	Modify incoming security rules for the Linux VM</b>
  
Return to Azure and utilize the search bar to locate "network security groups." Access the network security group associated with the Linux virtual machine. Then, proceed to the "Inbound rules" section and generate a new rule. This rule should be configured to reject inbound ICMP traffic. Choose ICMP, select "Deny," set the priority to 200, and click "Add."
</p>

<p>
<img src="https://i.imgur.com/W7jdODf.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>
Wait for several minutes to allow the rule to update, and then go back to the Windows VM. Attempt to ping the Linux VM once more using the command "ping 10.0.0.5." You will notice that the ping request to the Linux machine will time out. As previously mentioned, this occurs because the "ping" command uses ICMP (Internet Control Messaging Protocol). By adjusting the inbound rules in the network security group of the Linux virtual machine and denying inbound ICMP traffic, we have effectively restricted the capability of other machines to ping the Linux machine.
</p>

<p>
<img src="https://i.imgur.com/FTlFLI6.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>

To reverse these modifications, you can return to Azure and either delete the rule that was recently created or adjust it to permit incoming ICMP traffic.
</p>

<br />

<p>
<b>7.	“Ipconfig /renew” to Observe DHCP traffic</b>
  

Head back to the Windows VM and launch Wireshark. In the search bar, search for "DHCP" and hit Enter.

It's worth noting that DHCP stands for "Dynamic Host Configuration Protocol," which is the protocol responsible for assigning IP addresses to computers. When a computer requires an IP address, it sends a request to the DHCP server, and the DHCP server responds by providing an IP address.

We can observe this by typing the command “ipconfig /renew”
</p>

<p>
<img src="https://i.imgur.com/68XHciA.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>
<b>8.	“nslookup” to Observe DNS Traffic</b>
  

Go back to the Windows VM and launch Wireshark. In the search bar, search for "DNS" and hit Enter.

It's crucial to recall that DNS stands for "Domain Name System," which is the protocol responsible for mapping IP addresses to websites.

The DNS protocol translates a name, such as "www.google.com," into the corresponding IP address by interacting with various servers.

We can witness this process by using the "nslookup" command with a website name like Google, YouTube, or Disney. The complete command would appear as "nslookup www.google.com."
</p>

<p>
<img src="https://i.imgur.com/vv5xoRM.png" height="80%" width="80%" alt="Disk Sanitization Steps"/>
</p>

<br />

<p>
<b>9.	Clean-up Azure</b>
  
After you have concluded your exploration of various web traffic types, please revisit Azure and remove all the resources we established. Leaving these resources running can result in ongoing costs, even without your active use.

Throughout this lab, we have strived to enhance our comprehension of various web protocols and examine their associated traffic with the aid of Wireshark. Additionally, we have gained insights into how different network security group regulations can influence web traffic by allowing or denying specific types of traffic.
</p>
