---
layout: post
title: "How to install RedHat Software Collections Library PHP 7 on CentOS 7"
author: "Joseph Tingiris"
date: 2016-08-28 07:19:59 -0400
last_modified_at: 2017-10-21 16:03:00 -0400
categories:
- howto
- install
- php 7
- centos 7
tags:
- howto
- install
- php
- php 7.0
- centos
- centos 7
- centos 7.0
- centos 7.1
- centos 7.2
- centos 7.3
- centos 7.4
- software collections library
- scl
published: true
---

# Ensure httpd is stopped and disabled.

```
systemctl stop httpd
systemctl disable httpd
```

# Install the CentOS EPEL & Software Collections Library (scl) release packages, clean, & rebuild the yum cache.

```
yum install epel-release centos-release-scl
yum clean all
yum makecache
```

# Install the latest RedHat SCL php 7.0 & corresponding, dependent httpd24 packages.

The following command installs the most commonly required php modules, as well.

```
yum install rh-php70 rh-php70-php rh-php70-php-bcmath rh-php70-php-cli rh-php70-php-common rh-php70-php-dba rh-php70-php-embedded rh-php70-php-enchant rh-php70-php-fpm rh-php70-php-gd rh-php70-php-intl rh-php70-php-ldap rh-php70-php-mbstring rh-php70-php-mysqlnd rh-php70-php-odbc rh-php70-php-pdo rh-php70-php-pear rh-php70-php-pgsql rh-php70-php-process rh-php70-php-pspell rh-php70-php-recode rh-php70-php-snmp rh-php70-php-soap rh-php70-php-xml rh-php70-php-xmlrpc sclo-php70-php-imap sclo-php70-php-mcrypt sclo-php70-php-pecl-apcu sclo-php70-php-pecl-apcu-bc sclo-php70-php-pecl-memcached sclo-php70-php-tidy httpd24 httpd24-mod_ssl
```

# Configure the necessary operating system files.

For **''bash''**, **''crond''**, & **''systemctl''** to work properly with the RedHat SCL php 7 packages, the following files MUST be configured manually.

#### /etc/ld.so.conf.d/rh-php70.conf

```
/opt/rh/rh-php70/root/usr/lib64
/opt/rh/httpd24/root/usr/lib64
/opt/rh/rh-php70/root/usr/lib
/opt/rh/httpd24/root/usr/lib
```

#### /etc/profile.d/zzzz-scl.sh

```bash
if [ -d /opt/rh/ ] && [ -r /opt/rh/ ]; then
    Rhscl_Roots=$(find /opt/rh/ -type f -name enable 2> /dev/null | sort -V)
    for Rhscl_Enable in $Rhscl_Roots; do
        if [ -r "$Rhscl_Enable" ]; then
            . "$Rhscl_Enable"
        else
            continue
        fi
        Rhscl_Root="$(dirname "$Rhscl_Enable")/root"
        Rhscl_Bins="usr/bin usr/sbin bin sbin"
        for Rhscl_Bin in $Rhscl_Bins; do
            if [ -d "$Rhscl_Root/$Rhscl_Bin" ]; then
                Scl_Path+="$Rhscl_Root/$Rhscl_Bin:"
            fi
        done
        unset Rhscl_Bin Rhscl_Bins  Rhscl_Enable Rhscl_Root
    done
    unset Rhscl_Roots
fi
export PATH_SCL=$Scl_Path
```

#### /etc/systemd/system/httpd.service

```
[Unit]
Description=The Apache HTTP Server
After=network.target remote-fs.target nss-lookup.target
Documentation=man:httpd(8)
Documentation=man:apachectl(8)

[Service]
Type=notify
EnvironmentFile=/opt/rh/httpd24/root/etc/sysconfig/httpd
ExecStart=/opt/rh/httpd24/root/usr/sbin/httpd-scl-wrapper $OPTIONS -DFOREGROUND
ExecReload=/opt/rh/httpd24/root/usr/sbin/httpd-scl-wrapper $OPTIONS -k graceful
ExecStop=/bin/kill -WINCH ${MAINPID}
# We want systemd to give httpd some time to finish gracefully, but still want
# it to kill httpd after TimeoutStopSec if something went wrong during the
# graceful stop. Normally, Systemd sends SIGTERM signal right after the
# ExecStop, which would kill httpd. We are sending useless SIGCONT here to give
# httpd time to finish.
KillSignal=SIGCONT
PrivateTmp=true

[Install]
WantedBy=multi-user.target
```


