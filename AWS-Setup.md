# AWS EC2 server Instance Creation and Access

## Prerequisite
- AWS account
	- https://aws.amazon.com/free (setup free account. credit card requires)
- Access to CLI (Command Line Interface) 
	- Windows - Powershell
	- mac - /Applications/Utilities
	- linux - You should already know. (ok for Ubuntu search for Terminal in 'Show Applications')

## System Setup
1. Create a Private / Public key pair. 
	- ```
		ssh-kegen
	- Use the defaults, by pushing enter through.
		- If you chose to change the path write down or save in a txt file the path.
	> Read more about ssh-keygen : https://www.ssh.com/academy/ssh/keygen
## Create Ubuntu Server
1. Login into AWS console. https://aws.amazon.com
	- ->Click on 'Sign in to the Console" button at top right. 
		- (If The top right button says 'Create an AWS Account') Click on the dropdown 'My Account' and select 'AWS Managment Console'
2. Access the EC2 console.
	- A. At the top bar search for 'EC2'. It should be the first item on the list.
	- B. Visually look for 'EC2' in the 'All Services' section. 
	- C. If you have already accessed the ec2 console it should/could be in the "Recently Visited Services" section.
3. Access the 'Instances' Section. 
	- A. If you see the lefthand navbar click on "instances"
	- B. In the main section under Resources click on "instances" or "instances (running)" if it is there.
4. Launch a new Instance
	1. Click on the 'Launch instances' button.
	2. Select Instances
		- In the search bar type "Ubuntu" and press enter.
		> For most other distrubutions of linux (Amazon Linux, SUSE etc) this guid would work.
	3.  The top one should be 'Ubuntu Server 20.04 LTS' make sure the 64-bit (x86) radio button is slected and press select.
	4. Choose instance size.
		- The t2.micro is free tier and it is normally pre-selected.
	5. (optional mostly for demostration) Setup Security Group.
		1. At the top select '6. Configure Security Group'.  
		2. Make sure "create a new security group" is selected and give it a name and description.
		3. On the Rule Section remove the one that says 'ssh' and port range '22' by pressing the x.
			>This is only for the demostration. You will need to have port 22 open to be able to access the server via ssh. 
		4. Click on 'Review and launch' button at the bottom or the link "review" at the top.
	6. Launch It
		- A. Without a Key
			1. Choose 'Proceed without a key pair'
			2. Check 'I ackowledge ....'
			3. Click Launch instance.
			4. click on view instances.

		- B. With a Key
			1. Select 'Create a new Key Pair'
			2. Give it a name
			3. Click 'Download Key Pair'
			4. Click 'Launch Instances'
5. Access the instance (Attempt)
	1. (Optional) Give the instance a name.
		- In the name column hover over the " - " and click the write icon.
	2. Wait until the 'Status check" says '2/2 checks passed'
	3. Click on the instance id 
	4. Click on the 'connect' button at the top right.
	5. If the tab 'EC2 Instance Connect' isn't selected click on it.
	6. Click Connect.
		- ...FAIL ?!?
		> Port 22 isn't open so even from the AWS console you cannot access the server.

6. Set security group permissions.
	1. On the left panel select 'Security Groups'.
	2. Click the security group id of the security group created when launching the instance.
	3. Click on 'Edit iunbound rules'
	4. Click on 'Add Rule'
	5. Select 'ssh' from the first dropdown. 
		- Or leave it at custom tcp and put in port 22. It's the same thing either way.
	6. Select 'Anywhere' from source.
		> If you choose my ip address it will fail in the next step.

7. Actually Connect!
	1. Repeat section 5. #3-#6.

8. Connect from your computer
	1. Copy the output of the below command.
		``` 
		head ~/.ssh/id_rsa.pub
	- A. With Key
		1. From the instances. Click on the checkbox beside the instance protected with PEM.
		2. Click the connect button at the top and take notice of the example.
		4. (in terminal) CD to the folder that the pem is downloaded into.
		5. past the copied line.
		6. WARNING: UNPROTECTED PRIVATE KEY FILE!
			- chmod 0700 'name of pem file'
		7. Try command again.
		8. We are in!
	- B. Without Key
		1. Connect like before from the ec2 instance connect.
	
	2. On the server
		```
		vim .ssh/authorized_keys 
	3. paste the public key contents.
		> if not familiar with vim you press 'i' to start typing then escape when done typing. :wq to write and quit vim.

		> If on the pem enabled server and want to keep the pem login possible, go to the bottom of the file and do not delete the current content.

	4. exit the server 
		``` 
		exit 
	5. ssh into the server :
		```
		ssh ubuntu@'ip address'
9. Extra security
	1. At the AWS console terminal click on the security groups under network and security.
	2. Find the security group and click on the id.
	3. Click on Edit inbound rules.
	4. On source Choose "My IP".
10. Lost / new computer
	1. Change the security group source back to anywhere.
	2. Connect (#5 3-6)

