<h1>Active Directory Home Lab</h1>

<h2>Description</h2>

In this lab I utilized Oracle Virtual Box to create an Active Directory home lab to better understand the inner workings of active directories and to expand on my knowledge of Window's networking.

<h3>Step 1 - Download Oracle Virtual Box</h3>

Link: https://www.virtualbox.org/wiki/Downloads

Make sure to pick the correct OS and following the Oracle VM download, make sure to download the Extension Pack under "VirtualBox 7.0.14 Oracle VM VirtualBox Extension Pack"

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/1c7677dc-3372-4e4b-829d-c493928e5dde)

<h3>Step 2 - Download Windows 10 Iso</h2>
Link: https://www.microsoft.com/en-us/software-download/windows10

Link: https://www.microsoft.com/en-us/evalcenter/evaluate-windows-server-2019

Follow the link to download the 64 bit Iso file for Windows 10. Make sure to save the file somewhere easy to find (recommend just saving it to your desktop). Then follow the second link to download the Windows 2019 Server Iso file. 

<h3>Step 3 - Create Windows 2019 Server Virtual Machine</h3>
Next, we're going to start creating our first VM. You're going to hit the "New" button. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/205af6b6-6c63-4fba-ac89-054fca5cc76f)

Go ahead and name the VM. I followed Josh Madakor's video and he recommended just naming it "DC" for Domain Controller. Then select your ISO image from your desktop and make sure to pick one of the "Desktop Experience" options in order to ensure you have a GUI upon creation. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/9881ff40-1e18-44b8-9bbf-4d9e38bc496a)

Next, you'll configure the Hardware settings for the VM by choosing the base memory and number of processors. I went ahead and put 4096MB of memory and 4CPUs. Click "next" when  you get to Virtual Hard Disk and then Finish. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/04ccc50d-c5b6-4576-b23e-c55748e328ef)

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/376b719d-ea6d-4b60-a219-e749067adaa5

<h3>Step 4 - Configure DC Machine Basics</h3>

Once the machine is created; before you power it up, click on the machine in Oracle VM and then click settings. On the general page, click the "Advanced" tab and under Shared Clipboard and Drag'n'Drop, choose the "Bidirectional" option. This will allow us to copy and paste and/or drag files from our main desktop to our VMs. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/3e7f368b-4f08-4d50-b92e-12a0792a903d)

Next, you'll go down to "Network" and, since we're creating our domain controller, so we want to have 2 NICs: 1 running NAT and another running to the Internal Network. Click on Adapter 2 and check the "Enable Network Adapter" box. For the "Attached to:" drop down, select Internal Network and then press OK. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/02910cfe-268e-4dde-bc8b-5cc7707c25fe)

If you notice that your  mouse and VM screen is laggy or unresponsive, you can go to "Devices" and select "Insert Guest Additions CD Image". Then go to your file directory and look for the CD Drive (D:) VirtualBox Guest Additions, then find the VBoxWindowsAdditions-amd64 file and run it. You can restart when prompted or manually reboot on your own. You should notice the QoL change immediately upon restart. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/7239ad7a-9bb7-413e-9c1f-e8100f35e065)

<h3>Step 5 - Set Up IP Addressing on DC</h3>

Next up we are setting up our IP Adressing. We will have two NICs: one that will automatically get an IP address from your home router and the other we will now manually set up. Click the network icon at the bottom right of the VM screen and select "Network and Internet Settings". Under Related Settings, select "Change adapter options" and you will see two adapters. We will now figure out which adapter is directing to the internal network and which one is directing to the internet. You'll do this by right-clicking on either of the two and then select "Status" and then "Details". From here, you are looking for the IPv4 address. I selected the first adapter and can see that this particular adapter has an IPv4 address of 10.0.2.15, meaning this is my home DNS server.   

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/54ba3d8a-fd3b-49aa-9080-92ec225032ee)

Then right-click on the same adapter and rename the home DNS server to "INTERNET" and the other "INTERNAL".

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/8a1bcb63-5091-4b9f-bd90-736dd2ecc4d8)

Once you designate which adapter is the internal, we need to set an IP address for it. Right-click the "internal" adapter, select properties, hightlight "Internet Protocol Version 4 (TCP/IPv4) and click properties again. You'll click the box that says, "Use the following IP address" and enter the following information as seen below. The Preferred DNS server is going to re-route back to itself, so you can enter the IP address again, or use the following default DNS server ID to allow the computer to ping, or loopback, to itself. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/3391f572-e3ad-4070-8201-0e5d18edad26)

