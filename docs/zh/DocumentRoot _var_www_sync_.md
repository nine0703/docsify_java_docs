```bash
<VirtualHost *:443>

  DocumentRoot /var/www/webdav/ 
  <Directory /var/www/webdav/> 
  Options Indexes MultiViews 
  AllowOverride None 
  Order allow,deny 
  allow from all 
  </Directory> 
  Alias /webdav /var/www/webdav 
  <Location /webdav>
  DAV On 
  AuthType Basic 
  AuthName "webdav" 
  AuthUserFile /etc/apache2/webdav.user.passwd 
  Require valid-user 
</Location>
     ErrorDocument 404 /error.html
     ServerName www.rcilabarary.space
     SSLEngine off
     SSLCertificateFile /etc/apache2/ssl/server.crt
     SSLCertificateKeyFile /etc/apache2/ssl/server.key
     SSLCertificateChainFile /etc/apache2/ssl/ca.crt
</VirtualHost>
```

=========================================================

```
<VirtualHost *:80>
# The ServerName directive sets the request scheme, hostname and port that
# the server uses to identify itself. This is used when creating
# redirection URLs. In the context of virtual hosts, the ServerName
# specifies what hostname must appear in the request's Host: header to
# match this virtual host. For the default virtual host (this file) this
# value is not decisive as it is used as a last resort host regardless.
# However, you must set it for any further virtual host explicitly.
#ServerName www.example.com
     ServerAdmin webmaster@localhost
     DocumentRoot /var/www/html
# Available loglevels: trace8, ..., trace1, debug, info, notice, warn,
# error, crit, alert, emerg.
# It is also possible to configure the loglevel for particular
# modules, e.g.
#LogLevel info ssl:warn
     ErrorDocument 404 /error.html
     ErrorLog ${APACHE_LOG_DIR}/error.log
     CustomLog ${APACHE_LOG_DIR}/access.log combined
# For most configuration files from conf-available/, which are
# enabled or disabled at a global level, it is possible to
# include a line for only one particular virtual host. For example the
# following line enables the CGI configuration for this host only
# after it has been globally disabled with "a2disconf".
#Include conf-available/serve-cgi-bin.conf
     ServerName rcilabarary.space
     ServerName www.rcilabarary.space
     Redirect permanent / https://www.rcilabarary.space/
</VirtualHost>
        
#https Configuration
<VirtualHost *:443>
     ErrorDocument 404 /error.html
     ServerName www.rcilabarary.space
     SSLEngine off
     SSLCertificateFile /etc/apache2/ssl/server.crt
     SSLCertificateKeyFile /etc/apache2/ssl/server.key
     SSLCertificateChainFile /etc/apache2/ssl/ca.crt
</VirtualHost>
=====================================
<VirtualHost *:4560>
  DocumentRoot /var/www/webdav/ 
  <Directory /var/www/webdav/> 
  Options Indexes MultiViews 
  AllowOverride None 
  Order allow,deny 
  allow from all 
  </Directory> 
  Alias /webdav /var/www/webdav 
  <Location /webdav>
  DAV On 
  AuthType Basic 
  AuthName "webdav" 
  AuthUserFile /etc/apache2/webdav.user.passwd 
  Require valid-user 
</Location>
     ServerName www.rcilabarary.space
     SSLEngine off
     SSLCertificateFile /etc/apache2/ssl/server.crt
     SSLCertificateKeyFile /etc/apache2/ssl/server.key
     SSLCertificateChainFile /etc/apache2/ssl/ca.crt
</VirtualHost>

# vim: syntax=apache ts=4 sw=4 sts=4 sr noet
```