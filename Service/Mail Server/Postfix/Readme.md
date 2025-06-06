### Linux Mail server With Postfix,Dovecote and RoundCube
To set up a complete Linux mail server on Ubuntu 22.04 using Postfix, Dovecot, and Roundcube, you need to install and configure each component properly. Here is a detailed step-by-step guide based on best practices and recent resources:

## 1. Prerequisites
- Ubuntu 22.04 server with root or sudo access
- A registered domain name with DNS A and MX records pointing to your server
- Basic Linux command-line knowledge

---

## 2. Install and Configure Postfix (SMTP Server)

Postfix handles sending and routing emails.

**Installation:**
```bash
sudo apt update
sudo DEBIAN_PRIORITY=low apt install postfix
sudo apt install postfix
```
During installation, select **Internet Site** and enter your domain name (e.g., mail.yourdomain.com).

**Basic Configuration:**
Edit `/etc/postfix/main.cf`:
```bash
sudo nano /etc/postfix/main.cf
```
Add or update the following:
```
myhostname = mail.yourdomain.com
mydomain = yourdomain.com
myorigin = /etc/mailname
mydestination = $myhostname, yourdomain.com, localhost.com, localhost
smtpd_relay_restrictions = permit_mynetworks, permit_sasl_authenticated, defer_unauth_destination
home_mailbox = Maildir/
inet_interfaces = all
inet_protocols = all
```
Save and restart Postfix:
```bash
sudo systemctl restart postfix
```

---

## 3. Install and Configure Dovecot (IMAP/POP3 Server)

Dovecot manages mail retrieval and storage.

**Installation:**
```bash
sudo apt install dovecot-core dovecot-imapd
```

**Configuration:**

- Set mail storage location:
```bash
sudo nano /etc/dovecot/conf.d/10-mail.conf
```
Set:
```
mail_location = maildir:~/Maildir
```

- Enable listening on all interfaces:
```bash
sudo nano /etc/dovecot/dovecot.conf
```
Add:
```
listen = *
```

- Configure authentication and SSL (edit `/etc/dovecot/conf.d/10-auth.conf` and `/etc/dovecot/conf.d/10-ssl.conf`) to require SSL/TLS for secure connections.

Restart Dovecot:
```bash
sudo systemctl restart dovecot
```

---

## 4. Install and Configure Roundcube (Webmail Client)

Roundcube provides a user-friendly web interface for email.

**Installation:**
```bash
sudo apt install roundcube roundcube-core roundcube-mysql roundcube-plugins
```
During installation, select **Apache2** as the web server and configure the database with dbconfig-common.

**Configure Apache to Serve Roundcube:**
Enable Roundcube configuration:
```bash
sudo ln -s /etc/roundcube/apache.conf /etc/apache2/conf-enabled/roundcube.conf
sudo systemctl restart apache2
```

Alternatively, you can add an alias in your Apache SSL site config:
```
Alias /mail /usr/share/roundcube
```

**Configure Roundcube to Connect to Postfix and Dovecot:**
Edit Roundcube config file:
```bash
sudo nano /etc/roundcube/config.inc.php
```
Set the IMAP and SMTP server:
```php
$config['default_host'] = 'mail.yourdomain.com';  // IMAP server
$config['default_port'] = 143;                    // IMAP port (use 993 for SSL)
$config['smtp_server'] = 'mail.yourdomain.com';  // SMTP server
$config['smtp_port'] = 25;                         // SMTP port (use 465 or 587 for SSL/TLS)
```

---

## 5. Secure Your Mail Server with SSL/TLS

Use Let’s Encrypt to obtain free SSL certificates:
```bash
sudo apt install certbot python3-certbot-apache
sudo certbot --apache -d mail.yourdomain.com
```

Configure Postfix and Dovecot to use SSL certificates by editing their respective config files (`main.cf` for Postfix, `10-ssl.conf` for Dovecot) to point to the certificate and key files.

---

## 6. Testing and Finalization

- Test sending and receiving emails using command line or Roundcube webmail.
- Check Postfix and Dovecot logs for errors.
- Ensure firewall allows ports 25 (SMTP), 143/993 (IMAP), 80/443 (HTTP/HTTPS).

---

This setup gives you a fully functional mail server with Postfix handling mail transfer, Dovecot managing mail storage and retrieval, and Roundcube providing a webmail interface. For detailed commands and configuration snippets, see the comprehensive guides from ServerStadium[1], Bitnami[2], Vultr[3], and Ubuntu-focused tutorials[6].

---