# Enable, start, and check the status of httpd.

```
systemctl daemon-reload # important!
systemctl enable httpd
systemctl start httpd
systemctl status httpd
```

# Verify locations of php ini files.

*At this point, there should be a fully functional httpd 2.4 with php 7.0 (using the mysqlnd driver, etc).*

`php --ini`

```
Configuration File (php.ini) Path: /etc/opt/rh/rh-php70
Loaded Configuration File:         /etc/opt/rh/rh-php70/php.ini
Scan for additional .ini files in: /etc/opt/rh/rh-php70/php.d
Additional .ini files parsed:      /etc/opt/rh/rh-php70/php.d/20-bcmath.ini,
/etc/opt/rh/rh-php70/php.d/20-bz2.ini,
/etc/opt/rh/rh-php70/php.d/20-calendar.ini,
/etc/opt/rh/rh-php70/php.d/20-ctype.ini,
/etc/opt/rh/rh-php70/php.d/20-curl.ini,
/etc/opt/rh/rh-php70/php.d/20-dba.ini,
/etc/opt/rh/rh-php70/php.d/20-dom.ini,
/etc/opt/rh/rh-php70/php.d/20-enchant.ini,
/etc/opt/rh/rh-php70/php.d/20-exif.ini,
/etc/opt/rh/rh-php70/php.d/20-fileinfo.ini,
/etc/opt/rh/rh-php70/php.d/20-ftp.ini,
/etc/opt/rh/rh-php70/php.d/20-gd.ini,
/etc/opt/rh/rh-php70/php.d/20-gettext.ini,
/etc/opt/rh/rh-php70/php.d/20-iconv.ini,
/etc/opt/rh/rh-php70/php.d/20-imap.ini,
/etc/opt/rh/rh-php70/php.d/20-intl.ini,
/etc/opt/rh/rh-php70/php.d/20-json.ini,
/etc/opt/rh/rh-php70/php.d/20-ldap.ini,
/etc/opt/rh/rh-php70/php.d/20-mbstring.ini,
/etc/opt/rh/rh-php70/php.d/20-mcrypt.ini,
/etc/opt/rh/rh-php70/php.d/20-mysqlnd.ini,
/etc/opt/rh/rh-php70/php.d/20-odbc.ini,
/etc/opt/rh/rh-php70/php.d/20-pdo.ini,
/etc/opt/rh/rh-php70/php.d/20-pgsql.ini,
/etc/opt/rh/rh-php70/php.d/20-phar.ini,
/etc/opt/rh/rh-php70/php.d/20-posix.ini,
/etc/opt/rh/rh-php70/php.d/20-pspell.ini,
/etc/opt/rh/rh-php70/php.d/20-recode.ini,
/etc/opt/rh/rh-php70/php.d/20-shmop.ini,
/etc/opt/rh/rh-php70/php.d/20-simplexml.ini,
/etc/opt/rh/rh-php70/php.d/20-snmp.ini,
/etc/opt/rh/rh-php70/php.d/20-soap.ini,
/etc/opt/rh/rh-php70/php.d/20-sockets.ini,
/etc/opt/rh/rh-php70/php.d/20-sqlite3.ini,
/etc/opt/rh/rh-php70/php.d/20-sysvmsg.ini,
/etc/opt/rh/rh-php70/php.d/20-sysvsem.ini,
/etc/opt/rh/rh-php70/php.d/20-sysvshm.ini,
/etc/opt/rh/rh-php70/php.d/20-tidy.ini,
/etc/opt/rh/rh-php70/php.d/20-tokenizer.ini,
/etc/opt/rh/rh-php70/php.d/20-xml.ini,
/etc/opt/rh/rh-php70/php.d/20-xmlwriter.ini,
/etc/opt/rh/rh-php70/php.d/20-xsl.ini,
/etc/opt/rh/rh-php70/php.d/20-zip.ini,
/etc/opt/rh/rh-php70/php.d/30-mysqli.ini,
/etc/opt/rh/rh-php70/php.d/30-pdo_mysql.ini,
/etc/opt/rh/rh-php70/php.d/30-pdo_odbc.ini,
/etc/opt/rh/rh-php70/php.d/30-pdo_pgsql.ini,
/etc/opt/rh/rh-php70/php.d/30-pdo_sqlite.ini,
/etc/opt/rh/rh-php70/php.d/30-wddx.ini,
/etc/opt/rh/rh-php70/php.d/30-xmlreader.ini,
/etc/opt/rh/rh-php70/php.d/30-xmlrpc.ini,
/etc/opt/rh/rh-php70/php.d/40-apcu.ini,
/etc/opt/rh/rh-php70/php.d/40-igbinary.ini,
/etc/opt/rh/rh-php70/php.d/40-msgpack.ini,
/etc/opt/rh/rh-php70/php.d/50-apc.ini,
/etc/opt/rh/rh-php70/php.d/50-memcached.ini
```

