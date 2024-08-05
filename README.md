# Remote-access-controls
**Implementing and evaluating some of the best RDP and SSH security practices.**

RDP Control Implementation and Testing:
Implementing and testing RDP control practices such as user access control, firewall configuration, complex password policy, NLA, account lockout policy and restricting RDP access configurations in the Windows VM.
1.Remote desktop user access control:  
  Create a new user in the windows VM:
  
  ![image](https://github.com/user-attachments/assets/79e106e8-52fe-4c86-8905-a8ae9360a151)

  Add the created new user to Remote Desktop Users group
  
  ![image](https://github.com/user-attachments/assets/5d856003-bd71-4849-b3c3-a9335361536d)

  Connect to Remote Desktop
  
  ![image](https://github.com/user-attachments/assets/f1e229e6-26df-46f3-9660-88cc33f78076)

  ![image](https://github.com/user-attachments/assets/1c793d9c-c23b-4192-bc65-17dbae079e8d)

  Remove remote desktop access for the account.

  ![image](https://github.com/user-attachments/assets/8920cc10-927d-4a12-80f2-b5ca57730c0e)

  Disconnect from Windows VM RDP session and login again.

  ![image](https://github.com/user-attachments/assets/35821d73-4377-4552-b3bc-4b4b9c45dfb6)

  Configure firewall rules to restrict RDP access from off traffic

  ![image](https://github.com/user-attachments/assets/d4c75bde-876f-4b37-bd87-4ff1f3801590)

 Install Remote Desktop Gateway service and create connection authorization and resource authorization policy

 ![image](https://github.com/user-attachments/assets/d570ed12-9cd0-43cb-95c1-3ccb4514feaf)

 ![image](https://github.com/user-attachments/assets/cf505116-f8ea-4b28-902c-82be128e70c5)

 Add User groups to access list and specify remote desktop ports.

 ![image](https://github.com/user-attachments/assets/61a0edcb-47ab-4e39-ae9d-6570e3e5b4ed)

 ![image](https://github.com/user-attachments/assets/36a9c67c-66eb-45f5-be18-e069b8299710)

 Install a certificate, evaluate the connectivity, and verify access.

 ![image](https://github.com/user-attachments/assets/6f96ea72-c7b7-4af8-ae4f-5c4701c58a58)

 ![image](https://github.com/user-attachments/assets/6bfb153f-caac-471c-ba94-4b799a30a9c4)

Check if Network Level Authentication is enabled. 

![image](https://github.com/user-attachments/assets/9565a389-1920-4e10-8d9c-6f52b877f4cb)

Set an account lock out policy to lock an account for a certain number of incorrect guesses which will prevent any brute force attacks.

![image](https://github.com/user-attachments/assets/d05babff-3e12-47f3-8dbe-f1e3c5b48e70)

Configure complex password rules in the Password Policy. 

![image](https://github.com/user-attachments/assets/25e418f8-7e68-4a43-b25e-57fac02ecb14)

**Secure Shell Control Implementation and Testing:**
In this section, SSH control security practices such as firewall rules configurations for SSH remote access and changing the default SSH config file options were implemented and evaluated.

Configuring firewall inbound rules for OpenSSH SSH Server in Windows VM.

![image](https://github.com/user-attachments/assets/3d37490f-47ea-4d7e-8ba9-57aa4c110fa1)

Connect to Windows VM from Linux VM from the terminal using the below command.

![image](https://github.com/user-attachments/assets/37f9dd03-8bab-40ba-a4ae-6737f9d75b86)

Remove the SSH access for the Kali Linux VM, and add another IP address as shown below.

a.	  Windows Defender Firewall with Advanced Security->Inbound rules->
      OpenSSH SSH Server->Properties->Scope->Remote IP Address->
      These IP Addresses->Select Linux VM Ip address->Remove.

      Windows Defender Firewall with Advanced Security->Inbound rules->
      OpenSSH SSH Server->Properties->Scope>Remote IP Address
      These IP Addresses->Enter 10.10.10.10

Try connecting to Windows VM from Kali Linux terminal using SSH again, with both Administrator and user accounts.
Could not connect via SSH.

![image](https://github.com/user-attachments/assets/a3ac1c04-ec75-4567-b82f-c10393a32b1e)

![image](https://github.com/user-attachments/assets/f75ada19-caa1-4e10-97e4-783f138eda1e)

Implementing and testing SSH security practices while connecting to Kali Linux VM from Windows VM using Secure Shell.

Open Power shell application in Windows VM and connect to Kali Linux VM over SSH using the below command

![image](https://github.com/user-attachments/assets/4abf3ceb-80bc-4f29-8fb4-f4790211cd2d)
Permission denied, as connecting via SSH with root account disabled by default.

Go to Kali Linux VM and change the below options in the ssh config file using this command in the Linux terminal.
sudo mousepad  /etc/ssh/sshd_config
Edit both PermitRootLogin and PasswordAuthentication to yes and save the file.
![image](https://github.com/user-attachments/assets/0a3a1b89-409c-4afa-a2d3-c7c81f565e3b)

Run the command systemctl restart ssh and try connecting to Kali Linux VM again from the power shell in the Windows VM with the root account.
The connection was successful.

![image](https://github.com/user-attachments/assets/91375d82-cce2-437b-8458-48553e75cf93)

Add a new user and test SSH access for that account.

![image](https://github.com/user-attachments/assets/3a32c4d7-e376-4a07-b197-53a13052af3b)

![image](https://github.com/user-attachments/assets/d6c44ef0-12fd-40b5-9a34-1aa85d5a4807)

The connection was successful. 

Restrict access to Linux VM using IPTables.
Run sudo iptables -L to show a list of IPTables in the Linux terminal.
![image](https://github.com/user-attachments/assets/9d48fc74-6f89-4756-9d0b-bf0f6e9bdb90)

Validate if Windows VM can still SSH to Kali Linux VM and run exit to close the connection.
Run sudo iptables -A INPUT -p -tcp –dport 22 -j DROP
IPTables drops any connection from anywhere at port 22.
![image](https://github.com/user-attachments/assets/a70dfb3c-cc28-4793-a7d7-4448a7efed05)

Run the command sudo iptables –L again and check for the new rule in the INPUT rule set.
![image](https://github.com/user-attachments/assets/fc9e5c82-ae4f-48c1-92b3-5621bea37d72)

Try connecting to Linux VM from Windows VM via SSH again.
Connection failed.

![image](https://github.com/user-attachments/assets/f034ea24-28e3-4b1b-a232-6e309c37718f)

Remove the added IPTables rule from the rule set by executing the following command and check the INPUT rule set again.
sudo iptables –D INPUT 1 
sudo iptables -L

![image](https://github.com/user-attachments/assets/e80a6674-0a4d-4afa-b069-29ce23882824)

Connect to Linux VM again from Windows VM power shell application via SSH.
Both root and student1 SSH access connections were successful.

![image](https://github.com/user-attachments/assets/deaf0280-27f3-4ff0-8047-5481224503b8)

**Conclusion:** Remote access controls are critical for ensuring secure connections to systems, especially with protocols like Remote Desktop Protocol (RDP) and Secure Shell (SSH). By implementing practices such as strong authentication, restricting access, keeping systems updated, and monitoring activity, we can secure their remote access infrastructure and protect against unauthorized access and potential attacks. 




































