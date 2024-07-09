LanternFlyTVOS
==========

.. image:: media/LanternFlyTVOS.png?raw=true
.. :scale: 25 %
.. :alt: LanternFlyTVOS temporary logo July 2024

A `Raspberry Pi <http://www.raspberrypi.org/>`_ distribution to display webpages in full screen. It includes `LanternFlyTV Cloud Browser`_ out of the box and the scripts necessary to load it at boot.
This repository contains the source script to generate the distribution out of an existing `Raspbian <http://www.raspbian.org/>`_ distro image.

LanternFlyTVOS is a derivative of FullPageOS and joined the distros that use `CustomPiOS <https://github.com/guysoft/CustomPiOS>`_.

.. _Release notes:

v0.0.4 (2024-Jul-09) - Experimental release
-------------------------------------------

* Trying to get the LanterFlyTV Cloud Browser to run on the x server
* Build process on Github Actions is now working
* Announcements once I have a working version. Let's hope it's this one

Where to get it?
----------------

Take a look at the `actions` section. 

Very early versions. Please be patient.

How to use it?
--------------

#. Unzip the image and install it to an SD card `like any other Raspberry Pi image <https://www.raspberrypi.org/documentation/installation/installing-images/README.md>`_
#. Configure your WiFi by editing ``LanternFlyTVOS-wpa-supplicant.txt`` on the first partition of the flashed card when using it like a flash drive
#. Boot the Pi from the SD card
#. Log into your Pi via SSH (it is located at ``LanternFlyTVOS.local`` `if your computer supports bonjour <https://learn.adafruit.com/bonjour-zeroconf-networking-for-windows-and-linux/overview>`_ or the IP address assigned by your router), default username is "pi", default password is "raspberry" and change the password using the ``passwd`` command. Consider also changing the vnc password as well by `x11vnc -storepasswd`.

Requirements
------------
* Raspberry Pi 4 and newer or device running Armbian. Older Raspberry Pis are not currently supported.
* SD card, 4GB or larger, Class 10. 
* 2A power supply


Features
--------

* Ships with `LanternFlyTV Cloud Browser <https://github.com/tailorvj/LanternFlyTV>`_
* Loads LanternFlyTV Browser at boot in full screen
* Onboarding info is displayed in the browser when it's first connected to the network
* URLs are rotated according to a schedule from a list of URLs in the cloud database
* Define wifi settings in a config file
* Ships with preconfigured `X11VNC <http://www.karlrunge.com/x11vnc/>`_, for remote connection (password 'raspberry')
* Specify a custom Splashscreen that gets displayed in the booting process instead of Kernel messages/text

Developing
----------

The easiest way to have a working build system is using Github Actions. 

This also conserves resources on your local machine.

* Fork the repo 
* Clone to a workstation
* Create a feature branch
* Push
* Create Pull request
* Approve Pull request
* Automatic build should start
* Download the image from the actions tab

Local Requirements
~~~~~~~~~~~~

#. `qemu-arm-static <http://packages.debian.org/sid/qemu-user-static>`_
#. `CustomPiOS <https://github.com/guysoft/CustomPiOS>`_
#. Downloaded `Raspbian <http://www.raspbian.org/>`_ image.
#. root privileges for chroot
#. Bash
#. realpath
#. sudo (the script itself calls it, running as root without sudo won't work)
#. jq (part of CustomPiOS dependencies)

Build LanternFlyTVOS From within LanternFlyTVOS / Raspbian / Debian / Ubuntu
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

LanternFlyTVOS can be built from Debian, Ubuntu, Raspbian, or even LanternFlyTVOS.
Build requires about 2.5 GB of free space available.
You can build it by issuing the following commands::

    sudo apt install coreutils p7zip-full qemu-user-static
    
    git clone https://github.com/guysoft/CustomPiOS.git
    git clone https://github.com/tailorvj/LanternFlyTVOS.git
    cd LanternFlyTVOS/src/image
    wget -c --trust-server-names 'https://downloads.raspberrypi.org/raspios_lite_armhf_latest'
    cd ..
    ../../CustomPiOS/src/update-custompios-paths
    sudo modprobe loop
    sudo bash -x ./build_dist
    
Building LanternFlyTVOS Variants
~~~~~~~~~~~~~~~~~~~~~~~~

LanternFlyTVOS supports building variants, which are builds with changes from the main release build. An example and other variants are available in the folder ``src/variants/example``.

To build a variant use::

    sudo bash -x ./build_dist [Variant]
    
    
Building Using Docker
~~~~~~~~~~~~~~~~~~~~~~
`See Building with docker entry in wiki <https://github.com/guysoft/CustomPiOS/wiki/Building-with-Docker>`_

    
Building Using Vagrant
~~~~~~~~~~~~~~~~~~~~~~
There is a vagrant machine configuration to let build LanternFlyTVOS in case your build environment behaves differently. Unless you do extra configuration, vagrant must run as root to have nfs folder sync working.

Make sure you have a version of vagrant later than 1.9!

If you are using older versions of Ubuntu/Debian and not using apt-get `from the download page <https://www.vagrantup.com/downloads.html>`_.

To use it::

    sudo apt-get install vagrant nfs-kernel-server virtualbox
    sudo vagrant plugin install vagrant-nfs_guest
    sudo modprobe nfs
    cd LanternFlyTVOS/src/vagrant
    sudo vagrant up

After provisioning the machine, it's also possible to run a nightly build which updates from devel using::

    cd LanternFlyTVOS/src/vagrant
    run_vagrant_build.sh
    
To build a variant on the machine simply run::

    cd LanternFlyTVOS/src/vagrant
    run_vagrant_build.sh [Variant]

Usage
~~~~~

#. If needed, override existing config settings by creating a new file ``src/config.local``. You can override all settings found in ``src/config``. If you need to override the path to the Raspbian image to use for building OctoPi, override the path to be used in ``ZIP_IMG``. By default, the most recent file matching ``*-raspbian.zip`` found in ``src/image`` will be used.
#. Run ``src/build_dist`` as root.
#. The final image will be created in ``src/workspace``


Remote access
~~~~~~~~~~~~~

Remote GUI access can be achieved through VNC Viewer. Get the IP of your raspberry ``hostname -I`` via SSH. 

The password is ``raspberry`` and is independent of password you have set for your user(s). Change the password by ``x11vnc -storepasswd`` via SSH.

Code contribution would be appreciated!

License
-------

LanternFlyTVOS is licensed under the GPL v3.0 license. See the LICENSE file for details.

LanternFlyTVOS  Copyright (C) 2024. All rights reserved to Asaf Pri Hadash tailorvj.com
This program comes with ABSOLUTELY NO WARRANTY; for details see the LICENSE file.