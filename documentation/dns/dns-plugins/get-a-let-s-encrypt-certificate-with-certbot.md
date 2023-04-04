---
title: get-a-let-s-encrypt-certificate-with-certbot
displayName: Certbot
published: true
order: 20
toc:
   --1--What is the Certbot plugin?: "what-is-the-certbot-plugin"
   --1--Install the plugin: "install-the-certbot-plugin"
   --1--Create credentials: "create-credentials-for-the-certbot-plugin"
   --1--Acquire a certificate: "acquire-a-certificate-via-the-certbot-plugin"
   --1--Renew a certificate: "renew-a-certificate"
---
  
  
  
  

What is the Certbot plugin?
---------------------------

The Certbot plugin automates Let’s Encrypt certificate issuance with Gcore DNS API.

**How it works.** To issue a Let’s Encrypt certificate for a domain, you need to confirm that you are its owner. You can do this in several ways (you can find more info in [Let’s Encrypt documentation](https://letsencrypt.org/docs/challenge-types)). One of the most common ways is a DNS-01 challenge that asks you to prove that you control the DNS for your domain name by creating a specific value in a TXT record under that domain name. You can use the Certbot plugin to send a request to issue a certificate. It automatically creates TXT records and subsequently removes them for obtaining, renewing, or revoking certificates. You can get more information about this in the [Certbot documentation](https://eff-certbot.readthedocs.io/en/stable/).

To use the Certbot plugin, you’ll need:

*   A domain with Gcore nameservers (_ns1.gcorelabs.net_ and _ns2.gcdn.services_) that the certificate is hosted on or delegated to
*   A Gcore account

Install the Certbot plugin
--------------------------

Below, we provide the commands for Certbot plugin installation on Ubuntu 22.04. If you have a different OS, the commands you need may differ a little.

1. Connect to your server via SSH.

2. Download and install the updates for outdated packages using the sudo command for package manager privileges. You need to upgrade to avoid errors when installing the plugin.

sudo apt update

3. Install the _pip3_ Python library if it’s missing. It is required to install Certbot.

sudo apt install python3-pip

4. Install the Certbot plugin using the command:

pip install certbot-dns-gcore

If you see the output _Successfully installed …_, installation is complete.

<img src="https://support.gcore.com/hc/article_attachments/9620506251025" alt="image1.png">

Create credentials for the Certbot plugin
-----------------------------------------

To use the Certbot plugin, you’ll need Gcore DNS API credentials. Without them, the Certbot plugin won’t be able to issue a certificate for your domain.

You can use two types of credentials: a permanent API token or the e-mail address and password associated with your Gcore account. We recommend using the first one because it’s the preferred option for integration via API. To receive the token, follow the [How to create, use and delete permanent token](https://gcore.com/support/articles/360018625617/) guide.

1. Go to the _home/user\_name_ directory:

cd ~/

2. Create the _gcore.ini_ file in the home directory. You can do this with _nano_ or any text editor. For example:

nano gcore.ini

3. Add the credentials into _gcore.ini_ as follows:

dns\_gcore\_apitoken = 0123456789abcdef...

where _0123456789abcdef..._ is your API token.

4. Restrict access to the _gcore.ini_ file to protect it from other users. To do this, use the following command:

chmod 600 gcore.ini

You’ve now added credentials.

Acquire a certificate via the Certbot plugin
--------------------------------------------

1. Run the following command to acquire the certificate:

certbot certonly --authenticator dns-gcore --dns-gcore-credentials=./gcore.ini --dns-gcore-propagation-seconds=30 -d '\*.example.com' --key-type ecdsa --logs-dir=. --config-dir=. --work-dir=.

where _\*.example.com_ is the domain and its subdomains for which you want to issue a Let’s Encrypt certificate. You also can specify _\*.example.com_ to issue a wildcard certificate for all subdomains.

2. The system will prompt you to enter your e-mail address for urgent renewal and security notifications. Specify your e-mail address. If you don’t do this, you’ll see the error _An e-mail address or --register-unsafely-without-email must be provided_.

Then enter **Y** to agree to the terms. After that, certificate issuance will start.

<img src="https://support.gcore.com/hc/article_attachments/9620506234257" alt="image2.png">

3. If the certificate for your domain is issued, you will see the following notification:

<img src="https://support.gcore.com/hc/article_attachments/9620491170833" alt="image3.png">

If you get the _Some challenges have failed_ error, try to increase the propagation time:

\--dns-gcore-propagation-seconds=80

where _80_ is a setting value.

Repeat the command from step 1 with the new value for propagation:

certbot certonly --authenticator dns-gcore --dns-gcore-credentials=./gcore.ini --dns-gcore-propagation-seconds=80 -d '\*.example.com' --key-type ecdsa --logs-dir=. --config-dir=. --work-dir=

If that didn’t work, make sure that the domain is delegated to Gcore nameservers using _dig_ utility or via online services. If not, change nameservers and wait 24 hours for the DNS to update. Then try to issue the certificate again.

Renew a certificate
-------------------

You need to renew the issued Let’s Encrypt certificate every 90 days. It can be done in two ways:

*   Manually with a command that attempts to renew any previously issued certificates that expire within 30 days. The working directory and the rest of the parameters should be the same for issuing.

certbot renew

*   (recommended) Automatically with the _cron_ daemon that maintains automated renewal in the background. You can find the commands for this method in [Certbot documentation](https://eff-certbot.readthedocs.io/en/stable/using.html?highlight=cron).