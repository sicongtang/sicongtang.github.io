---
layout: post
title: "Python Environment Setup"
date: 2014-04-29 08:10:26 +0800
comments: true
categories: devenvsetup python
---
Python is a script language, the main idea for learning python is to utilize python to create easy hands-on automation script on linux env instead of shell.

Here, list the steps to configure PyDev, an eclipse plugin, inside eclipse environment.

1. Access PyDev webpage: http://pydev.org/manual_101_install.html
2. Download PyDev certificate
3. cd %JAVA_HOME%/bin
4. Run command: keytool.exe -import -file C:/download/pydev_certificate.cer -keystore %JAVA_HOME%/jre/lib/security/cacerts
5. Input default JDK cacerts password: changeit
6. Certificate installment completed
7. Download eclipse plugin on marketplace, or mannually download it and extract into dropins directory like: (dropins/pydev/eclipse/...)
8. Edit eclipse.ini, convert -vm=... JDK must be 1.7
9. Access JPython webpage: https://wiki.python.org/jython/InstallationInstructions
10. Download Jython 2.5.4rc1 - Traditional Installer at link: http://jython.org/downloads.html
11. Run command: java -jar jython_installer-2.5.2.jar --console
12. Configure JPython interceptor in PvDev eclipse plugin