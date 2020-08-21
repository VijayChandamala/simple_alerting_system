# Simple alert system using smtp and shell script

## Step 1
-------------------

### install ssmtp

```sudo apt-get update && sudo apt install ssmtp -y```

## Step 2
------------------

### configure smtp to use your gmail as mail server

```sudo vim /etc/ssmtp/ssmtp.conf```

### example config:

``` # Config file for sSMTP sendmail
#
# The person who gets all mail for userids < 1000
# Make this empty to disable rewriting.
root=postmaster

# The place where the mail goes. The actual machine name is required no 
# MX records are consulted. Commonly mailhosts are named mail.domain.com
mailhub=mail.google.com:587
useSTARTTLS=YES
AuthUser=example@gmail.com
AuthPass=example
TLS_CA_File=/etc/pki/tls/certs/ca-bundle.crt

# Where will the mail seem to come from?
#rewriteDomain=

# The full hostname
hostname=example.com

# Are users allowed to set their own From: address?
 YES - Allow the user to specify their own From: address
 NO - Use the system generated From: address
 FromLineOverride=YES
 ```
 
 ## Step 3
 
 ### use smtp command in shell script to send alert mails
 
 ```#!/bin/bash  

if [ "$( docker ps | grep -o 'my_container' )" == "my_container" ];
then
echo "my_container is running"
else
echo "my_container is not running, sending an alert";
{ echo "To: alert@example.com"; echo "Subject: My container alert"; echo "My container is not running"; } | sudo ssmtp example@gmail.com,reciepient2@gmail.com;
fi
```
### above script sends a mail to example@gmail.com and reciepient2@gmail.com (you can add multiple reciepients seperated by a comma)

