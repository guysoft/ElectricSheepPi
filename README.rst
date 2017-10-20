ElectricSheepPi
===============


.. image:: http://unofficialpi.org/media/electric_sheep_pi_image2.jpg
.. :scale: 100 %
.. :alt: Image of Electric Sheep

A `Raspberry Pi <http://www.raspberrypi.org/>`_ distribution to to run `Electric Sheep : Crowdsourced Evolving Art <https://electricsheep.org/>`_ out of the box and the scripts necessary to load it at boot. This repository contains the source script to generate the distribution out of an existing `Raspbian <http://www.raspbian.org/>`_ distro image. `You can download a built image here <http://unofficialpi.org/Distros/ElectricSheepPi>`_

ElectricSheepPi is based on `CustomPiOS <https://github.com/guysoft/CustomPiOS>`_

Where to get it?
----------------

Official mirror is `here <http://unofficialpi.org/Distros/ElectricSheepPi>`_


How to use it?
--------------

#. Unzip the image and install it to an SD card `like any other Raspberry Pi image <https://www.raspberrypi.org/documentation/installation/installing-images/README.md>`_
#. Configure your WiFi by editing ``electricsheep-network.txt`` or ``electric-sheep-wpa-supplicant.txt`` on the root of the flashed card when using it like a flash drive
#. Boot the Pi from the SD card
#. If you have internet, Electric Sheep will start.
#. If needed Log into your Pi via SSH (it is located at ``electricsheep.local`` `if your computer supports bonjour <https://learn.adafruit.com/bonjour-zeroconf-networking-for-windows-and-linux/overview>`_ or the IP address assigned by your router), default username is "pi", default password is "raspberry", change the password using the ``passwd`` command and expand the filesystem of the SD card through the corresponding option when running ``sudo raspi-config``.

Requirements
------------
* Raspberrypi 2 and newser or device running Armbian, Older Rasperry Pis are not currently supported.  See `Raspberry Pi <https://github.com/guysoft/FullPageOS/issues/12>`_ and `Raspberry Pi <https://github.com/guysoft/FullPageOS/issues/43>`_.
* 2A power supply


Features
--------

* Runs Electric Sheep out of the box

Developing
----------

Requirements
~~~~~~~~~~~~

#. `qemu-arm-static <http://packages.debian.org/sid/qemu-user-static>`_
#. `CustomPiOS <https://github.com/guysoft/CustomPiOS>`_
#. Downloaded `Raspbian <http://www.raspbian.org/>`_ image.
#. root privileges for chroot
#. Bash
#. realpath
#. sudo (the script itself calls it, running as root without sudo won't work)

Build ElectricSheepPi From within Raspbian / Debian / Ubuntu
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

ElectricSheepPi can be built from Debian, Ubuntu, Raspbian.
Build requires about 2.5 GB of free space available.
You can build it by issuing the following commands::

    sudo apt-get install realpath qemu-user-static
    
    git clone https://github.com/guysoft/CustomPiOS.git
    git clone https://github.com/guysoft/ElectricSheepPi.git
    cd ElectricSheepPi/src/image
    wget -c --trust-server-names 'https://downloads.raspberrypi.org/raspbian_lite_latest'
    cd ..
    ../../CustomPiOS/src/update-custompios-paths
    sudo modprobe loop
    sudo bash -x ./build_dist
    
Building ElectricSheepPi Variants
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

ElectricSheepPi supports building variants, which are builds with changes from the main release build. An example and other variants are available in the folder ``src/variants/example``.

To build a variant use::

    sudo bash -x ./build_dist [Variant]
    
Building Using Vagrant
~~~~~~~~~~~~~~~~~~~~~~
There is a vagrant machine configuration to let build ElectricSheepPi in case your build environment behaves differently. Unless you do extra configuration, vagrant must run as root to have nfs folder sync working.

To use it::

    sudo apt-get install vagrant nfs-kernel-server
    sudo vagrant plugin install vagrant-nfs_guest
    sudo modprobe nfs
    cd ElectricSheepPi/src/vagrant
    sudo vagrant up

After provisioning the machine, its also possible to run a nightly build which updates from devel using::

    cd ElectricSheepPi/src/vagrant
    run_vagrant_build.sh
    
To build a variant on the machine simply run::

    cd ElectricSheepPi/src/vagrant
    run_vagrant_build.sh [Variant]

Usage
~~~~~

#. If needed, override existing config settings by creating a new file ``src/config.local``. You can override all settings found in ``src/config``. If you need to override the path to the Raspbian image to use for building OctoPi, override the path to be used in ``ZIP_IMG``. By default, the most recent file matching ``*-raspbian.zip`` found in ``src/image`` will be used.
#. Run ``src/build_dist`` as root.
#. The final image will be created in ``src/workspace``

Code contribution would be appreciated!
