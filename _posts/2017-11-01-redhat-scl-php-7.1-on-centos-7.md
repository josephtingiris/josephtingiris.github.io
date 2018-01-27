---
layout: post
title: "How to install the RedHat Software Collections Library PHP 7.1 on CentOS 7"
author: "Joseph Tingiris"
date: 2017-11-01 12:18:00 -0400
last_modified_at: 2018-01-27 09:25:00 -0400
categories:
- howto
- install
- php 7
- centos 7
tags:
- howto
- install
- php
- php 7
- php 7.1
- centos
- centos 7
- centos 7.0
- centos 7.1
- centos 7.2
- centos 7.3
- centos 7.4
- centos 7.5
- mysqlnd
- software collections
- software collections library
- software collections library online
- scl
- sclo
- sclo-rh
- centos-sclo-rh
published: true
---

This is a functional reference guide for Enterprise Linux, specifically CentOS 7, that attempts to provide a straightforward procedure for producing a highly functional, stable, and modern LAMP 7 stack (i.e. HOWTO).

# Install the CentOS EPEL & RedHat Software Collections Library (scl) release packages, clean, & rebuild the yum cache.
```
sudo yum clean all
sudo yum makecache
sudo yum install epel-release centos-release-scl
```

# Install the latest RedHat SCL php 7.1 & corresponding, dependent httpd24 packages.

The following command installs the most commonly required php modules, as well.
```
sudo yum install rh-php71 rh-php71-php rh-php71-php-bcmath rh-php71-php-cli rh-php71-php-common rh-php71-php-dba rh-php71-php-embedded rh-php71-php-enchant rh-php71-php-fpm rh-php71-php-gd rh-php71-php-intl rh-php71-php-ldap rh-php71-php-mbstring rh-php71-php-mysqlnd rh-php71-php-odbc rh-php71-php-pdo rh-php71-php-pear rh-php71-php-pecl-apcu rh-php71-php-pgsql rh-php71-php-process rh-php71-php-pspell rh-php71-php-recode rh-php71-php-snmp rh-php71-php-soap rh-php71-php-xml rh-php71-php-xmlrpc sclo-php71-php-imap sclo-php71-php-mcrypt sclo-php71-php-pecl-amqp sclo-php71-php-pecl-apcu-bc sclo-php71-php-pecl-apfd sclo-php71-php-pecl-geoip sclo-php71-php-pecl-http sclo-php71-php-pecl-igbinary sclo-php71-php-pecl-imagick sclo-php71-php-pecl-lzf sclo-php71-php-pecl-memcached sclo-php71-php-pecl-mongodb sclo-php71-php-pecl-msgpack sclo-php71-php-pecl-propro sclo-php71-php-pecl-raphf sclo-php71-php-pecl-redis sclo-php71-php-pecl-selinux sclo-php71-php-pecl-solr2 sclo-php71-php-pecl-uploadprogress sclo-php71-php-pecl-uuid sclo-php71-php-pecl-xattr sclo-php71-php-pecl-xdebug sclo-php71-php-tidy httpd24 httpd24-mod_ssl
```

# Verify the php 7.1 installation.

