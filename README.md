# mail_conf_in_linux

reference
~~~
#!/bin/bash

# Email details
from_mail="baskeyvj03@gmail.com"
to_mail="vijaybaskar193@gmail.com"
subject="Disk Usage Alert at admin instance"

# Get disk usage for /dev/vda1
disk_usage=$(df / | grep / |awk '{print $5}'|sed 's/%//')

# Check if disk usage exceeds 85%
if [ "$disk_usage" -gt 85 ]; then
  echo "Your instance has reached ${disk_usage}% disk usage!" | mail -s "$subject" "$to_mail"
  echo "Disk usage report sent to $to_mail"
fi
~~~


Steps :

1. Install Mail Packages
 ~~~  
   yum install postfix mailx cyrus-sasl cyrus-sasl-plain -y
~~~
3. Create GMail Account
   a) Generate App Password

4. Edit sasl_password file and add gmail account details
~~~
   vi /etc/postfix/sasl_passwd
~~~
~~~
   [smtp.gmail.com]:587 <emailid>@gmail.com:<app password>
~~~
5. Generate DB file
~~~
   postmap /etc/postfix/sasl_passwd
~~~
7. Edit main.cf
~~~
   relayhost = [smtp.gmail.com]:587
   myhostname = <your server hostname >
   
   # Enable SASL authentication for postfix
   smtp_use_tls = yes                                                                                 
   smtp_sasl_auth_enable = yes   
   smtp_tls_security_level = encrypt
   smtp_tls_CAfile = /etc/ssl/certs/ca-bundle.crt
   
   # Disallow methods that allow anonymous authentication
   smtp_sasl_security_options = noanonymous    
   
   # Location of sasl_passwd   
   smtp_sasl_password_maps = hash:/etc/postfix/sasl_passwd
~~~
9. execute below commad to comment duplicate line
~~~
    
   sudo sed -i '/smtp_tls_CAfile = \/etc\/pki\/tls\/certs\/ca-bundle.crt/s/^/#/' /etc/postfix/main.cf
~~~
~~~
   sudo sed -i '/smtp_tls_security_level = may/s/^/#/' /etc/postfix/main.cf
~~~

11. Test EMAIL
~~~
   echo "Test Email" | mail -s "Send Email from Linux" sendmailfromlinux@gmail.com
~~~
