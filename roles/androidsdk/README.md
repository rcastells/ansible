This role allows you to install Android-SDK for Linux, Windows or Mac:

Ubuntu OS

In order to run the playbook for Ubuntu OS you must have a user with SSH access to the server and sudo privileges to become root.

Edit your inventory file and add ubuntu server alias if necessary:

ubuntu ansible_host=<ubuntu_host>
Create your playbook file (for example androidsdk.yml) and add the ubuntu hosts:

--- 
# Playbook that installs AndroidSDK 
- hosts: ubuntu
    roles:
     - androidsdk
Then run the playbook as follows:

ansible-playbook -i <your_inventory> androidsdk.yml -u <user> -b -K -k
Notes:
'-k' it will ask you for your password. You can avoid it if you are going to use ssh keys.
'-u <user>' is the user you are going to use to connect to remote server. You can avoid it if you are going to use your current username.
'-b' is to become root on the remote server. Mandatory.
'-K' it will ask you for sudo password. Mandatory if you need password to become root.

As you will see the playbook shows the list of available packages and by default it will install 1,2,3,5,37.

You can change which packages do you want to install by editing file roles/androidsdk/defaults/main.yml variable:

android_tools_filter: "1,2,3,5,37"
Also you could edit a variable for every host, so you can specify different packages for every host or group of hosts.

WindowsOS

In order to run the playbook for WindowsOS you must have credentials for an Admin account.

Edit your inventory file and add ubuntu server alias if necessary:

windows ansible_host=<windows_host>
Edit or create the playbook file androidsdk.yml file and put windows host alias in host option as follows:

--- 
# Playbook that installs AndroidSDK + JAVA
- hosts: windows
     roles:
        - androidsdk


Ansible will use winrm connection. In order to be able to run the Ansible on the remote host you must run ConfigureRemotingForAnsible.ps1 file on the WindowsOS remote host. So connect to your windows host first and run the powershell script. You must be able also to connect to winrmi secure port (by default 5986).
Create or edit host_vars file for this windows host (not included in github) and put your Admin account credentials in host_vars/windows.yml file as follows (here you can also change wirmi port if necessary):

ansible_user: Administrator
ansible_password: "password_example"
ansible_port: 5986
ansible_connection: winrm
ansible_winrm_server_cert_validation: ignore
Remember to run the Powershell script ConfigureRemotingForAnsible.ps1 on remote host before to run the playbook.

Then run the playbook as follows:

ansible-playbook -i <your_inventory> androidsdk.yml
MacOS

In order to run the playbook for MacOSX you must have a user with SSH access to the server and sudo privileges to become root.

Edit your inventory file and add mac server alias if necessary:

mac ansible_host=<mac_host>
Edit or create androidsdk.yml file and put mac host alias in host option as follows:

--- 
# Playbook that installs AndroidSDK + JAVA
- hosts: mac
   roles:
     - androidsdk
Then run the playbook as follows:

ansible-playbook -i inventory/production androidsdk.yml -u <user> -b -K -k
Notes:
'-k' it will ask you for your password. You can avoid it if you are going to use ssh keys.
'-u <user>' is the user you are going to use to connect to remote server. You can avoid it if you are going to use your current username.
'-b' is to become root on the remote server. Mandatory.
'-K' it will ask you for sudo password. Mandatory if you need the password to become root.

You can change which packages do you want to install by editing file roles/androidsdk/defaults/main.yml variable:

android_tools_filter: "1,2,3,5,37"
Also you could edit a variable for every host, so you can specify different packages for every host or group of hosts.
