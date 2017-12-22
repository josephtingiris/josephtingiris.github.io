---
layout: post
title: "How to Download, Build, & Install Asterisk 13 on CentOS 7"
author: "Joseph Tingiris"
date: 2016-12-15 08:20:28 -0400
last_modified_at: 2017-12-22 12:30:00 -0400
categories:
- howto
- install
- asterisk 13
- centos 7
tags:
- bcg729
- dahdi
- digium
- g729
- gerrit
- howto
- install
- libpri
- linphone
- php
- asterisk 13
- gcc
- scl
published: true
---

At the time this was written, Asterisk 13 is the current LTS (long term support) version.

## Downloading, Building, & Installing Asterisk 13

The official documentation is here, [https://wiki.asterisk.org/wiki/display/AST/Building+and+Installing+Asterisk](https://wiki.asterisk.org/wiki/display/AST/Building+and+Installing+Asterisk)

This guide will primarily focus on downloading, building, customizing, and installing the most current version of Asterisk 13 on CentOS 7 with an open source G729 codec (that's not supplied with Asterisk 13).  Patching and/or rebuilding from Gerrit will be noted but are otherwise out of scope at this time.

### Prepare

In addition to a C compiler, Asterisk 13 also requires some specific system libraries.  Before Asterisk can be compiled, they must be installed.  For more information, see [https://wiki.asterisk.org/wiki/display/AST/System+Libraries](https://wiki.asterisk.org/wiki/display/AST/System+Libraries).

To _run_ the Asterisk 13 binaries compiled in this guide, on CentOS 7, *_only_* the following dependencies are actually required (i.e. an entire development environment is *not* required).

```bash
sudo yum install epel-release audiofile bluez-libs corosynclib curl freetds gmime gnutls gsm haveged help2man iksemel iksemel-utils jack-audio-connection-kit jansson libcurl libedit libical libnl libogg libsq3 libsqlite3x libsrtp libtiff libtool-ltdl libvorbis libxml2 libxslt lua mysql-connector-odbc mysql-utilities ncurses neon net-snmp net-snmp-utils newt openssl opus opusfile opus-tools patch pjproject portaudio postgresql-libs radiusclient-ng sox soxr spandsp speex speex-tools sqlite sqlite2 unbound-libs unixODBC uuid wget xmlstarlet SDL
```

**In addition** to the above packages, To _compile_ Asterisk 13 on CentOS 7, the following yum command will install all of the necessary development dependencies.

```bash
sudo yum install audiofile-devel autoconf automake bluez-libs-devel cmake3 corosynclib-devel doxygen-latex freetds-devel gcc git gmime-devel gnutls-devel gsm-devel gtk2-devel haveged-devel iksemel-devel jack-audio-connection-kit-devel jansson-devel kernel-devel libcurl-devel libedit-devel libical-devel libnl-devel libogg-devel libsq3-devel libsqlite3x-devel libsrtp-devel libtiff-devel libtool-ltdl-devel libvorbis-devel libxml2-devel libxslt-devel lua-devel mysql-devel ncurses-devel neon-devel net-snmp-devel newt-devel openssl-devel opus-devel opusfile-devel patch pjproject-devel pkgconfig portaudio-devel postgresql-devel radiusclient-ng-devel sox-devel soxr-devel spandsp-devel speex-devel speex-tools sqlite2-devel sqlite-devel unbound-devel unixODBC-devel uuid-devel SDL-devel
```

This guide will depend on a couple of environment variables, to make the procedure repeatable with 'copy/paste'.  It's assumed that these commands are run as _root_.

```bash
export BUILD_DIR=/usr/local/src/dps/build
export BUILD_PREFIX=/opt/dps
# to start from scratch ...
if [ -d "${BUILD_DIR}" ]; then mv "${BUILD_DIR}" "${BUILD_DIR}.$(date +%Y%m%d%H%M%S)" && mkdir -p "${BUILD_DIR}"; fi
if [ -d "${BUILD_PREFIX}" ]; then mv "${BUILD_PREFIX}" "${BUILD_PREFIX}.$(date +%Y%m%d%H%M%S)" && mkdir -p "${BUILD_PREFIX}"; fi
```

### Download the current version of Asterisk 13; choose a download method

There are a couple choices to get the source code from digium.  For the sake of simplicity, the public tar download repository is easier to work from and sufficicent for most new builds.  Maintaining concurrent builds and/or patching previous builds is possible but usually unnecessary.  I'd recommend just starting from scratch each time.

##### The public tar download repository for asterisk source, is available here:

[http://downloads.asterisk.org/pub/telephony/asterisk/](http://downloads.asterisk.org/pub/telephony/asterisk/)

For example, it is a good idea to download all files in an empty directory (even though they may not be used)
```bash
cd "${BUILD_DIR}"
wget http://downloads.asterisk.org/pub/telephony/asterisk/ChangeLog-13-current
wget http://downloads.asterisk.org/pub/telephony/asterisk/README-13-current
wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-13-current-patch.tar.gz
wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-13-current-patch.tar.gz.asc
wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-13-current-summary.html
wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-13-current-summary.txt
wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-13-current.md5
wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-13-current.sha1
wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-13-current.sha256
wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-13-current.tar.gz
wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-13-current.tar.gz.asc
wget http://downloads.asterisk.org/pub/telephony/libpri/libpri-current.tar.gz # optional
wget http://downloads.asterisk.org/pub/telephony/dahdi-linux/dahdi-linux-current.tar.gz # optional
wget http://downloads.asterisk.org/pub/telephony/dahdi-tools/dahdi-tools-current.tar.gz # optional
wget http://downloads.asterisk.org/pub/telephony/dahdi-linux-complete/dahdi-linux-complete-current.tar.gz
```

##### The semi-public Gerrit repository for asterisk revision control, is documented here:

[https://www.asterisk.org/downloads/source-code](https://www.asterisk.org/downloads/source-code)


```bash
# note; this is unnecessary if wget was used as above
cd "${BUILD_DIR}"
# git clone -b 13 http://gerrit.asterisk.org/asterisk asterisk-13
```

### Build the current version of Asterisk 13

The following steps assume wget was used as described above.

##### Verify asterisk-13-current.tar.gz is valid; compare the hashes from the following output
```bash
cd "${BUILD_DIR}"
cat asterisk-13-current.sha256
sha256sum asterisk-13-current.tar.gz
```

##### Untar asterisk-13-current.tar.gz into an empty directory
```bash
cd "${BUILD_DIR}"
tar zxvf asterisk-13-current.tar.gz
```

##### [OPTIONAL] Build & Install DHADI; this must be done prior to building LibPRI & Asterisk 13
```bash
cd "${BUILD_DIR}"
tar zxvf dahdi-linux-complete-current.tar.gz
cd dahdi-linux-complete-*
make distclean
DESTDIR="${BUILD_PREFIX}" make -C linux all # NOTE: This builds ONLY the drivers for the CURRENT running kernel
DESTDIR="${BUILD_PREFIX}" make -C linux install # NOTE: This installs ONLY the drivers for the CURRENT running kernel, but NOT in /usr
(cd tools && [ -f config.status ] || ./configure --prefix="${BUILD_PREFIX}" --with-dahdi=../linux)
make -C tools all
make -C tools install
make -C tools config
```

##### [OPTIONAL] Build & Install LibPRI; this must done prior to building Asterisk 13
```bash
cd "${BUILD_DIR}"
tar zxvf libpri-current.tar.gz
cd libpri-1*
make clean
DESTDIR="${BUILD_PREFIX}" LDFLAGS="-L${BUILD_PREFIX}/lib -L${BUILD_PREFIX}/usr/lib" CFLAGS="-I${BUILD_PREFIX}/include -I${BUILD_PREFIX}/usr/include" make
DESTDIR="${BUILD_PREFIX}" LDFLAGS="-L${BUILD_PREFIX}/lib -L${BUILD_PREFIX}/usr/lib" CFLAGS="-I${BUILD_PREFIX}/include -I${BUILD_PREFIX}/usr/include" make install
```


##### Configure Asterisk 13 source
```bash
cd "${BUILD_DIR}/asterisk-13."*
make distclean
./contrib/scripts/install_prereq install-unpackaged
./contrib/scripts/get_mp3_source.sh
LDFLAGS="-L${BUILD_PREFIX}/lib -L${BUILD_PREFIX}/usr/lib" CFLAGS="-I${BUILD_PREFIX}/include -I${BUILD_PREFIX}/usr/include" ./configure --with-pjproject-bundled --prefix="${BUILD_PREFIX}"
```

##### Select Asterisk 13 modules that will be compiled and check dependencies for various optional modules.
```bash
cd "${BUILD_DIR}/asterisk-13."*
make menuselect
```

This following are subjective; Typically, it may be useful to enable the following to be built, via menuselect.  Or, automatically enabled via menuselect/menuselect, see [https://wiki.asterisk.org/wiki/display/AST/Using+Menuselect+to+Select+Asterisk+Options](https://wiki.asterisk.org/wiki/display/AST/Using+Menuselect+to+Select+Asterisk+Options)

###### Add-ons
* chan_mobile
* chan_ooh323
* format_mp3
* res_config_mysql
* app_mysql
* cdr_mysql

###### Applications
* app_fax
* app_ivrdemo
* app_meetme
* app_saycounted
* app_setcallerid

###### Codec Translators
* codec_opus
* codec_silk
* codec_siren7
* codec_siren14
* DO NOT INSTALL CODEC g729a (it will be installed later)

###### Resource Modules
* res_fax
* res_mwi_external
* res_mwi_external_ami
* res_stasis_mailbox
* res_chan_stats
* res_corosync
* res_endpoint_stats
* res_fax_spandsp
* res_pktccops
* res_digium_phone

###### Compiler Flags
* G711_NEW_ALGORITHM
* G711_REDUCED_BRANCHING

###### Utilities
* aelparse
* astman
* check_expr
* check_expr2
* conf2ael
* muted
* smsq
* stereorize
* streamplayer

###### AGI Samples
* jukebox.agi


###### Core Sound Samples
* _Select ALL_ [for chosen language(s), e.g. EN & ES]

###### Music On Hold File Packages
* _Select ALL_ [for chosen language(s), e.g. EN & ES]

###### Extra Sound Packages
* _Select ALL_ [for chosen language(s), e.g. EN & ES]

##### Make Asterisk 13 (core)
```bash
cd "${BUILD_DIR}/asterisk-13."*
make
```

##### Install Asterisk 13 (core)
```bash
cd "${BUILD_DIR}/asterisk-13."*
make install
```

##### Make Asterisk 13 (documentation)
```bash
cd "${BUILD_DIR}/asterisk-13."*
make samples
make basic-pbx
make progdocs
```

##### Install Asterisk 13 (documentation)
```bash
cd "${BUILD_DIR}/asterisk-13."*
make install
```

##### Install G729 codec
```bash
cd "${BUILD_PREFIX}"
if [ -d lib64 ]; then mv lib64/* lib/ && rmdir lib64; fi
if [ ! -h lib64 ]; then ln -s lib lib64; fi

cd "${BUILD_DIR}"
git clone git://git.linphone.org/bcg729.git
cd bcg729
make clean
LDFLAGS='-L/opt/dps/lib -L/opt/dps/lib64' cmake3 . -DCMAKE_INSTALL_PREFIX=/opt/dps
make
make install

cd "${BUILD_DIR}"
wget http://asterisk.hosting.lv/src/asterisk-g72x-1.4.2.tar.bz2
bunzip2 -c asterisk-g72x-1.4.2.tar.bz2 | tar xvf -
cd asterisk-g72x-1.4.2/
LDFLAGS='-L/opt/dps/lib -L/opt/dps/lib64' ./autogen.sh
make distclean
LDFLAGS='-L/opt/dps/lib -L/opt/dps/lib64' ./configure --prefix=/opt/dps --with-bcg729
make
make install

```
