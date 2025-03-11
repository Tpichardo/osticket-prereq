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
   <br>
   <img src="https://i.imgur.com/q2CIjtQ.png" height="80%" width="80%" alt="interface for adding a PC on the Windows App"/>
2. Paste your Windows VM's public IP address for <b>PC name</b>. Feel free to give your VM whatever friendly name you would like. I named mine windows-vm so that I can easily know what operating system is running on the PC. Then click <b>Add</b> to add the VM.
   <br>
   <img src="https://i.imgur.com/cvBmV30.png" height="80%" width="80%" alt=""/>
3. Click on the ellipsis and select <b>connect</b> to connect to the Windows VM.
   <br>
   <img src="https://i.imgur.com/OKSJhL1.png" height="80%" width="80%" alt=""/>
4. Add the username and password you created when you created your Virtual Machine in the Azure portal to authenticate, and click continue.
   <br>
   <img src="https://i.imgur.com/dkhuqJB.png" height="80%" width="80%" alt=""/>
5. Feel free to toggle no for all privacy settings.
    <br>
   <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 10 21 35 AM" src="https://github.com/user-attachments/assets/816ffd66-35d4-4d9e-a061-bf9eb916c8b9" />



<h2>Enable Internet Information Services (IIS) with Commmon Gateway Interface (CGI)</h2>

IIS is a web server that is part of Windows systems. We will use IIS to host our osTicket web application. Since osTicket is built with PHP and IIS does not know how to process PHP files by itself, we will use CGI to pass requests for PHP files from IIS to the PHP interpreter. The PHP interpreter will then process the file, and CGI will send the result back to IIS, which will deliver the page to the browser.

1. To Enable IIS:
    - Go into the <b>Control Panel</b>.
    - Select <b>Programs</b>.
      <br>
      <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 10 29 23 AM" src="https://github.com/user-attachments/assets/0558ebe2-6b89-43d2-91d1-9cac58610a21" />
    - Select <b>Turn Windows features on or off</b>.
      <br>
      <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 10 29 39 AM" src="https://github.com/user-attachments/assets/5374d622-966a-4c2a-85e0-59404217b099" />
    - Check the checkbox next to <b>Internet Information Services</b> to enable it.
      <br>
      <img height="80%" width="80%" alt="" src="https://github.com/user-attachments/assets/69ba977e-7d93-4d98-a47e-6b8d728b611c" />      
2. To Enable CGI:
   - Click the <b>+</b> to the left of <b>Internet Information Services</b> to expand it.
   - Expand <b>World Wide Web Services</b>.
   - Expand <b>Application Development Features</b>.
   - Check the checkbox next to <b>CGI</b> to enable it, and select <b>Ok</b>.
      <br>
      <img height="80%" width="80%" alt="" src="https://github.com/user-attachments/assets/924f961f-1c78-4820-b711-0ba629994448" />

<h2>Extract osTicket Installation Files</h2>
The dependencies needed for the osTicket installation are bundled together in a single ZIP folder for easy download.