# Optional Steps

*The following steps are **OPTIONAL**, but recommended.*

These are some things that will make using the RedHat PHP 7.0.x SCL packages and upgrading existing code bases a little easier.  


* On development machines that may be building C/C++ packages that require installed php headers, additionally install the corresponding devel packages.

    ```
    yum install rh-php70-php-devel rh-php70-scldevel sclo-php70-php-pecl-apcu-devel sclo-php70-php-pecl-http-devel
    ```

* Remove the default CentOS7 *httpd\** & *php\** packages & their dependencies.  This can help avoid confusion as to which version of php (& httpd) is actually in use.

    ```
    yum remove httpd httpd-tools php*
    ```

* Create (or modify) the systemctl EnvironmentFile for httpd.service.  By doing this, it will cause the scl httpd24 to re-use the existing `/etc/httpd` structure (rather than what's in `/opt/rh/httpd24/root/etc/httpd`).  If **`#OPTIONS= in /opt/rh/httpd24/root/etc/sysconfig/httpd`** and change it to the following, then be sure to update the contents of `/etc/httpd` to reflect what's in `/opt/rh/httpd24/root/etc/httpd`.

    ```
    OPTIONS="-d /etc/httpd"
    ```


# Caveats

*The following information may be helpful.*

Many packages, programs, & utilities (and people) expect the default CentOS 7 filesystem locations for associated files.  The RedHat SCL packages relocate everything to `/opt/rh/`.  

* If you chose to update the `/opt/rh/httpd24/root/etc/httpd` OPTIONS then make sure there are no base httpd artifacts left in `/etc/httpd`.  Using rsync can be helpful in this case, i.e.

    ```
    rsync -avp /opt/rh/httpd24/root/etc/httpd/ /etc/httpd/ --delete-after
    ```

* On CentOS 7 machines with *only* rh-php-7.0 installed, it may be easiest to use a symbolic link.  However, it should **not** be done on machines that choose to have the default php and scl php co-exist on the same machine.

    ```
    ln -s /opt/rh/rh-php70/root/usr/bin/php /usr/bin/php
    ```

* Best practice, for scripts in general, is to use tge `/usr/bin/env <processor>` directives rather than hard coding something like `#!/usr/bin/php`.  

    `#!/usr/bin/env php`

* Here's a quick one liner to find all files from the current directory that contain ''bin/php''; they may need to be changed to ``#!/usr/bin/env php`` (rather than relying on a symbolic link).  

    ```
    find . -type f -exec gawk 'FNR>10 {nextfile} /bin\/php/ { print FILENAME ; nextfile }' {} \;
    ```

* If open source scripts or packages are found using hard coded shebang conventions then please report them to their author(s) and have them changed upstream.


# REFERENCES

* https://access.redhat.com/solutions/2662201
* http://wiki.centos.org/SpecialInterestGroup/SCLo