*At this point, there should be a fully functional httpd 2.4 with php 7.1 (using the mysqlnd driver, etc).*
```sh
/opt/rh/rh-php71/root/usr/bin/php --ini
```
```
Configuration File (php.ini) Path: /etc/opt/rh/rh-php71
Loaded Configuration File:         /etc/opt/rh/rh-php71/php.ini
Scan for additional .ini files in: /etc/opt/rh/rh-php71/php.d
Additional .ini files parsed:      /etc/opt/rh/rh-php71/php.d/15-xdebug.ini,
/etc/opt/rh/rh-php71/php.d/20-bcmath.ini,
/etc/opt/rh/rh-php71/php.d/20-bz2.ini,
/etc/opt/rh/rh-php71/php.d/20-calendar.ini,
/etc/opt/rh/rh-php71/php.d/20-ctype.ini,
/etc/opt/rh/rh-php71/php.d/20-curl.ini,
/etc/opt/rh/rh-php71/php.d/20-dba.ini,
/etc/opt/rh/rh-php71/php.d/20-dom.ini,
/etc/opt/rh/rh-php71/php.d/20-enchant.ini,
/etc/opt/rh/rh-php71/php.d/20-exif.ini,
/etc/opt/rh/rh-php71/php.d/20-fileinfo.ini,
/etc/opt/rh/rh-php71/php.d/20-ftp.ini,
/etc/opt/rh/rh-php71/php.d/20-gd.ini,
/etc/opt/rh/rh-php71/php.d/20-gettext.ini,
/etc/opt/rh/rh-php71/php.d/20-iconv.ini,
/etc/opt/rh/rh-php71/php.d/20-imap.ini,
/etc/opt/rh/rh-php71/php.d/20-intl.ini,
/etc/opt/rh/rh-php71/php.d/20-json.ini,
/etc/opt/rh/rh-php71/php.d/20-ldap.ini,
/etc/opt/rh/rh-php71/php.d/20-mbstring.ini,
/etc/opt/rh/rh-php71/php.d/20-mcrypt.ini,
/etc/opt/rh/rh-php71/php.d/20-mysqlnd.ini,
/etc/opt/rh/rh-php71/php.d/20-odbc.ini,
/etc/opt/rh/rh-php71/php.d/20-pdo.ini,
/etc/opt/rh/rh-php71/php.d/20-pgsql.ini,
/etc/opt/rh/rh-php71/php.d/20-phar.ini,
/etc/opt/rh/rh-php71/php.d/20-posix.ini,
/etc/opt/rh/rh-php71/php.d/20-pspell.ini,
/etc/opt/rh/rh-php71/php.d/20-recode.ini,
/etc/opt/rh/rh-php71/php.d/20-shmop.ini,
/etc/opt/rh/rh-php71/php.d/20-simplexml.ini,
/etc/opt/rh/rh-php71/php.d/20-snmp.ini,
/etc/opt/rh/rh-php71/php.d/20-soap.ini,
/etc/opt/rh/rh-php71/php.d/20-sockets.ini,
/etc/opt/rh/rh-php71/php.d/20-sqlite3.ini,
/etc/opt/rh/rh-php71/php.d/20-sysvmsg.ini,
/etc/opt/rh/rh-php71/php.d/20-sysvsem.ini,
/etc/opt/rh/rh-php71/php.d/20-sysvshm.ini,
/etc/opt/rh/rh-php71/php.d/20-tidy.ini,
/etc/opt/rh/rh-php71/php.d/20-tokenizer.ini,
/etc/opt/rh/rh-php71/php.d/20-xml.ini,
/etc/opt/rh/rh-php71/php.d/20-xmlwriter.ini,
/etc/opt/rh/rh-php71/php.d/20-xsl.ini,
/etc/opt/rh/rh-php71/php.d/20-zip.ini,
/etc/opt/rh/rh-php71/php.d/30-mysqli.ini,
/etc/opt/rh/rh-php71/php.d/30-pdo_mysql.ini,
/etc/opt/rh/rh-php71/php.d/30-pdo_odbc.ini,
/etc/opt/rh/rh-php71/php.d/30-pdo_pgsql.ini,
/etc/opt/rh/rh-php71/php.d/30-pdo_sqlite.ini,
/etc/opt/rh/rh-php71/php.d/30-wddx.ini,
/etc/opt/rh/rh-php71/php.d/30-xmlreader.ini,
/etc/opt/rh/rh-php71/php.d/30-xmlrpc.ini,
/etc/opt/rh/rh-php71/php.d/40-amqp.ini,
/etc/opt/rh/rh-php71/php.d/40-apcu.ini,
/etc/opt/rh/rh-php71/php.d/40-apfd.ini,
/etc/opt/rh/rh-php71/php.d/40-geoip.ini,
/etc/opt/rh/rh-php71/php.d/40-igbinary.ini,
/etc/opt/rh/rh-php71/php.d/40-imagick.ini,
/etc/opt/rh/rh-php71/php.d/40-lzf.ini,
/etc/opt/rh/rh-php71/php.d/40-msgpack.ini,
/etc/opt/rh/rh-php71/php.d/40-propro.ini,
/etc/opt/rh/rh-php71/php.d/40-raphf.ini,
/etc/opt/rh/rh-php71/php.d/40-selinux.ini,
/etc/opt/rh/rh-php71/php.d/40-uploadprogress.ini,
/etc/opt/rh/rh-php71/php.d/40-uuid.ini,
/etc/opt/rh/rh-php71/php.d/40-xattr.ini,
/etc/opt/rh/rh-php71/php.d/50-apc.ini,
/etc/opt/rh/rh-php71/php.d/50-http.ini,
/etc/opt/rh/rh-php71/php.d/50-memcached.ini,
/etc/opt/rh/rh-php71/php.d/50-mongodb.ini,
/etc/opt/rh/rh-php71/php.d/50-redis.ini,
/etc/opt/rh/rh-php71/php.d/50-solr.ini
```

# Additional Steps

There are some things that I've found make using the RedHat PHP 7.1.x SCL packages and upgrading existing code bases a little easier.  *The following steps are **OPTIONAL**, but recommended.*

## To avoid potential conflict, ensure the CentOS base httpd is stopped, disabled, and completely removed from the system.

