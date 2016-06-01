---
layout: post
title: "Install cx-Oracle on Mavericks correctly"
date: 2014-07-14 21:44
categories: Programming
tags:
  - Python
  - Oracle
slug: install-cx-oracle-on-mavericks-correctly
---

![Oracle](/images/oracle-banner.jpg)

cx-Oracle is a Python Oracle DB connection package, ealier today I met some trouble installing on my macbook running Mavericks, after hacking I found the means to install it correctly:


1. Download `instantclient-basic-macos.x64-11.2.0.4.0.zip` and `instantclient-sdk-macos.x64-11.2.0.4.0.zip` from  [http://www.oracle.com/technetwork/topics/intel-macsoft-096467.html](http://www.oracle.com/technetwork/topics/intel-macsoft-096467.html).

2. Download the cx-Oracle source code from [http://cx-oracle.sourceforge.net](http://cx-oracle.sourceforge.net).

<!--more-->

3. Create a new directory to place the Oracle client files.

    mkdir -p /opt/oracle/

4. Move `instantclient-basic-macos.x64-11.2.0.4.0.zip` and `instantclient-sdk-macos.x64-11.2.0.4.0.zip` into the folder `/opt/oracle/`.

5. cd into `/opt/oracle`, unzip the two package.

    cd /opt/oracle/
        unzip instantclient-basic-macos.x64-11.2.0.4.0.zip
        unzip instantclient-sdk-macos.x64-11.2.0.4.0.zip

6. cd into `instantclient_11_2/sdk`, unzip the package `ottclasses.zip`.

    cd instantclient_11_2/sdk
        unzip ottclasses.zip

7. Copy some files.

        cd /opt/oracle/
        cp -R ./sdk/* .
        cp -R ./sdk/include/* .

8. Make some soft links so that cx-Oracle can correctly recognize the Oracle client.

        ln -s libclntsh.dylib.11.1 libclntsh.dylib
        ln -s libocci.dylib.11.1 libocci.dylib

9. Edit `~/.bash_profile`, add these lines.

        export ORACLE_HOME=/opt/opracle/instantclient_11_2
        export DYLD_LIBRARY_PATH=$ORACLE_HOME
        export LD_LIBRARY_PATH=$ORACLE_HOME

    then source it.

        source ~/.bash_profile

10. Now we have installed the Oracle client, it's cx-Oracle's turn.

    tar -xzvf cx_Oracle-5.1.2.tar.gz
        cd cx_Oracle-5.1.2.tar.gz
        sudo python setup.py build
        sudo python setup.py install

11. Now we can check it out whether cx-Oracle is successfully installed.

        >>> import cx_Oracle
        # If this works without a problem, then cx_Oracle is successfully installed.

If you have any questions, plz let me know in the comments.
