# RES-Auth0
Configuring AWS Research and Engineering Studio with Auth0 for SSO authentication.

# How to configure Auth0 to work with RES

Note: When you upgrade your environment to a new version of RES, you may have to reconnect the Auth0 integration by replacing URLs (outlined in steps Create an Auth0 Application for RES and onward). You may also need to point DNS records for portal and vdi webpages to any new application or network load balancers.

# Summary: 
Auth0 can integrate with Active Directory via an AD/LDAP connector running on a Windows server that’s domain joined to your AD. A summary of the steps are below:

* Create an Auth0 account, if you do not have one existing.
* Deploy a Windows-based EC2 instance to host the AD/LDAP Connector.
* Create and install the AD/LDAP Connector through Auth0 on the Windows instance.
* Create an Application for Research and Engineering Studio in the Auth0 console.
* Configure your AD/LDAP Connector to use SAML 2.0 to integrate with your RES application.

# Modify Security Group Settings
* Add an inbound security group rule to both your AdDomainWindowsNode and AdDomainAdminNode instances
    * Type: LDAP
    * Source: 0.0.0.0/0
 
# Deploy a Auth0-Connect-Node Instance
* Choose Launch an Instance
    * Name: (ex. Auth0-Connect-Node)
    * AMI: Microsoft Windows Server 2019 Base
    * Instance Type: t2.medium
    * Key Pair: (same key pair that you used to deploy RES)
    * VPC: (change VPC to match that of your RES deployment)
    * Open Advanced Details
        * Domain Join Directory: (join the AD domain used for RES)
        * IAM Instance Profile: AmazonSSMDirectoryServiceInstanceProfileRole (choose a role with AmazonSSMManagedInstanceCore and AmazonSSMDirectoryServiceAccess permissions)
* Add an inbound security group rule on this instance
    * Type: Custom TCP
    * Port: 443
    * Source: 0.0.0.0/0

# Access Auth0-Connect-Node
* Log in to the new Auth0-Connect-Node as an the RES Admin
    * Choose Connect
    * Navigate to the RDP Client tab
    * Choose Connect Using Fleet Manager
    * Choose Fleet Manager Remote Desktop
    * Add the user credentials for your Admin user of RES

# Change Security Settings on Desktop
* Open the Windows Start menu
* Click on Server Manager
* Click Local Server
* Select “On” next to IE Enhanced Security Configuration 
* Select “Off” for both Administrators and Users and Save

# Install Google Chrome (you can continue to use Explorer; however, this process is optimized on Google Chrome)
* Navigate to Internet Explorer and download Google Chrome
    * If Internet Explorer asks about Security Settings, select “Do not use recommended settings”.

# Create an Enterprise Connection in Auth0
* From Internet Explorer or Google Chrome, open Auth0 and log in to your account
* Navigate to Auth0 Dashboard > Authentication > Enterprise and select Active Directory/LDAP
* Select “Create Connection” 
* Enter a Connection Name and optional Display Name (ex. Auth0-AD-Connection)
* Leave the remaining option as default and Save
* From the resulting screen, copy the Provisioning Ticket URL and select “Install for Windows”





