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
- MySQL
- HeidiSQL

<h2>Why Are We Using a Virtual Machine?</h2>

Using a Virtual Machine to test software is a great practice because it keeps the environment of your physical computer clean. It also prevents potential issues during installation from affecting your main system.

<h2>Create Virtual Machine</h2>

1. Log in to the Azure portal.
2. Create a Virtual Machine:
   - Use a Windows 10 image to create your VM.
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
     <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 10 21 35 AM" src="https://github.com/user-attachments/assets/816ffd66-35d4-4d9e-a061-bf9eb916c8b9" />



<h2>Enable Internet Information Services (IIS) with Commmon Gateway Interface (CGI)</h2>

IIS is a web server that is part of Windows systems. We will use IIS to host our osTicket web application. Since osTicket is built with PHP and IIS does not know how to process PHP files by itself, we will use CGI to pass requests for PHP files from IIS to the PHP interpreter. The PHP interpreter will then process the file, and CGI will send the result back to IIS, which will deliver the page to the browser.

1. To Enable IIS:
    - Go into the <b>Control Panel</b>. You can find it by typing <b>Control Panel</b> in the search bar on the bottom left.
    - Select <b>Programs</b>.
    - Select <b>Turn Windows features on or off</b>.
    - Check the checkbox next to <b>Internet Information Services</b> to enable it.
        <img height="65%" width="65%" alt="" src="https://github.com/user-attachments/assets/69ba977e-7d93-4d98-a47e-6b8d728b611c" />      
2. To Enable CGI:
   - Click the <b>+</b> to the left of <b>Internet Information Services</b> to expand it.
   - Expand <b>World Wide Web Services</b>.
   - Expand <b>Application Development Features</b>.
   - Check the checkbox next to <b>CGI</b> to enable it, and select <b>Ok</b>.  
       <img height="65%" width="65%" alt="" src="https://github.com/user-attachments/assets/924f961f-1c78-4820-b711-0ba629994448" />

<h2>Extract osTicket Installation Files</h2>
The dependencies needed for the osTicket installation are bundled together in a single ZIP folder for easy download.

