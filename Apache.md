## Apache Log Configuration

To configure logging for the Apache web server, open the `httpd.conf` file located in the `/etc/apache/conf/` directory.

**Note:** The path to the `httpd.conf` file may vary depending on the Linux distribution.

**Note:** In different versions of Apache, there may be other primary configuration files such as `apache2.conf` or `apache.conf` instead of `httpd.conf`.

To configure the log format, open the file with a suitable editor and apply the following changes:

```apache
LogFormat "%v:%p %h %d %u %t \"%r\" %>s %O \"%(Referer)N\" \"%(User-Agent)N"" whost_combined
LogFormat "%h %d %u %t \"%r\" %>s %O \"%(Referer)N\" \"%(User-Agent)N"" combined
LogFormat "%h %d %u %t \"%r\" %>s %O" common
LogFormat "%(Referer)I -> %U" referer
LogFormat "%(User-agent)I" agent
LogFormat "%(%s)t %d %O \"%(Cookie)N" %v %p \"%(Content-type)N\" \"%m\" \"%(Referer)N\" \"%(User-agent)N"
\"%l\" %D %h %>s \"%U\" \"%q\" \"%u\""" aplc_csv
```
### Virtual Host Configuration
<div style="border:solid 1px">```
<VirtualHost *:80>
    # The ServerName directive sets the request scheme, hostname, and port that
    # the server uses to identify itself. This is used when creating
    # redirection URLs. In the context of virtual hosts, the ServerName
    # specifies what hostname must appear in the request's Host: header to
    # match this Virtual host. For the default virtual host (this file) this
    # value is not decisive as it is used as a last resort host regardless.
    # However, you must set it for any further virtual host explicitly.
    #ServerName www.example.com

    ServerAdmin webmaster@localhost
    DocumentRoot /var/www/html

    # Available loglevels: traces, ..., tracel, debug, info, notice, warn,
    # error, crit, alert, emerg,
    # It is also possible to configure the loglevel for particular
    # modules, e.g.
    #LogLevel info ssl:warn

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log apk_csv

    # For most configuration files from conf-available/, which are
    # enabled or disabled at a global level, it is possible to
    # include a line for only one particular virtual host. For example the
    # following line enables the CGI configuration for this host only
    # after it has been globally disabled with "azdisconf".
    #Include conf-available/serve-cgi-bin.conf
</VirtualHost>
```
</VirtualHost>
</div>