<h3>Step 6 - Set Up Domain/AD DS</h3>

We will now install Active Directory Domain Services and create a domain. Go to the Server Manager in the DC VM and click "Add Roles and Features". Click next twice, then when you come to 'Select Destination Server', your server should already be highlighted (as it's the only one), but if not, highlight your server. On the 'Select Server Roles' screen you'll select 'Active Directory Domain Services' and when the next box pops up asking if you want to add the required features, press "Add Features". After that, you can click next until  you get to the last page and then click "Install". 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/3eb00a93-6b28-4b19-b0c1-1270a8c36bb4)

Following the AD/DS installation, you'll notice a caution sign at the top of the screen. You'll go ahead and click 'Promote this sever to a domain controller'.

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/6b26d92e-c108-466d-b7a0-f484f82a9911)

Next, you'll select 'Add a new forest' and enter a generic name into the domain name box. Then click next. You will be prompted to enter a password, choose one of your liking (good idea to make a habit of using strong passwords!). Then click next until you're done and the system will restart on it's own. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/cc2631ee-ac28-4d02-90f1-aeb9d8bd995e)

Upon restart, you should see the MYDOMAIN\Administrator as the user login. We are going to add our own domain admin account. Go to start, Windows Administrative Tools, then Active Directory Users and Computers. You'll see our newly created mydomain.com. Right-click on the mydomain.com, go to New and scroll down to Organizational Unit. Go ahead and name this folder ADMIN and uncheck the box asking to protect the container from accidental deletion. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/d5369059-03c1-45d8-9bfe-a6800b5b9501)

In the new ADMIN folder, right-click the folder, go down to New and add a new User. Fill everything out with your name and choose your logon name. You'll be prompted for a password. I also checked the box "Password never expires" since it's a lab. Note, that is obviously not a good security policy!

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/03e79cd4-3a80-49dd-aa31-5a1e75f6e8f1)

To make your new account an admin account; right-click your name, click properties, then click add at the bottom of the screen. Type in Domain Admins and then click 'check names' to the right. It should find the admin group and go ahead and press OK. Go ahead and log out of the current user profile and log back in with your new admin account. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/17c311e4-e62a-4a81-baf1-653753775d95)

<h3>Step 7 - Set Up RAS / NAT</h3>

Next we are installing our Remote Access Server and Network Address Translation to allow us to give our client to be on private virtual network, but access the internet through the domain controller. Go to the Server Manager and click Add Roles and Features. Click next three times and once you get to the 'Select Server Roles' page, you want to select Remote Access. Click next until you get to the 'Select Role Services' page and select 'Routing'. DirectAccess and VPN (RAS) will auto populate, go ahead and leave it and then click next until you get to install and then install the new role and feature. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/65998ed4-da1e-42ed-a5dc-54e19cc6caa6)

Next, you'll go to 'Tools' on the Server Manager Dashboard and select 'Routing and Remote Access'. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/bc03a744-f3ec-42a0-abab-373baf3eab00)

Right-Click your Domain Controller local server and click 'Configure and Enable Routing and Remote Access'. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/a95a127d-07ff-49ea-881d-a8c94c70e364)

Under 'Configuration', Select 'Network address translation (NAT)

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/67859e64-efd8-4063-927c-e3ef14f0d745)

Under 'NAT Internet Connection', make sure to select the INTERNET adapter, NOT the internal one we named earlier. Hence, the importance of identifying them earlier. Then click next and then finish. The Domain Controller should now have a green arrow pointing up. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/f8d7abb7-56b5-4ec8-b763-fe77ee7e949c)

<h3>Step 8 - Set up DHCP Server</h3>

We will now set up our DHCP server to allow our client machine to browse the internet with a set range of IP addresses and subnet mask. This simulates how things work in your company and/or school. What you'll need to do is go to the Server Manager, click Add Roles and Features, next, next, next, and then on the 'Select Server Roles' page you'll select the DHCP Server box and add the required features. Then click next to the end and select install. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/aacffd98-c5ce-4e84-81d0-eccdf3b2b7b7)

Next, you'll go to 'Tools' on the Server Manager Dashboard and select DHCP. You'll see the DHCP popup box where we will set up our scope. Right-click the IPv4 server and click New Scope. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/2a491277-8609-4667-b528-36d156ef149c)

For the name of the Scope, put the IP address range (172.16.0.100-200)

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/fde8745a-2fa5-41c6-9df0-26ae008249aa)

