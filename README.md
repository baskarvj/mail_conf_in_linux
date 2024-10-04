# mail_conf_in_linux

Steps :

1. Install Mail Packages
   yum install postfix mailx cyrus-sasl cyrus-sasl-plain -y

2. Create GMail Account
   a) Generate App Password

3. Edit sasl_password file and add gmail account details

   vi /etc/postfix/sasl_passwd

   [smtp.gmail.com]:587 <emailid>@gmail.com:**********

5. Generate DB file

   postmap /etc/postfix/sasl_passwd
   
7. Edit main.cf

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

9. execute below commad to comment duplicate line
   sudo sed -i '/smtp_tls_CAfile = \/etc\/pki\/tls\/certs\/ca-bundle.crt/s/^/#/' /etc/postfix/main.cf
 
10. Test EMAIL   
   echo "Test Email" | mail -s "Send Email from Linux" sendmailfromlinux@gmail.com