**_I'd strongly recommend removing the CentOS base php & httpd._**  Having the base packages on the system, with SCL packages, can cause confusion.  Although, to be fair, it's possible as long as both httpd installs are configured to use different interfaces and/or ports.
```sh
sudo systemctl stop httpd
sudo systemctl disable httpd
```

It's also important to note that, by default and because, both base & SCL httpd packages are configured to bind to all interfaces on port 80 (& port 443 if mod_ssl is installed).   As a result, only one at a time will start successfully.  If you're not doing this then I'm sure you know what I mean & how to fix that.
```sh
yum remove httpd httpd-tools php*
```

## Create a custom systemd httpd.service file.

This will allow for a normal Enterprise Linux administrative 'feel' and retain functionality such as `systemctl restart httpd`.  It will also override the default CentOS base httpd (if it exists on the system) and allow for local customizations.  It's helpful to me, sometimes.  

The tendency for both httpd & httpd24 to use the same ports can be frustrating, especially for web developers who are unfamiliar with SCL.  Ultimately, I'd recommend this and **_I do not recommend having both of the CentOS base httpd & SCL httpd24 on the same system_**.  Nevertheless, if you do this then remember to run `systemctl daemon-reload` after creating /etc/systemd/system/httpd.service

### /etc/systemd/system/httpd.service
```
cat <<EOF > /etc/systemd/system/httpd.service
[Unit]
Description=The RedHat Software Collections Library (SCL) Apache HTTP Server
After=network.target remote-fs.target nss-lookup.target
Documentation=man:httpd(8)
Documentation=man:apachectl(8)

[Service]
Type=notify
EnvironmentFile=/etc/sysconfig/httpd
ExecStart=/opt/rh/httpd24/root/usr/sbin/httpd-scl-wrapper \$OPTIONS -DFOREGROUND
ExecReload=/opt/rh/httpd24/root/usr/sbin/httpd-scl-wrapper \$OPTIONS -k graceful
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
EOF
```

After creating or updating /etc/systemd/system/httpd.service then systemd needs to be reloaded.  This, or a reboot, is mandatory every time a custom /etc/systemd service file is modified.

```sh
sudo systemctl daemon-reload # important!
sudo systemctl status httpd
```
```
● httpd.service - The RedHat Software Collections (SCL) Apache HTTP Server
   Loaded: loaded (/etc/systemd/system/httpd.service; disabled; vendor preset: disabled)
   Active: inactive (dead)
     Docs: man:httpd(8)
           man:apachectl(8)
```

## Create (or modify) the systemctl EnvironmentFile for httpd.service

