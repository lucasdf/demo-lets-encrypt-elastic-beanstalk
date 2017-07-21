# Demo: Let's Encrypt certificate with Elastic Beanstalk single-instance

This shows how to enable an SSL certificate from Let's Encrypt with AWS Elastic Beanstalk single-instance. The file *ssl.config* should go into the directory *.ebextensions*. It contains instructions for Apache to enable SSL and redirect traffic from HTTP to HTTPS. It also includes a cron job that will renew the certificate every week.

I created a [post](https://blog.lucasferreira.org/howto/2017/07/21/set-up-let-s-encrypt-ssl-certificate-with-aws-elastic-beanstalk-single-instance.html) in my blog explaining the file.

# Quick Notes

1. If you are not using Python 3.4 then you must edit the line 30 (python-path=...). For instance, if you are using Python 2.7 then this line should be replaced by `python-path=/opt/python/current/app:/opt/python/run/venv/lib/python2.7/site-packages:/opt/python/run/venv/lib64/python2.7/site-packages`.
2. If you are not using the file *application.py* to initialize the WSGI server then you must edit the line 23 (WSGIScriptAlias...) use your file.
3. You must setup the environment variables *LETSENCRYPT_EMAIL* and *LETSENCRYPT_DOMAIN* or replace its usage in the file.
4. If you are using an instance with enough RAM then you should delete the instructions *00_enable_swap* and *50_cleanup_swap*. These instructions are necessary when there is less than 512MB of RAM available to build Certbot, such as when using t2.nano or t2.micro instances. You can read more about it [here](https://certbot.eff.org/docs/install.html#problems-with-python-virtual-environment).
5. Traffic for port 443 must be allowed in the instance's security group.
6. To deploy it into production **remove the flag --staging** from the command */opt/certbot/certbot-auto certonly*
