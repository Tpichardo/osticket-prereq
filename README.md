<p align="center">
<img src="https://i.imgur.com/Clzj7Xs.png" alt="osTicket logo"/>
</p>

<h1>osTicket: Prerequisites and Installation</h1>
<p> This tutorial outlines the prerequisites and guides you through the installation process of the open-source support ticketing system, osTicket.<p />

<h2>What is osTicket?</h2>
osTicket is a free open-source help desk ticketing system companies can use to manage support tickets.

<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machine)
- [Windows App](https://apps.apple.com/us/app/windows-app/id1295203466?mt=12) for macOS or Remote Desktop for Windows  
- Internet Information Services (IIS)

<h2>Why Are We Using a Virtual Machine?</h2>

Using a Virtual Machine to test software is a great practice because it keeps the environment of your physical computer clean. It also prevents potential issues during installation from affecting your main system.

<h2>Create Virtual Machine</h2>

1. Log in to the Azure portal.
2. Create a Virtual Machine
   - Use a Windows 10 image to create your VM
   - For VM size, select anything with 2-4 virtual CPUs (vcpus).
   - Save your authentication credentials as you will use them for authenticating later.

<h2>Connect to Windows VM</h2>

1. Open the Windows App and click on the <b>+</b> at the top right of the window. Then select <b>add PC</b>
   
   <img src="https://i.imgur.com/q2CIjtQ.png" height="80%" width="80%" alt="interface for adding a PC on the Windows App"/>
2. Paste your Windows VM's public IP address for <b>PC name</b>. Feel free to give your VM whatever friendly name you would like. I named mine windows-vm so that I can easily know what operating system is running on the PC. Then click <b>Add</b> to add the VM.
   
      <img src="https://i.imgur.com/cvBmV30.png" height="80%" width="80%" alt=""/>
3. Click on the ellipsis and select <b>connect</b> to connect to the Windows VM.
   
   <img src="https://i.imgur.com/OKSJhL1.png" height="80%" width="80%" alt=""/>
4. Add the username and password you created when you created your Virtual Machine in the Azure portal to authenticate, and click continue.
   
    <img src="https://i.imgur.com/dkhuqJB.png" height="80%" width="80%" alt=""/>
5. Feel free to toggle no for all privacy settings.

<h2>Enable Internet Information Services (IIS) with Commmon Gateway Interface (CGI)</h2>

IIS is a web server that is part of Windows systems. We will use IIS to host our osTicket web application. Since osTicket is built with PHP and IIS does not know how to process PHP files by itself, we will use CGI to pass requests for PHP files from IIS to the PHP interpreter. The PHP interpreter will then process the file, and CGI will send the result back to IIS, which will deliver the page to the browser.

1. To Enable IIS:
    - Go into the <b>Control Panel</b>. You can find it by typing <b>Control Panel</b> in the search bar on the bottom left
    - Select <b>Programs</b>
    - Select <b>Turn Windows features on or off</b>
    - Check the checkbox next to <b>Internet Information Services</b> to enable it
        <img height="65%" width="65%" alt="" src="https://github.com/user-attachments/assets/69ba977e-7d93-4d98-a47e-6b8d728b611c" />      
2. To Enable CGI:
   - Click the <b>+</b> to expand Internet Information Services
   - Expand <b>World Wide Web Services</b>
   - Expand <b>Application Development Features</b>
   - Check the checkbox next to <b>CGI</b> to enable it  
       <img height="65%" width="65%" alt="" src="https://github.com/user-attachments/assets/924f961f-1c78-4820-b711-0ba629994448" />

 