For the IP Address Range, you'll use that defined range of IPs we just discussed (100-200). We will give this DHCP client a mask of 24.

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/ae8c2d0f-6111-48d5-9def-6fd9faa28ecd)

You can skip the exclusions page as we won't add any IP exlusions for this lab. For the Lease Duration, since this is a lab, you can keep the duration at the 8 days. This just designates how long a computer can have the given IP address when using the DHCP server. On the Configure DHCP Options, leave the selection of "Yes, I want to configure these options now". Now we will add an IP address to give the clients a default gateway/router to get to the internet through the internal NIC. You'll enter the Domain Controller's IP address and click add.  

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/1a285e63-11d7-4502-b9c6-4569178c0087)

Next it's asking us what we want to use as our DNS controller, we will be using our domain controller... so click next. You can skip the WINS server. Then click to the end. After this is completed and your back at the DHCP server popout, Authorize the DHCP server. Right click on your domain controller and click 'Authorize'. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/d1296649-2caa-48ff-8d4e-d012e4050828)

You may have to refresh the IPv4 and IPv6. After refreshing, you should see them activate with the green upward arrow. 

<h3>Step 9 - Configure Domain Controller for Internet Browsing</h3>

**Not good practice for a production environment** Go to the Server Manager and click 'Configure this local server' and click 'IE Enhanced Security Configuration' and disabled both options. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/ee278708-72c9-4cac-b3c9-1cab475aa06b)

<h3>Step 10 - Using PowerShell to Create Users</h3>

Next, we will use the code provided by Josh Madakor for the list of user names we will be implementing. 

Link: https://www.youtube.com/redirect?event=video_description&redir_token=QUFFLUhqbGdCNTFMWmVTS2F2UUEyQ1J3ajU3SGd6Z2x4Z3xBQ3Jtc0ttWER0V1Z3SnBRTTlOc0RwOXlST2dOcEZSNnZfem5hLUFTVWQxTm9xWjNxc1JiSG14R2dMSTBCODBUb3d4WTZhcks5Szl6YnMzS3UwekVmSUswS2JBOWNaZTFsSjJibGg5WmlQWk5wRTFQZWgtSV9lTQ&q=https%3A%2F%2Fgithub.com%2Fjoshmadakor1%2FAD_PS%2Farchive%2Frefs%2Fheads%2Fmaster.zip&v=MHsI8hJmggI

Open Internet Explorer in the Domain Controller and copy the link. Save the zip file to the desktop and then copy the extracted files to the desktop as well. Once the file is saved, open the "names" file and add your name to the top of the list of names, save the file.

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/39b59afb-0f45-49f8-a47e-7fe41403bf23)

Then click on start, go to Windows PowerShell, and select Windows PowerShell ISE and right-click to run as an administrator.

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/98bae828-a6cd-4c96-858c-12ec3b773b52)

Open the PowerShell 1_CREATE_USERS file and in the command line type the command: Set-ExecutionPolicy Unrestricted... **Please note, this is a security feature and should not be used outside of a lab environment** This allows us to enable the execution of all scripts on the server. Then click 'Yes to All' on the Policy change banner. 

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/600b0f65-e192-4a0b-bc21-5c45361315d4)

Now, to get the code to generate the names, we have to go to the directory within PowerShell. I used cd to change the directory, and then went to the file location: i.e., cd C:\users\a-amolinaro\desktop\AD_PS-mater\1_CREATE_USERS.ps1

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/7db4105c-dfdd-4993-86c2-188ad1dd2552)

Now, click run and you will see the names begin to populate PowerShell. 

<h3>Step 11 - Create the Windows 10 VirtualBox</h3>

To set up our Client VM, we will essentially follow the exact same steps as when we created the Domain Controller. However, we are installing Windows 10 on this VM, so make sure to select the proper ISO file. If you run into trouble regarding the Product Key for the Windows10 machine, I used the following link from Microsoft for generic keys. I used the VK7JG-NPHTM-C97JM-9MPGT-3V66T key and it worked fine. The only difference, is once you have created the Client1 VM, go to settings on Oracle VM and under 'Network' change the Network Adapter to Internal Network. 

Link: https://www.tenforums.com/tutorials/95922-generic-product-keys-install-windows-10-editions.html

![image](https://github.com/amolinaro23/ActiveDirectoryLab/assets/164687651/bb225fbf-3a99-4ed3-96c7-c66d1ffdb705)

<h3>Step 12 - </h3>


