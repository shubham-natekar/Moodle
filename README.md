# Moodle Documentation
- This documentation is for the bitnami application(moodle) which is running on AWS EC2 instance
- Instance type:t3.nano
- Platform Details: Linux/UNIX

## CONNECTING INSTANCE THROUGH TERMINAL

   - On the EC2 console start the instance by selecting 
    `Instance > Actions > Instance State > Start`

   - Once the instance is started cd into a terminal where the private key lies
   
   - And on the EC2 Console goto: 
   	`Actions > Connect `and copy the ssh command
<img src="https://github.com/shubham-natekar/Moodle/blob/master/images/connect-instance-1.png">	
   (NOTE: If the username mentioned as "root", which is followed by instances Public DNS, does
         not suffice the needs change it to "bitnami" or as prompted by the following screen.)
	 
   <img src="https://github.com/shubham-natekar/Moodle/blob/master/images/connect-instance-02.png">

   - On successfully connecting to instance the path changes to "bitnami@PrivateIP" of instance.
   
## FIND APPLICATION CREDENTIALS

### Option 1: Find Credentials By Checking The System Log On The AWS Cloud Console (EC2)

	After creating EC2 Moodle instance acquire username and password of 
	Moodle in `Action > Instance Setting > Get System Log`.
	
<img src="https://github.com/shubham-natekar/Moodle/blob/master/images/cm-app-credentials-3.png">
(NOTE: We can see this log only once when we stop an instance we won't see it again.)

### Option 2: Find Credentials By Connecting To Your Application Through SSH
    
    Connect to the application through SSH.
    Run the following command to see your application credentials:
`   sudo cat /home/bitnami/bitnami_credentials
`
<img src="https://github.com/shubham-natekar/Moodle/blob/master/images/cm-app-credentials-4.jpg">
   You can find more information from 'https://docs.bitnami.com/aws/faq/get-started/find-credentials/'.

## CHANGING CREDENTIALS

- Once logged in by default username and password you can update the password for ease of understanding by following the steps below :
   - Click on "Admin User" on top right corner > Profile
   - Actions Menu appears alongside > Change password
   - Follow the steps after and Save changes once done.
<img src="https://github.com/shubham-natekar/Moodle/blob/master/images/editprofile.jpg">

## CUSTOMIZATION OF APPLICATION

- After log in as superuser, we get `customizing this page` option on the top right corner using this we can customize our home page.

- The Site administrator can change theme settings and customize in `Site administration > Appearance > Themes`
    If we want to add a new theme then we can download the theme plugin from here 'https://moodle.org/plugins/'.

- We can install, update, and delete plugins in `Site administration > Plugins` in this section.

## UPLOAD USERS

- For adding multiple users we need to create text or CSV file and we have to mention the appropriate field in the file.
<img src="https://github.com/shubham-natekar/Moodle/blob/master/images/upload-user.jpg">

- After uploading users we can create cohorts and assign a role then we can enroll multiple users in the courses and add some restrictions on them.
<img src="https://github.com/shubham-natekar/Moodle/blob/master/images/cohorts.jpg">

- We can check the permission of the particular user in `Site administration > Users > Permissions > Check system permissions`.

## LANGUAGE SETTING

### Option 1: Install language pack By Simply select the languages from the list of available language packs.

- Over 100 language packs are available for an administrator to install on your Moodle site in 'Language packs' in the Site administration. Simply select the languages you require from the list of available language packs and click on the "Install selected language pack" button.

- Multiple language packs may be selected for install by holding down the Alt or Ctrl key whilst clicking on the language packs.

- Multiple language packs may be uninstalled, once selected, by clicking the 'Uninstall selected language packs" button.
<img src="https://github.com/shubham-natekar/Moodle/blob/master/images/installing_a_lang_pack.png">

### Option 2: Install language pack By Connecting To Your Application Through SSH.

- Download the language pack manually from [Language packs for Moodle 3.8.](https://docs.moodle.org/38/en/Language_packs) and unzip it to a directory called lang in your moodledata directory.

- To copy zip file from local machine to aws server use following command:
`
scp -i /directory/to/abc.pem /your/local/file/to/copy user@ec2-xx-xx-xxx-xxx.compute-1.amazonaws.com:path/to/file
`
<img src="https://github.com/shubham-natekar/Moodle/blob/master/images/filecopy.jpg">
eg. 
` scp -i ~/ShubhamMoodle.pem ~/Downloads/fr_ca.zip bitnami@ec2-15-207-59-127.ap-south-1.compute.amazonaws.com:/opt/bitnami/apps/moodle/moodledata/lang	
`
- Connect to the application through SSH and unzip the file in `/opt/bitnami/apps/moodle/moodledata/lang` .

### Language setup

- A user can set their preferred language via Preferences in the user menu (top right).
<img src="https://github.com/shubham-natekar/Moodle/blob/master/images/lang-menu.jpg">

- A manager or teacher can force the language of their course in `Course administration > Edit settings > Force language`.

- This list will show all installed language packs unless the site administrator has limited these in `Site administration > Languages > Language settings > Languages` on language menu in which case it will show only that list.

## ATTACH ELASTIC IP TO AN INSTANCE

- On your EC2 AWS Console goto:
`Network & Security > Elastic IPs > Allocate Elastic IP address > Allocate`

- Once getting the Elastic IP Select the Instance you want to attach IP to  and follow 
	` Actions > Networking > Associate Elastic IP Address `
<img src="https://github.com/shubham-natekar/Moodle/blob/master/images/associat-e-ip.jpg">

## TRANSITIONING TO HTTPS

- Converting an instance to https support you need to map instances IP to the domain

- As the instance change IP from each time you need to assign Elastic IP to your instance.

- After mapping the IP to domain follow the steps on the terminal which is connected through SSH to your instance.

- Follow the steps: 
	`sudo /opt/bitnami/bncert-tool`
	In the resulting screen add the domain name to which the IP is being mapped and follow the steps thereafter.
	Add the email which will be site administrators mail address which registers the site to the domain and notifies through the mail.
<img src="https://github.com/shubham-natekar/Moodle/blob/master/images/bnctl-https.jpg">

- And you have access to the site through the domain name.

## CLASSIFICATION OF COURSES FOR INSTITUTIONS/ORGANIZATION (one cannot see other)

 Category admin cannot see other categories and courses .

- Create a new role under "ARCHETYPE: Manager" `Site administration > Users > Define Roles > Add a new role`.
<img src="https://github.com/shubham-natekar/Moodle/blob/master/images/addnewrole.jpg">

	1. Assing role too any one user to make school administrator or you can assign multiple users to multiple categories.
	
	2. Hide all the category ( by clicking on eye symbol ) which will appear when we edit category.

- Make change on "student" role
	- `View hidden courses `: set	Allow
	- `See hidden categories `: set	Allow
	- `View list of courses you are not enrolled in `: set	Allow.
<img src="https://github.com/shubham-natekar/Moodle/blob/master/images/permissions.jpg">

- Assign all the student too student role.
