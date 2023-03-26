It is easy to set up new websites running custom webservers in the lab.  Here is how.

The `https://baulab.us/` webserver is run from a tiny diskless $99 computer in David's office at Khoury, and all the experiments that we run including `memit.baulab.us` and `demystify.baulab.us` run from there.  It also serves the `baulab.us/u/username` and `baulab.us/p/projectname` subdirectories for instant websites.  You can get to that physical machine by `ssh bauserver`.

How can that little computer do so much?  The trick is, it doesn't actually run much, but it defers to NFS for most of everything, and it just serves as an HTTP proxy for anything nontrivial.


## Static file websites without a custom domain.

The easiest way to put content on the web is to just put your website content in a special directory name on the file server.

There are two special filename patterns:

```
/share/u/[username]/www/[dir/file.html]
             ->  http://baulab.us/u/[username]/[dir/file.html]
/share/projects/[projectname]/www/[dir/file.html]
             ->  http://baulab.us/p/[projectname]/[dir/file.html]
```

In other words, you can just create files and directories inside your `www` directory inside your home directory or your project directory, and these will be served directly on subdirectories of `baulab.us`.  Be sure to `chmod a+rX` any files and directories you wish to serve.  The webserver runs as the user `www-data` that doesn't have permissions to read anything unless you explicitly give it that permission.

Don't plan to run any server-side scripts using this method, however, and do not use this to post anything that you expect thousands of people to download.  Keep in mind that `baulab.us` is a shared $99 computer, that we all share.  It does not have much capacity to think very hard, and it also doesn't have much bandwidth.

If you have content that might consume a lot of bandwidth, work with David to post that content on `baulab.info` which is hosted at Google's cloud service and which has effectively infinite bandwidth.  More on that later.

## Custom websites with a custom domain.

OK but sometimes we will have a website like `memit.baulab.us` where we want to run GPU computations or something.  We also run those on the same bauserver, but in those cases, the server is just acting like a forwarding proxy.

Here is how it is set up.

To see what subdomains are set up, `ssh bauserver` and `ls /etc/apache2/sites-enabled`.  Currently as I write this page, I will see the following (but as we add and delete websites, it will be different):

```
001-memit.conf  002-shell.conf  004-demystify.conf  006-files.conf  999-default.conf
```

Each file sets up a subdomain `foo.baulab.us` for a particular site.  For example, do the following to see a very simple forwarding proxy for the demystify user study experiments:

```
$ cat 004-demystify.conf

<VirtualHost *:443>
    ServerName demystify.baulab.us
    ProxyPass / http://kyoto.khoury.northeastern.edu:3000/
    ProxyPassReverse / http://kyoto.khoury.northeastern.edu:3000/
    SSLEngine on
    Protocols h2 http/1.1
    SSLCertificateFile /etc/letsencrypt/live/baulab.us/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/baulab.us/privkey.pem
    # Include /etc/letsencrypt/options-ssl-apache.conf
</VirtualHost>
```

If you look at the memit one, you can see it's slightly more complicated and actually has two subdomains,
one for serving static content, and the other for fowarding backend traffic to a custom server.

```
$ cat 001-memit.conf
<VirtualHost *:443>
    ServerName memit.baulab.us
    ServerAlias *memit.baulab.us
    ServerAdmin webmaster@localhost
    DocumentRoot /share/u/jadenfk/deployment/memit/dist

    # We log memit in separate log files because we had a bunch
    # of stuff to debug.
    # LogLevel info ssl:warn

    ErrorLog ${APACHE_LOG_DIR}/memit_error.log
    CustomLog ${APACHE_LOG_DIR}/memit_access.log combined

    <Directory />
        # Enable .htaccess settings
        AllowOverride none
        Options Indexes FollowSymLinks
        ServerSignature off
        Require all granted
    </Directory>

    SSLEngine on
    Protocols h2 http/1.1
    SSLCertificateFile /etc/letsencrypt/live/baulab.us/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/baulab.us/privkey.pem
    Include /etc/letsencrypt/options-ssl-apache.conf
</VirtualHost>

<VirtualHost *:443>
    ServerName memitbe.baulab.us
    ProxyPass / http://karakuri.khoury.northeastern.edu:5000/
    ProxyPassReverse / http://karakuri.khoury.northeastern.edu:5000/
    SSLEngine on
    Protocols h2 http/1.1
    SSLCertificateFile /etc/letsencrypt/live/baulab.us/fullchain.pem
    SSLCertificateKeyFile /etc/letsencrypt/live/baulab.us/privkey.pem
    # Include /etc/letsencrypt/options-ssl-apache.conf
</VirtualHost>
```

To create your own custom server, first prototype it on your own machine on a high-number port like 5000, and then when it's ready to deploy, create a configuration like this and then work with David to put it on `bauserver`.


