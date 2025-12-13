# Lab-3-Azure
Installing and Using a packet sniffer(WireShark) to monitor Simple Network Traffic in a subnet 
<h1>WireShark installation and usage</h1>

<h2>Description</h2>
How to install and use a packet sniffer(WireShark) with the ping command to monitor Simple Network Traffic in a subnet 
<br />


<h2>Utilities Used</h2>

- <b>Azure</b> 
- <b>WireShark</b> 
- <b>Powershell</b> 

<h2>Environments Used </h2>

- <b>Windows 11</b> (25H2)

<h2>Prerequisites </h2>

- <b>An Azure Account/Subscription (paid or free)</b>
- <b>Pre-Created Resource Group</b> (See Lab 1: <a href="https://github.com/Azure-Labs-IT/Lab-1-Azure/blob/main/README.md"> How to create Resource Group</a>)
- <b>Pre-Created Virtual Machines in the same resource group and subnet. One Linux Virtual Machine and One Windows Virtual Machine</b> <br/>
    (See Lab 1: <a href="https://github.com/Azure-Labs-IT/Lab-1-Azure/blob/main/README.md"> How to create a Virtual Machine(s)</a>)

<h2>Labs walk-through:</h2>

<p align="center">
On your Windows Virtual Machine go to <a href="https://www.wireshark.org/download.html">Wireshark's Website</a> and download and install Wireshark for your Windows Operating System: <br/>
<img src="https://github.com/Azure-Labs-IT/Lab-3/blob/main/1.%20Download%20and%20install%20wireshark%20for%20your%20OS%20on%20the%20Windows%20VM.png" height="80%" width="80%" alt="Choose right Wireshark install file for your OS"/>
<img src="https://github.com/Azure-Labs-IT/Lab-3/blob/main/1.%20Download%20and%20install%20WireShark.png" height="80%" width="80%" alt="Download and Install Wireshark"/>
<br />
<br />
</p>

<p align="center">
🔴VERY IMPORTANT!!: <br/> 
Click on Install NPCAP during installation. This should be enabled on Windows because it's the essential packet capture driver that allows Wireshark to see and capture live network traffic, putting your network interface into "promiscuous mode" to see all data, not just data addressed to your machine. Without Npcap, Wireshark can only open saved capture files (like .pcap or .pcapng) but can't sniff packets directly from the network. So basically without it enabled, Wireshark cannot "see" any real-time network traffic and can only be used to analyze pre-saved capture files stored on your pc
<b>(If this is not enabled, Wireshark cannot access your network adapter to initiate a live capture session. The interface list will show a "Local interfaces are unavailable because no packet capture driver is installed" error, and no packets can be captured live. The NPCAP tells the network adapter to go into acccept all traffic mode and thus allowing wireshark to see the live network traffic.)
</b> 
 <br />
<img src="https://github.com/Azure-Labs-IT/Lab-3/blob/main/2.%20Make%20sure%20to%20enable%20NPCAP%20during%20installation%2C%20this%20%20allows%20Wireshark%20to%20access%20and%20analyze%20network%20traffic%20effectively.%20USBcap%20is%20not%20essential.png" height="80%" width="80%" alt="Enable NPCAP"/>
<br />
<br />
</p>

<p align="center">
Launch Wireshark after installation:  <br/>
<img src="https://github.com/Azure-Labs-IT/Lab-3/blob/main/3.%20Launch%20Wireshark.png" height="80%" width="80%" alt="Launch Wireshark"/>
<br />
<br />
</p>


<p align="center">
You should see a list of Network adapters if not one .Look for ethernet if there are more than one, it might be different for you but look for an interface that's showing traffic movement (the up and down lines are traffic)<br/>
<img src="https://github.com/Azure-Labs-IT/Lab-3/blob/main/4.%20Look%20for%20ethernet%20on%20Wireshark%2C%20it%20might%20be%20different%20for%20you%20but%20look%20for%20an%20interface%20that's%20showing%20traffic.png" height="80%" width="80%" alt="Look for the/a Network adapter with traffic"/>
<br />
<br />
</p>

<p align="center">
Double-Click on Ethernet or click on it and then click on the Shark Icon to observe the traffic flowing through the interface:  <br/>
<img src="https://github.com/Azure-Labs-IT/Lab-3/blob/main/5.%20Double-Click%20on%20Ethernet%20or%20click%20on%20it%20and%20then%20click%20on%20the%20Shark%20to%20observe%20the%20traffic%20flowing%20through%20the%20interface.png" height="80%" width="80%" alt="Click on Network adapter and then Click on Shark Icon"/>
<br />
<br />
</p>

