<p align="center">
<img src="https://i.imgur.com/Ua7udoS.png" alt="Traffic Examination"/>
</p>

<h1>Network Security Groups (NSGs) and Inspecting Traffic Between Azure Virtual Machines</h1>
In this tutorial, we observe various network traffic to and from Azure Virtual Machines with Wireshark as well as experiment with Network Security Groups. <br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Various Command-Line Tools
- Various Network Protocols (SSH, RDH, DNS, HTTP/S, ICMP)
- Wireshark (Protocol Analyzer)

<h2>Operating Systems Used </h2>

- Windows 10 (22H2)
- Ubuntu Server 20.04

<h2>High-Level Steps</h2>

- Create Resources
- Observe ICMP Traffic
- Observe SSH Traffic
- Observe DHCP Traffic
- Observe DNS Traffic
- Observe RDP Traffic
- Cleanup Resources

<h2>Actions and Observations</h2>

<h3>Create Resources</h3>

<p>
<img src="https://i.imgur.com/lAoqcOl.png" height="80%" width="80%" alt="VM1 Creation"/>
</p>
<p>
Let's start this lab off by going into Azure and creating our resources. This will involve the creation of 2 virtual machines. One with Windows 10 and another with Ubuntu. Pictured above you will see that I created a resource group called "RG-Lab" and selected the Windows 10 Pro image. I left the rest of the settings as default, but I will be reusing the vnet that gets created for our second virtual machine (VM2). Pictured below, you will see that vnet (VM1-vnet).

</p>
<br />

<p>
<img src="https://i.imgur.com/HGB9eWS.png" height="80%" width="80%" alt="Ubuntu VM2 creation"/>
</p>

<p>
<img src="https://i.imgur.com/oPmgUFe.png" height="80%" width="80%" alt="Download Wireshark"/>
</p>
<p>
Pictured above is the RDP connection to Windows VM (VM1) where we're downloading Wireshark from the web. Once it is installed, we are ready to start observing some traffic. 
</p>
<br />

<h3>Observe ICMP Traffic</h3>

<p>
<img src="https://i.imgur.com/dcj0HbJ.png" height="80%" width="80%" alt="Wireshark ICMP"/>
</p>
<p>
Now inside of Wireshark you can see that I have highlighted the shark icon near the top left corner. This will start observing all of the ongoing network traffic. Just under that icon, you can type in "ICMP" to filter out only ICMP traffic. Now we will open command prompt and ping VM2 (the other virtual machine we created with the Ubuntu image) using the private ip address that can be found by navigating to VM2 in the Azure portal. As pictured, you will see the packets of data sent to the virtual machine.
</p>
<br />

<p>
<img src="https://i.imgur.com/1nXjLTw.png" height="80%" width="80%" alt="Perpetual ping"/>
</p>
<p>
Let's play with some firewall rules. First, we will run the "ping -t" command followed by VM2's private ip address. This will create a perpetual ping that won't end until we tell it to stop. Once that's been done, we'll open up Network Security Groups inside of the Azure portal and create an inbound rule that denies any ICMP traffic. If you go back to VM1 and watch the traffic, you will see that our requests are timing out and we are receiving no replies.
</p>
<br />

<p>
<img src="https://i.imgur.com/1yQq9R7.png" height="80%" width="80%" alt="Inbound Rules"/>
</p>

<p>
<img src="https://i.imgur.com/qzY3MU0.png" height="80%" width="80%" alt="Request timeouts"/>
</p>
<br />
<h3>Observe SSH Traffic</h3>

<p>
<img src="https://i.imgur.com/udnuRNG.png" height="80%" width="80%" alt="SSH"/>
</p>
<p>
Let's observe some SSH traffic now. Start by filtering Wireshark by ssh traffic. Open up command prompt and type "ssh [username]@[vm2 ip address]". As you can see I was prompted to enter the password for VM2. Type commands (cd, pwd, ls, etc) to examine the traffic in Wireshark. You can exit the connection by simply typing "exit" and hitting enter. 
</p>
<br />

<h3>Observe DHCP Traffic</h3>

<p>
<img src="https://i.imgur.com/hIT2QLJ.png" height="80%" width="80%" alt="DHCP"/>
</p>
<p>
Now let's do some DHCP traffic. Start by filtering Wireshark by DHCP. Attempt to assign your VM a new ip address by typing "ipconfig /renew" in the command prompt. Notice the traffic in Wireshark. 
</p>
<br />

<h3>Observe DNS Traffic</h3>

<p>
<img src="https://i.imgur.com/UKLcxsP.png" height="80%" width="80%" alt="DNS"/>
</p>
<p>
Back in Wireshark, filter for DNS traffic only. Fro VM1 within the command line, use nslookup to see what google.com and disney.com’s IP addresses are. Observe the DNS traffic being show in WireShark
</p>
<br />

<h3>Observe RDP Traffic</h3>

<p>
<img src="https://i.imgur.com/fnt7dEb.png" height="80%" width="80%" alt="RDP"/>
</p>
<p>
Back in Wireshark, filter for RDP traffic only (tcp.port == 3389). Notice that the traffic is spamming. This is because there is a constant connection between our pc and the virtual machine that is being transmitted. 
</p>
<br />

<h3>Clean up Resources</h3>

<p>
That wraps up this lab. When you are done, make sure to delete all of your resources in Azure to ensure that you don't get a surprise bill at the end of the month. 
</p>
<br />