1. Open the browser on your VM.
2. Copy and paste [this link](https://docs.google.com/document/d/1aT20BxyepQpP6PAVp9QQ_p1fYtP7Cip2UxhMYVfY97s/edit?tab=t.0) into the browser on your VM to download the ZIP folder containing all the necessary files for the osTicket installation.
   <br>
   <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 11 55 25 AM" src="https://github.com/user-attachments/assets/f7f53b4b-e7e3-46ad-9873-8543f7d2f1b9" />
3. Click the folder icon on the downloads window in the browser.
   <br>
   <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 11 55 25 AM" src="https://github.com/user-attachments/assets/4940c2ec-1fb5-4f11-b153-b6e6bf2e3435" />
4. Move the ZIP folder to your desktop.
5. Right-click the ZIP folder and select <b>Extract All</b> to extract the files from the ZIP folder onto your desktop. This step is necessary because compressed files cannot be directly used by the system, as they need to be unpacked to their original format for installation. The extracted folder should be called “osTicket-Installation-Files”.

<h2>Configuring PHP for IIS</h2>

1. Install <b>PHP Manager for IIS (PHPManagerForIIS_V1.5.0.msi)</b> from the “osTicket-Installation-Files” folder. This tool helps manage PHP configurations in IIS.
2. Create the directory <b>C:\PHP</b>. This directory will store the PHP interpreter and related files.
   <br>
   <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 12 06 46 PM" src="https://github.com/user-attachments/assets/c5973974-b360-4d0e-b2f6-86f2cada6d2a" />
3. Extract all the files from <b>PHP 7.3.8 (php-7.3.8-nts-Win32-VC15-x86.zip</b>), located in the “osTicket-Installation-Files” folder, into the <b>C:\PHP</b> folder.
   <br>
   <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 12 07 15 PM" src="https://github.com/user-attachments/assets/28be059f-2df4-484e-b0c7-cad1f6a1534a" />
   <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 12 09 24 PM" src="https://github.com/user-attachments/assets/6a2041fd-7d87-4e4e-9b85-2037d9df6f8b" />
4. Install <b>VC_redist.x86.exe</b> from the “osTicket-Installation-Files” folder. This step ensures PHP has the necessary dependencies needed to work on Windows.
5. Register PHP with IIS:
   - Open IIS as an Admin.
     <br>
     <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 12 18 29 PM" src="https://github.com/user-attachments/assets/ed01645a-66d9-46bf-8eb3-b630f5762533" />
   - Select <b>PHP Manager</b>.
     <br>
     <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 12 32 14 PM" src="https://github.com/user-attachments/assets/83fd195f-747d-44c9-95be-132ad3680029" />
   - Click <b>Register new PHP version</b>.
     <br>
     <img height="80%" width="80%" alt="Screenshot 2025-03-10 at 9 11 36 PM" src="https://github.com/user-attachments/assets/f9daece9-897c-4312-9eb1-074e86d0bf89" />
   - Paste the file path to the PHP executable. Since we placed PHP in the <b>C:/PHP</b> directory, our path should be <b>C:\PHP\php-cgi.exe</b>.
   - Stop and restart the server to apply changes:
     - In IIS right-click the top-level entry.
     - Select <b>Stop</b>, then repeat the steps to <b>Start</b> the server.
       <br>
       <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 12 32 47 PM" src="https://github.com/user-attachments/assets/318df9e5-bd3a-4c83-afc6-a72c3da40b76" />

<h2>Install the Rewrite Module</h2>
The Rewrite Module helps IIS modify URLs to create user-friendly URLs that are easier to read, share, and remember.

1. Install the <b>Rewrite Module (rewrite_amd64_en-US.msi)</b> from the “osTicket-Installation-Files” folder. 

<h2>Install MySQL</h2>
MySQL is a relational database management system used to store and manage data. In this instance, we will use it to store osTicket data, such as user accounts and ticketing information.

1. Install <b>MySQL 5.5.62 (mysql-5.5.62-win32.msi)</b> from the “osTicket-Installation-Files” folder.
2. Follow the installation Wizard:
   - Select <b>Typical</b> for Setup Type.
     <br>
     <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 12 27 42 PM" src="https://github.com/user-attachments/assets/b1abaad5-e421-4b7c-80ec-ad67b2498ed8" />
   - After installation, launch the configuation wizard.
   - For Serve Instance Configuration:
     - Select <b>Standard Configuration</b>.
       <br>
       <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 12 34 12 PM" src="https://github.com/user-attachments/assets/32c919d5-52b4-43ea-b0b7-1a0f2e8de128" />
     - Setup a new root password for the root user. Be sure to keep track of these credentials.
       <br>
       <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 12 34 37 PM" src="https://github.com/user-attachments/assets/15bc512f-d12e-4e41-9e2a-0edf90fa1c6e" />
     - Follow the remaining steps in the Wizard and select <b>Execute</b> to complete the setup.

<h2>Setup osTicket files</h2>

1. Right-click <b>“osTicket-v1.15.8.zip”</b> from the “osTicket-Installation-Files” folder and select <b>Extract All</b>.
2. Open the unzipped foder <b>“osTicket-v1.15.8”</b>, and drag the <b>Upload</b> folder into <b>“c:\inetpub\wwwroot”</b>. The <b>wwwroot</b> folder is the root directory for IIS-hosted websites. By placing the osTicket files here, IIS will know where to find them when the application is accessed.
   <br>
   <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 12 51 33 PM" src="https://github.com/user-attachments/assets/e7b3d89c-265f-4643-9b93-d1d837fcb3df" />
3. Within <b>c:\inetpub\wwwroot</b>, rename “upload” to “osTicket”. This will help IIS serve the application under the name "osTicket" when accessed through a browser. Ex: <b>http://localhost/osTicket</b>.
   <br>
   <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 12 52 22 PM" src="https://github.com/user-attachments/assets/17b4e55f-d7b7-4a08-93d4-ea5fec42a448" />
4. Stop and restart the server to apply changes:
     - In IIS right-click the top-level entry.
     - Select <b>Stop</b>, and then repeat the process to <b>Start</b> the server.

<h2>Open osTicket in Browser</h2>
Let's access osTicket in the browser to ensure IIS is serving the app.

1. In IIS, on the left-hand panel, select <b>Sites</b> -> <b>Default Websites</b> -> <b>osTicket</b>.
   <br>
   <img height="80%" width="80%" alt="Screenshot 2025-03-11 at 2 49 26 AM" src="https://github.com/user-attachments/assets/fce59458-3285-43aa-8b83-bebf960413e8" />
2. In the right-hand panel, click <b>Browse *:80</b> to open osTicket in your browser.
   <br>
   <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 12 53 50 PM" src="https://github.com/user-attachments/assets/a7e60cf2-c872-490f-88fc-3a9678a875a0" />
   <br>
   <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 12 54 29 PM" src="https://github.com/user-attachments/assets/d560946f-ec48-492f-81bc-b92b60663001" />

<h2>Enable PHP Extensions</h2>
Some required PHP extensions are disabled by default. Lets enable them:

1. In IIS, on the left-hand panel, select <b>Sites</b> -> <b>Default Websites</b> -> <b>osTicket</b>.
2. Double click <b>PHP Manager</b>.
3. Click <b>Enable or disable extension</b>.
   <br>
   <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 12 55 32 PM" src="https://github.com/user-attachments/assets/17a7a857-c8fb-4bcf-8d32-9fec91cd2788" />
4. To enable an extension, right-click it and select <b>Enable</b>.
   <br>
   <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 12 56 01 PM" src="https://github.com/user-attachments/assets/6fe0231a-1c89-4ca3-a76a-c56dd8b053c2" />
   - Extensions to enable:
      - <b>php_imap.dll</b>
      - <b>php_intl.dll</b>
      - <b>php_opcache.dll</b>
5. Refresh the browser to observe the changes or visit <b>http://localhost/osTicket</b>.
   
<h2>Configure ost-config.php</h2>

<h3>Rename ost-sampleconfig.php:</h3>
The file <b>ost-sampleconfig.php</b> is a template configuration file included with osTicket. It is named this way by default to ensure flexibility and security during installation and updates. By renaming it, we are creating the active configuration file required for osTicket to function.

1. Follow the path to rename the file:
   - From: <b>C:\inetpub\wwwroot\osTicket\include\ost-sampleconfig.php</b>.
     <br>
     <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 1 01 55 PM" src="https://github.com/user-attachments/assets/15e7ac78-7646-462a-970c-72d8f2f57887" />
   - To: <b>C:\inetpub\wwwroot\osTicket\include\ost-config.php</b>.
     <br>
     <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 1 02 26 PM" src="https://github.com/user-attachments/assets/9b8d0dc4-c81a-49f2-bd20-fc3f9f0577c8" />

<h3>Assign Permissions</h3>
To allow osTicket to make changes on the backend and store database configuration details, we need to modify the permissions for <b>ost-config.php</b>.

1. Right-click <b>ost-config.php</b>.
2. Select <b>Properties</b>.
3. Select the <b>Security</b> tab.
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
14. Click <b>OK</b> to close the window.

<h2>Install HeidiSQL</h2>
HeidiSQL provides us with a graphical user interface (GUI) for managing our MySQL database. It eliminates the need to write SQL commands in the command line by offering a visual way to create databases, manage tables, and run queries. This makes database management simpler and more accessible, especially for those who are not comfortable using the command line.

1. Install <b>HeidiSQL</b> from From the “osTicket-Installation-Files” folder, and launch it.
2. Click <b>New</b>.
   <br>
   <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 3 47 09 PM" src="https://github.com/user-attachments/assets/20003b36-e5c0-4f32-b7ee-5e663beb9496" />
3. Create a new session:
   - Ensure that the <b>User</b> is <b>root</b>.
     <br>
     <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 3 47 20 PM" src="https://github.com/user-attachments/assets/4da50271-d3dc-4632-be56-3acea22bd3b7" />
   - Add the password that you created when you configured your <b>MySQL</b> database.
4. Select <b>Open</b> to connect to the session.
   <br>
   <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 3 49 11 PM" src="https://github.com/user-attachments/assets/5f669979-3ab0-45b7-9d59-3fe3d67cf553" />
5. Create a database called <b>osTicket</b>:
   - Right-click the top-level entry on the left-hand panel.
     <br>
     <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 5 03 05 PM" src="https://github.com/user-attachments/assets/c6cdc438-8736-4d1e-ac55-a5fbc350068f" />
   - Select <b>Create new</b> -> <b>Database</b>.
     <br>
     <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 5 03 12 PM" src="https://github.com/user-attachments/assets/e5e25433-294c-4660-8909-5b03b657245e" />
   - Provide a name for the database.
     <br>
     <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 5 03 28 PM" src="https://github.com/user-attachments/assets/e7f58c04-3785-4bee-bbf2-927b5944cbf8" />
     <br>
     <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 5 03 52 PM" src="https://github.com/user-attachments/assets/d389617e-507f-4782-9d78-70e357878200" />


<h2>Database Setup & Final Installation</h2>

1. In the browser, select <b>Continue</b> to continue the osTicket installation process, If you closed the window, the URL is  <b>http://localhost/osTicket</b>.
2. Fill out the form with your information. <i>Note:</i> The <b>Default Email</b> and the <b>Admin User Email address</b> must be different.
  - System Settings:
       - Create a <b>Helpdesk Name</b>.
       - Add a <b>Default Email</b>. <i>Note:</i> This is the email address that will receive emails from customers.
   - Admin User:
     - Add your <b>Name</b>, <b>Last Name</b>, and an additional <b>Email address</b>.
     - Create a <b>username</b> and <b>password</b> for Admin User.
       <br>
       <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 5 16 28 PM" src="https://github.com/user-attachments/assets/52c5d938-05a5-4504-8568-80e48cf279d7" />
   - Database Settings:
     - MySQL Database: osTicket.
     - MySQL Username: root.
     - MySQL Password: The password you created when configuring MySQL.
       <br>
       <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 5 17 36 PM" src="https://github.com/user-attachments/assets/f11aa0d9-75a9-4797-91f9-a9aa591edc99" />
- Click <b>Install Now</b>.

<h2>Test Installation</h2>
To ensure that osTicket was properly installed and configured, let's browse to the Admin and End User pages.

1. Admin:
   - URL: http://localhost/osTicket/scp/login.php
   - Sign in with your <b>Admin User</b> credentials.
   - You should see 1 ticket in the queue with the subject: <b>osTicket Installed</b>. This confirms that osTicket is successfully installed!
     <br>
     <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 5 19 50 PM" src="https://github.com/user-attachments/assets/979265c0-7370-41f5-9d73-c248a3cf5c15" />
2. End User:
   - URL: http://localhost/osTicket/
     <br>
     <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 5 20 21 PM" src="https://github.com/user-attachments/assets/7c162dc5-e867-4d30-9080-418ad9c3f5e6" />
 
<h2>Post-Installation Cleanup</h2>
To secure your osTicket installation and reduce vulnerabilities, follow these steps:

1. Delete the setup folder: The setup folder contains installation files that can be overwritten. Deleting it prevents your osTicket installation from being tampered with.
   - Delete: <b>C:\inetpub\wwwroot\osTicket\setup</b>.
2. Set permissions to "Read" only: The ost-config.php file contains your database credentials. Changing its permission to "Read" only prevents sensitive information from being modified and enhances overall security.
   - Set Permissions to “Read” only: <b>C:\inetpub\wwwroot\osTicket\include\ost-config.php</b>.
     <br>
     <img height="80%" width="80%" alt="Screenshot 2025-03-06 at 5 23 39 PM" src="https://github.com/user-attachments/assets/53a7aa5e-e966-4c44-8168-f15de680d724" />