Doing this will cause the SCL httpd24 to re-use the familiar `/etc/httpd` structure (rather than what's in `/opt/rh/httpd24/root/etc/httpd`).

### /etc/sysconfig/httpd
```
cat <<EOF > /etc/sysconfig/httpd
OPTIONS="-d /etc/httpd"
LANG=C
EOF
chmod 644 /etc/sysconfig/httpd

```

## Copy the httpd24 files to /etc/httpd and modify them.

**_PAY MORE ATTENTION TO THE FINAL CONFIGURATIONS THAN IS OUTLINED HERE._**


If using `/etc/httpd`, then be sure to update the contents of `/etc/httpd` to reflect what's in `/opt/rh/httpd24/root/etc/httpd`.  I typically move the old /etc/httpd out of the way, rsync the SCL configs, and then modify what's in `/etc/httpd` because that preserves the integrity of the SCL packaged configs.
```sh
if [ -d /etc/httpd ] && [ ! -d /etc/httpd.base ]; then (mv /etc/httpd /etc/httpd.base); fi # artifacts may exist, e.q. conf.d/squid.conf
if [ ! -f /etc/httpd/conf/httpd.conf ]; then (rsync -avp /opt/rh/httpd24/root/etc/httpd/ /etc/httpd/); fi # don't accidentally overwrite httpd.conf (et al)
```

For new installs, I typically prefer `/var/www` as a starting point for _DocumentRoot_.  It's also important that the _ServerRoot_ directive is changed in `httpd.conf`, to reference `/etc/httpd`, so that the correct config structure is used.
```sh
for httpd24_conf in $(find /etc/httpd -type f | xargs grep -l "/opt/rh/httpd24/root"); do echo $httpd24_conf; sed -i -e 's#/opt/rh/httpd24/root/etc#/etc#g' -e 's#/opt/rh/httpd24/root/var/www#/var/www#g' $httpd24_conf; done
```

I also prefer the standard `/var/log/httpd` directory for logs, and usually ensure the `/etc/httpd/logs` link points there.
```sh
rm /etc/httpd/logs && ln -s /var/log/httpd /etc/httpd/logs
```

This can also be helpful, if for nothing more than preserving the original SCL config.  After years and years of `/etc/php.ini`, it's become finger memory for many.  It's hard coded into lots of stuff that looks for it here, too.
```sh
if [ ! -f /etc/php.ini ]; then (cp /etc/opt/rh/rh-php71/php.ini /etc/php.ini); fi # don't accidentally overwrite php.ini
grep -q -i -F '^PHPIniDir' /etc/httpd/conf.d/rh-php71-php.conf || (echo; echo 'PHPIniDir /etc/php.ini') >> /etc/httpd/conf.d/rh-php71-php.conf
```

## On development machines that may be building C/C++ packages that require installed php headers, install the corresponding devel packages.

These are usually not needed (or recommended) on production systems.
```
yum install rh-php71-php-devel rh-php71-php-pecl-apcu-devel rh-php71-scldevel sclo-php71-php-pecl-http-devel sclo-php71-php-pecl-igbinary-devel sclo-php71-php-pecl-imagick-devel sclo-php71-php-pecl-msgpack-devel sclo-php71-php-pecl-propro-devel sclo-php71-php-pecl-raphf-devel
```

## Update systemd then enable, start, and check the status of httpd24-htcacheclean & httpd24-httpd (and/or httpd).

The CentOS base only contains a single service named _httpd_, whereas the SCL httpd24 package contains two services, _httpd24-htcacheclean_ and _httpd24-httpd_.  If you've gotten this far then the following depends on whether or not a custom _httpd_ service was added (above).  Again, I usually use a custom httpd.service.
```sh
sudo systemctl daemon-reload # can't hurt!
sudo systemctl enable httpd24-htcacheclean
sudo systemctl start httpd24-htcacheclean
sudo systemctl status httpd-htcacheclean
sudo systemctl enable httpd # or, httpd24-http
sudo systemctl start httpd # or, httpd24-http
sudo systemctl status httpd # or, httpd24-http
```

This is mostly for any other binaries that may be installed, that depend on the shared SCL libraries. For them to always work properly with the RedHat SCL packages, then the following files should be configured manually.  Anything compiled and linked with ld will find them.

## Add the rh-php71 shared library directories to the ld linker search path via /etc/ld.so.conf.d/rh-php71.conf

### /etc/ld.so.conf.d/rh-php71.conf

```
/opt/rh/rh-php71/root/usr/lib64
/opt/rh/httpd24/root/usr/lib64
/opt/rh/rh-php71/root/usr/lib
/opt/rh/httpd24/root/usr/lib
```

### Prefix the rh-php71 binary directories to all login PATHs via /etc/profile.d/zzzz-scl.sh

This is may be helpful.  Naming a profile.d include 'zzzz-scl.sh' will ensure it gets loaded last. Existing **PATH**s should be appended & preserved.

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
export PATH=${PATH_SCL}:$PATH
```

# Caveats

*The following information may be helpful.*

Many packages, programs, & utilities (and people) expect the default CentOS 7 filesystem locations for associated files.  The RedHat SCL packages relocate everything to `/opt/rh/`.  

* On CentOS 7 machines with *only* rh-php-7.1 installed, it may be easiest to use a symbolic link.  However, it should **not** be done on machines that choose to have the default php and scl php co-exist on the same machine.

    ```
    ln -s /opt/rh/rh-php71/root/usr/bin/php /usr/bin/php
    ```

* Best practice, for shell scripts in general, is to use the `/usr/bin/env <processor>` directives rather than hard coding something like `#!/usr/bin/php`.  This is a good practice.

    `#!/usr/bin/env php`

* Here's a quick one liner to find all files from the current directory that contain `/bin/php`; they may need to be changed to ``#!/usr/bin/env php`` (rather than relying on a symbolic link).  

    ```
    find . -type f | xargs grep -l "/bin/php"
    ```

# DISCLAIMER

**Many SCL packages are available and may have been installed if you're blindly copy/pasting the commands in this guide. Some are 'rh' (maintained by RedHat), while others are 'sclo' (maintained by the Software Collections organization [not RedHat]).  Some are required & others may not be needed.**

**Jumping into php 7 will enable some newer things to work, but if it isn't broke, then why fix it?  The reality is that php 5.4 has been around for a long time and by now is very stable.  It's still supported by RedHat and will be for many more years.**

**Or, if & when you're paranoid about security;  I'd recommend sticking with the CentOS 7 base php 5.4 & trust RedHat and the CentOS community.  Long term stability and security updates are the main reason a lot of us use RedHat or derivitives like CentOS, anyway.**



# REFERENCES

* https://access.redhat.com/solutions/2662201
* http://wiki.centos.org/SpecialInterestGroup/SCLo
* https://www.softwarecollections.org/en/
* https://access.redhat.com/security/updates/backporting