<p align="center">
You should see the traffic and individual packets visible to that adapter and on the network from here. This includes their port number and what protocol they/the packets use. The Port Number for the/a packet will be visible under the "Info" Column and the Protocol the packet used will be visible under the "Protocol" column. The IP address of Where the Packet came from otherwise known as the source IP address will be under the "Source" column and the Destination/IP address the packet is going to will be visible under the "Destination" column:  <br/>
<img src="https://github.com/Azure-Labs-IT/Lab-3/blob/main/6.%20You%20should%20see%20the%20traffic%20for%20that%20adapter%20from%20here.%20This%20includes%20their%20port%20number%20and%20what%20protocol%20they%20use.png" height="80%" width="80%" alt="View Packets and Packet details"/>
<br />
<br />
</p>

<p align="center">
You can filter the packets by their protocols/Port number of the protocol in order to refine packet view, since we are going to inspect traffic use icmp. Keep in mind the search bar is caps sensitive so the text must be in small caps.:  <br/>
<img src="https://github.com/Azure-Labs-IT/Lab-3/blob/main/7FILTE~1.PNG" height="80%" width="80%" alt="filter packets by protocol"/> <br/>
</p>

<p align="center">
Since we are going to be observing the traffic between Two Virtual Machines on a Subnet we will need to test the responsiveness of the Linux virtual machine by pinging it's private IP address via the Windows Virtual Machine PowerShell tool. To find the Private IP address of the Linux Virtual Machine go to your Azure Account under your virtual Machines, select the Pre-created Linux Virtual Machine. Under the Virtual Machines Network/Networking Info ,copy the Private IP address for the Linux Virtual Machine and paste it in Powershell of the Windows Virtual Machine after the ping command (for example ping 173.4.13.0) and press enter. The ping process should show in Powershell. Since we already filtered based on the ICMP Protocol you should now only see the packets being exchanged between the two Virtual Machines displayed on Wireshark:<br/>
<img src="https://github.com/Azure-Labs-IT/Lab-3/blob/main/8COPYT~1.PNG" height="80%" width="80%" alt="Copy private IP and Ping"/>
<br />
<br />
</p>

<p align="center">
Now let's look into what each packet entails .Under "Ethernet Ⅱ" you will find Layer 2 info like the Mac addresses. The Mac address will switch depending on the type of packet it was. The packet types are either request or reply. Request type being the Windows Virtual Machine sending the ping request to the Linux Virtual Machine making it the Source Mac Address as the packet came from it and the Reply type being the response received from the Linux Virtual making it the destination Mac Address:  <br/>
<img src="https://github.com/Azure-Labs-IT/Lab-3/blob/main/9.%20Under%20Ethernet%202%20you%20will%20find%20Layer%202%20info%20like%20the%20Mac%20address.%20It%20will%20switch%20depending%20on%20the%20type%20of%20packet%20it%20was%2C%20request%20or%20reply.png" height="80%" width="80%" alt="Ethernet 2 Info and what it has"/>
<br />
<br />
</p>

<p align="center">
Under "Internet Protocol Version" you will find Layer 3 info like IP addresses for the source and destination IP addresses. These will switch depending on the packet type, either request or reply. Request type being the Windows Virtual Machine sending the ping request to the Linux Virtual Machine making it the Source IP as the packet came from it and the Reply type being the response received from the Linux Virtual making it the destination IP :  <br/>
<img src="https://github.com/Azure-Labs-IT/Lab-3/blob/main/10.%20Under%20Internet%20Protocol%20Version%20you%20will%20find%20Layer%203%20info%20like%20IP%20addresses%20for%20the%20source%20and%20destination.%20These%20will%20switch%20depending%20on%20the%20packet%20type.png" height="80%" width="80%" alt="Internet protocol version and what it is"/>
<br />
<br />
</p>

<p align="center">
11. Under "Internet Control Message Protocol" you will see the data that is in the packet also known as the payload and be able to analyze/read/see what was sent in the case of ping it is a simple abcdef.. text. And the reason we can see the info in the payload is because ping transfers packets without encryption which means anyone who can access the packets can see what they entail:  <br/>
<img src="https://github.com/Azure-Labs-IT/Lab-3/blob/main/11.%20Under%20Internet%20Control%20Protocol%20you%20will%20see%20the%20payload%20and%20be%20able%20to%20analyze%20what%20was%20sent%20in%20the%20case%20of%20ping%20it%20is%20a%20simple%20abc...png" height="80%" width="80%" alt="Internet control message protocol and what it is"/>
<br />
<br />
</p>


<p align="center">
<b> And you're done now ,you have now successfully observed simple traffic between two virtual machines in the a subnet. Don't forget to disable/stop your virtual machines on Azure once done(remember you pay as you use): </b>  <br/>
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>