1. Open the browser on your VM.
2. Copy and paste [this link](https://docs.google.com/document/d/1aT20BxyepQpP6PAVp9QQ_p1fYtP7Cip2UxhMYVfY97s/edit?tab=t.0) into the browser on your VM to download the ZIP folder containing all the necessary files for the osTicket installation.
3. Right-click the ZIP folder and select <b>Extract All</b> to extract the files from the ZIP folder onto your desktop. This step is necessary because compressed files cannot be directly used by the system, as they need to be unpacked to their original format for installation. The extracted folder should be called “osTicket-Installation-Files”.

<h2>Configuring PHP for IIS</h2>

1. Install <b>PHP Manager for IIS (PHPManagerForIIS_V1.5.0.msi)</b> from the “osTicket-Installation-Files” folder. This tool helps manage PHP configurations in IIS.
2. Create the directory <b>C:\PHP</b>. This directory will store the PHP interpreter and related files.
3. Unzip <b>PHP 7.3.8 (php-7.3.8-nts-Win32-VC15-x86.zip</b>) from the “osTicket-Installation-Files” folder into the <b>C:\PHP</b> folder.
4. Install <b>VC_redist.x86.exe</b> from the “osTicket-Installation-Files” folder. This step ensures PHP has the necessary dependencies needed to work on Windows.
5. Register PHP with IIS:
   - Open IIS as an Admin.
   - Select <b>PHP Manager</b>.
   - Click <b>Register new PHP version</b>.
   - Paste the file path to the PHP executable. Since we placed PHP in the <b>C:/PHP</b> directory, our path should be <b>C:\PHP\php-cgi.exe</b>.
   - Stop and restart the server to apply changes:
     - In IIS right-click the top-level entry <b>osticket-vm</b> in IIS.
     - Select <b>Stop</b>, then repeat the steps to <b>Start</b> the server.

<h2>Install the Rewrite Module</h2>
The Rewrite Module helps IIS modify URLs to create user-friendly URLs that are easier to read, share, and remember.

1. Install the <b>Rewrite Module (rewrite_amd64_en-US.msi)</b> from the “osTicket-Installation-Files” folder. 

<h2>Install MySQL</h2>
MySQL is a relational database management system used to store and manage data. In this instance, we will use it to store osTicket data, such as user accounts and ticketing information.

1. Install <b>MySQL 5.5.62 (mysql-5.5.62-win32.msi)</b> from the “osTicket-Installation-Files” folder.
2. Follow the installation Wizard:
   - Select <b>Typical</b> for Setup Type.
   - After installation, launch the configuation wizard.
   - For Serve Instance Configuration:
     - Select <b>Standard Configuration</b>.
     - Setup a simple password for the root user. Be sure to keep track of these credentials.
     - Follow the remaining steps in the Wizard and select <b>Execute</b> to complete the setup.

<h2>Setup osTicket files</h2>

1. Unzip <b>“osTicket-v1.15.8.zip”</b> from the “osTicket-Installation-Files” folder
2. Copy the <b>Upload</b> folder into <b>“c:\inetpub\wwwroot”</b>. The <b>wwwroot</b> folder is the root directory for IIS-hosted websites. By placing the osTicket files here, IIS will know where to find them when the application is accessed.
3. Within <b>c:\inetpub\wwwroot</b>, rename “upload” to “osTicket”. This will help IIS serve the application under the name "osTicket" when accessed through a browser. Ex: <b>http://localhost/osTicket</b>
4. Stop and restart the server to apply changes:
     - In IIS right-click the top-level entry labeled <b>osticket-vm</b> in IIS.
     - Select <b>Stop</b>, and then repeat the process to <b>Start</b> the server.

<h2>Open osTicket in Browser</h2>
Let's access osTicket in the browser to ensure IIS is serving the app.

1. In IIS, on the left-hand panel, select <b>Sites</b> from the file hierarchy.
2. Expland <b>Default Websites</b>.
3. Select <b>osTicket</b>.
4. In the right-hand panel, click <b>Browse *:80</b> to open osTicket in your browser.

<h2>Enable PHP Extensions</h2>
Some required PHP extensions are disabled by default. Lets enable them:

1. In IIS, on the left-hand panel, select <b>Sites</b> -> <b>Default Websites</b> -> <b>osTicket</b>.
2. Double click <b>PHP Manager</b>.
3. Click <b>Enable or disable extension</b>.
4. To enable an extension, right-click it and select <b>Enable</b>.
   - Extensions to enable:
      - <b>php_imap.dll</b>
      - <b>php_intl.dll</b>
      - <b>php_opcache.dll</b>
5. Refresh the browser to observe the changes.
<h2>Configure ost-config.php</h2>

<h3>Rename ost-sampleconfig.php:</h3>
The file <b>ost-sampleconfig.php</b> is a template configuration file included with osTicket. It is named this way by default to ensure flexibility and security during installation and updates. By renaming it, we are creating the active configuration file required for osTicket to function.

1. Follow the path to rename the file:
   - From: <b>C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php</b>
   - To: <b>C:\inetpub\wwwroot\osTicket\include\ost-config.php</b>
   
<h3>Assign Permissions</h3>
To allow osTicket to make changes on the backend and store database configuration details, we need to modify the permissions for <b>ost-config.php</b>.

1. Right-click <b>ost-config.php</b>.
2. Select <b>Properties</b>.
3. Go to the <b>Security</b> tab.
4. Click <b>Advanced</b>.
5. Click <b>Disable inheritance</b> to remove all current permissions.
6. In the pop-up, select <b>Remove all inherited permissions from this object</b>.
7. Click <b>Add</b> to assign new permissions.
8. Select <b>Select a principal</b>.
9. Under <b>Enter the object name to select</b>, type <b>Everyone</b>.
<i>Note:</i> Assigning permissions to "Everyone" is not recommended in real-world scenarios. We are using it here for simplicity and because we do not know which user account represents osTicket.
10. Click <b>OK</b>.
11. Check the box for <b>Full control</b>.
12. Click <b>OK</b>.
13. Click <b>Apply</b>.
14. Click <b>OK</b> to close all windows.

<h2>Install HeidiSQL</h2>
HeidiSQL provides us with a graphical user interface (GUI) for managing our MySQL database. It eliminates the need to write SQL commands in the command line by offering a visual way to create databases, manage tables, and run queries. This makes database management simpler and more accessible, especially for those who are not comfortable using the command line.

1. Install <b>HeidiSQL</b> from From the “osTicket-Installation-Files” folder.
2. Click <b>New</b>.
3. Create a new session:
   - Ensure that the <b>User</b> is <b>root</b>.
   - Add the password that you created when you configured your <b>MySQL</b> database.
4. Select <b>Open</b> to connect to the session.
5. Create a database called <b>osTicket</b>.

 
<h2>Database Setup & Final Installation</h2>

1. In the browser, select <b>Continue</b> to continue the osTicket installation process.
2. Fill out the form with your information. <i>Note:</i> The <b>Default Email</b> and the <b>Admin User Email address</b> must be different.
  - System Settings:
       - Create a <b>Helpdesk Name</b>.
       - Add a <b>Default Email</b>. <i>Note:</i> This is the email address that will receive emails from customers.
   - Admin User:
     - Add your <b>Name</b>, <b>Last Name</b>, and <b>Email address</b>.
     - Create a <b>username</b> and <b>password</b> for Admin User.
   - Database Settings:
     - MySQL Database: osTicket.
     - MySQL Username: root.
     - MySQL Password: root.
- Click <b>Install Now</b>.

<h2>Test Installation</h2>
To ensure that osTicket was properly installed and configured, let's browse to the Admin and End User pages.

1. Admin:
   - URL: http://localhost/osTicket/scp/login.php
   - Sign in with your <b>Admin User</b> credentials.
   - You should see 1 ticket in the queue with the subject: <b>osTicket Installed</b>. This confirms that osTicket is successfully installed!
2. End User:
   - URL: http://localhost/osTicket/ 
 
<h2>Post-Installation Cleanup</h2>
To secure your osTicket installation and reduce vulnerabilities, follow these steps:

1. Delete the setup folder: The setup folder contains installation files that can be overwritten. Deleting it prevents your osTicket installation from being tampered with.
   - Delete: <b>C:\inetpub\wwwroot\osTicket\setup</b>.
2. Set permissions to "Read" only: The ost-config.php file contains your database credentials. Changing its permission to "Read" only prevents sensitive information from being modified and enhances overall security.
   - Set Permissions to “Read” only: <b>C:\inetpub\wwwroot\osTicket\include\ost-config.php</b>.

