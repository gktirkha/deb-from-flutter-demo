# **MAKING A DEB PACKAGE FROM FLUTTER PROJECT**
**MAKING A DEB PACKAGE FROM FLUTTER PROJECT**

This documentation is about how to make a .deb package from flutter
project

**About deb package:** deb package is a software package file commonly
used on Debian based Linux distributions like ubuntu, mint, kali etc. it
is just like as .exe file for Windows

**Prerequisites:** 
==================

1.  Running Debian based / Ubuntu based Linux.

2.  Basic knowledge of commands like cp mv rm rmdir touch and tar
    commands or knowledge of file management in Linux i.e., creating
    folder / file, deleting folder / file, extracting tar files.

3.  Flutter project having root folder name same as package name
    mentioned in pubspec.yaml

4.  If the flutter project is dependent on some dependencies like
    dart\_vlc is dependent on vlc and vlc-devel, so these two packages
    should be installed on the operating system on which you wand to
    build the app

**Setup:**
==========

1.  We're going to use
    [flutter\_to\_debian](https://pub.dev/packages/flutter_to_debian)
    pub package so, first the following command in a terminal\
    ***dart pub global activate flutter\_to\_debian***

2.  If it is the first time you are installing a pub package as a global
    package, open **\~/.bashrc** file and paste following at end of the
    file\
    *export PATH=\"\$PATH\":\"\$HOME/.pub-cache/bin\" and close all the
    existing terminals like terminals in vs code or gnome terminal
    etc.,*

**Steps To Create .deb package:**
=================================

1.  Create the following file structure in flutter project

***Flutter Project root (root directory of flutter project that contains
lib linux android etc.)***

\|

+\-\-- debian (Folder)

\|

\|

+\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\-\- gui
(Folder)

\| &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;\|

\| &nbsp; &nbsp; &nbsp;*Appname*.desktop (File)

Debian.yaml (File)

replace Appname in Appname.desktop with your application name. keep in
mind it should be unique like me.gtirkha.demo.desktop\

2.  ***open debian.yaml in a text editor***

    a.  copy the following boiler plate in debian.yaml file (Indent
        Sensitive) and follow the following instructions [copy](https://github.com/gtirkha1312/deb-from-flutter-demo/blob/master/debian/debian.yaml)

flutter\_app:

command:

arch: x64

parent:

control:

Package:

Version: 1.0.0

Architecture: amd64

Essential: no

Priority: optional

Depends:

Maintainer:

Description:

***Assign following value accordingly***

b.  ***command*** specify the flutter package name as mentioned in
    pubspec.yaml file

c.  ***parent*** specify the location where you want to extract the
    contents of the deb package after installation\
    commonly used locations are /opt, /bin, /usr/share/bin\
    (/opt is the best practice)

d.  ***Package*** any unique package name like me.gtirkha.demo

e.  ***Version*** specify your app version

f.  ***Depends*** if your project needs some Linux dependencies
    (packages) to be installed prior to installation, then specify them
    here, if there are multiple dependencies, separate them with a comma
    (",")

g.  ***Maintainer*** you can specify your GitHub home page, website or
    your name, although it is optional but leaving it empty will lead in
    future warnings while installations.

h.  ***Description*** it is an optional argument where you can specify
    what your app does like visual text editor, multiplayer game built
    on flutter etc

***After assigning value it should look like following***

flutter\_app:

command: demo\_application

arch: x64

parent: /opt

control:

Package: me.gtirkha.demo

Version: 1.0.0

Architecture: amd64

Essential: no

Priority: optional

Depends: vlc , vlc-devel

Maintainer: Gautam Tirkha

Description: Application built on flutter

3.  ***Open Appname.desktop file in a text editor***

    a.  copy the following boiler plate (Case sensitive) [copy](https://github.com/gtirkha1312/deb-from-flutter-demo/blob/master/debian/gui/me.gtirkha.desktop)

\[Desktop Entry\]

Version=

Name=

GenericName=

Comment=

Terminal=false

Type=Application

Categories=

Keywords=

Icon=

***Assign following value accordingly***

b.  ***Version*** Version number of your application

c.  ***Name*** Flutter application name, it would be visible when user
    opens the application or searches for it

d.  ***GeneriName*** it is an optional parameter; you may put it same as
    Name as you did in 3.c

e.  ***Comment*** you may tell what app does here like a demo
    application etc., depending on the desktop environment the user is
    using, it may or may not be displayed

f.  ***Categories*** you may set it as web, game, development,
    entertainment, productivity etc. it is an optional argument you may
    leave it empty or delete it\

g.  ***Keywords*** these are the keywords through which a user will be
    able to find your app easily on start menu. Suppose we want our
    application to be displayed in search results when a user searches
    for flutter, we can assign flutter as a value to keyword argument.
    We can have multiple keywords separated by semicolon (";")

h.  ***Icon*** the icon we want to display when a user searches for the
    application.\
    the method mentioned in the documentation did not work for me, so I
    came up with other solution\
    we can access our assets folder as following\
    Icon=/**parent**/**Package**/data/flutter\_assets\
    replace parent and package with the values you assigned in steps
    **2.c** and **2.d,** suppose we have our icon in
    ***Flutterprojectroot/assets/logo***, and **parent is /opt** and
    **package is demo,** we may access image file as\
    ***Icon=/opt/demo**/data/flutter\_assets/**assets/icons/logo/splash\_screen\_logo.svg***\
    *Make sure that folder containing icon is mentioned in pubspec.yaml
    assets*\
    *like*\
    *assets:*\
    *- assets/icons/logo/*

***After assigning value it should look like following***

*\[Desktop Entry\]*

*Version=1.0*

*Name=Demo App*

*GenericName= Demo App*

*Comment=Demo Linux App*

*Terminal=false*

*Type=Application*

*Categories=Utility*

*Keywords=Flutter;share;demo;texteditor;*

*Icon=/opt/iprep/data/flutter\_assets/assets/icons/logo/splash\_screen\_logo.sv*

4.  Form the root of your flutter project run ***flutter build linux
    --release && flutter\_to\_debian,*** after successful completion you
    can get deb file from project root/debian/packages\
>    ***Note:*** The generated deb file through this process cannot be
>    installed using gui package manager and can only be installed using
>    command line in terminal.

To install the .deb file run following command

***sudo dpkg -i /path/to/.deb && sudo apt -f install -y***

>   ***Note:*** during installation prompt the installer will ask if the
>   user wants to install the package or not, it accepts the process only
>   and only if, if the user types yes (lower case) and hit enter. Typing y
>   will lead to failure of installation

**uninstalling:** If you want to uninstall the .deb file use the
following command

***sudo apt autoremove \--purge Package***

*(Replace package with the value you defined in* *2.d. in debian.yaml)*

**Creating a deb file that can be installed using gui package manager
and removing confirmation prompt**

***Prerequisites***

1.  *.deb file generated from the above procedure*

**Steps to create gui instable deb package**

>   **Note:** I Am considering my debian file name as xyz.deb , replace it
>   with yours

1.  Place the deb file in an empty folder

2.  Open that folder in terminal and follow the following steps in this
    terminal

3.  Run ***ar x xyz.deb && rm xyz.deb*** in terminal

4.  You will now have 4 files in the folder

    a.  control.tar.zst

    b.  data.tar.zst

    c.  debian-binary

5.  run ***tar -xvf control.tar.zst && rm control.tar.zst***

6.  You Will now have 4 files in folder

    a.  control

    b.  preinst

    c.  data.tar.zst

    d.  debian-binary

7.  open preinst in any text editor app and replace its whole content
    with **\#!/bin/bash**

> ***Note:*** this file is executed before the installation, you can put
> commands that are needed to be executed before the installation like
> installing dependencies, although it is optional as most of the Debina
> distributions automatically install dependencies mentioned in
> debian.yaml but if you still want to be sure you can put commands in
> this file. For example, pub package dart\_vlc requires two Linux
> packages vlc and vlc-devel so we can put following command ***sudo apt
> install vlc vlc-devel*** in file, the file will look like following
>
> \#!/bin/bash
>
> sudo apt install vlc vlc-devel

8.  now in gui window select control and data.tar.zst then right click
    and select compress.

9.  Make sure you select to compress as .tar.*something (the name should
    contain .tar e.g., Xyz.tzr.xy or xyz.tzr.zst and so on)*

10. delete control and preinst file (***rm control && rm preinst***)
    ,you should be left with 3 files

11. You should be left with 3 files

    a.  debian-binary

    b.  data.tar.zst

    c.  control.tar.***something***

12. Rename the file generated in step 9 as control.tar.something\
    ***for example***

    a.  archive.tar.zst -\> control.tar.zst

    b.  xyz.tar.gz -\> control.tar.gz

    c.  package.tar.xy -\>

13. Run the following command:\
    ***ar r output.deb debian-binary control\* data\****\
    this will generate the output.deb file which can be published and
    send anywhere and can be installed with any gui package manger like
    discover gnome-package-manager etc\
    this process will also remove the confirmation prompt where user had
    to type yes to install the package (it was coded in preinst file
    where we removed this prompt)

14. **uninstalling:** If you want to uninstall the .deb file use the
    following command\
    ***sudo apt auto remove --purge Package***\
    *(Replace the package with the value you defined at step* *2.d. in
    debian.yaml)*
