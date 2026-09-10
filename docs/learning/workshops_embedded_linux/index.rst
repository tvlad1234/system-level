Embedded Linux
==============

Introduction
------------

This workshop is designed to explore the world of embedded Linux, where you'll
learn to develop applications that run on Linux-based embedded systems.
We'll start by understanding what Linux is and why it's widely used in embedded
systems. Through interactive demonstrations and hands-on activities, you'll gain
practical experience working with Linux device drivers, the IIO subsystem, and
GPIO interfaces on real hardware. Let's dive in and start our journey into
embedded Linux applications!

Slide Deck and Booklet
----------------------

Since this tutorial is also designed to be presented as a live, hands-on
workshop, a slide deck is provided here:

.. admonition:: Download

   :download:`Embedded Linux Slide Deck <embedded_linux.pdf>`

A complete booklet of the hands-on activity is also provided, as a companion to
following the tutorial yourself:

.. admonition:: Download

   :download:`Embedded Linux Booklet <embedded_linux_booklet.pdf>`

Theory
------

.. figure:: images/tux.png
   :align: right
   :width: 200

What is Linux?
~~~~~~~~~~~~~~

Linux is an open-source operating system found everywhere. It was created by
Linus Torvalds and is symbolized by the penguin mascot "Tux".

Why use Linux:

- Free
- Open source
- No safety issues (compared to proprietary alternatives)

Linux components include:

- Bootloader
- Kernel
- Init system
- Daemons
- Graphical server
- Desktop environment
- Applications

Linux vs Windows
~~~~~~~~~~~~~~~~

.. list-table::
   :header-rows: 1
   :widths: 50 50

   * - Linux
     - Windows
   * - Security
     - Prone to malware
   * - Open source
     - Closed source
   * - Community developed
     - Developed by a company
   * - Free
     - License based
   * - Lightweight
     - Resource intensive

Why Linux on embedded systems
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

- Linux can be used in reduced size systems
- Lightweight storage requirements
- Robust
- Stable
- Large developers' community
- Support for a wide range of software architectures
- Ability to run on fewer resources
- Vendor independence

.. figure:: images/linux_spaces.png
   :align: right
   :width: 400

Linux Structure
~~~~~~~~~~~~~~~

Linux is organized into two main spaces:

*User space* - the area of memory that non-kernel applications run in, executed
by a user in the operating system, rather than being part of the operating
system itself. It contains C libraries and user applications. User space
processes run in the user space part of the memory. The user mode is the
non-privileged execution mode that the instructions are executed with.

*Kernel space* - the area of system memory reserved for the kernel. It is
distro agnostic (various distros are built on the same kernel). The kernel is
loaded when the OS is loaded and remains in memory until the OS is shut down.
It contains the System Call Interface, the Linux Kernel, and the device
drivers. Linux drivers need to be included in the kernel so the hardware
becomes accessible in user space.

Linux Distributions
~~~~~~~~~~~~~~~~~~~

While the kernel space remains consistent across Linux systems, the user space
varies significantly between distributions. Linux distributions are built on
top of the Linux kernel, providing different user space environments with
varying package managers, init systems, and default applications.

Common distribution families include:

- **Debian based**: Ubuntu, Raspberry Pi OS, Kuiper
- **Arch based**: Manjaro
- **RPM based**: Fedora, CentOS

Regardless of the distribution, they all share the same hierarchical filesystem
structure. The root filesystem is at the top of the file tree (also known as
'/'). It has a number of directories containing files critical for booting the
system and system operations, including: ``/etc``, ``/bin``, ``/usr``,
``/tmp``, ``/dev``.

----

The following sections describe how hardware is made accessible to the Linux
kernel. The device tree provides a hardware description, while device drivers
implement the software interface to interact with that hardware.

Device Tree
~~~~~~~~~~~

The device tree describes the hardware configuration that the kernel needs to
understand.

Unlike PCI or USB devices that can self-identify, embedded peripherals such as
GPIOs, I2C sensors, and SPI devices cannot be automatically discovered by the
operating system. The Devicetree (DT) provides this explicit hardware
description.

Structurally, the DT is a hierarchical tree with the root node (``/``) at the
top. Each node represents a device or logical grouping and contains:

- **Properties**: Key-value pairs describing the node (e.g., ``compatible``,
  ``reg``, ``interrupts``)
- **Child nodes**: Representing sub-devices or components

Linux uses device tree data for platform identification, runtime configuration,
and device population. The kernel matches nodes to drivers using the
``compatible`` property—this is how the system knows which driver to load for
each piece of hardware.

Linux Device Drivers
~~~~~~~~~~~~~~~~~~~~

A Linux device driver is code that knows how to handle a particular device. It
provides the right interfaces for the user-space to access that device and
abstracts various hardware details from the user-space applications.

Key characteristics:

- Implemented in C, in a ``.c`` and ``.h`` pair
- Stored under ``/drivers/"type"/`` in the kernel source tree
- Compiled with the kernel

----

With hardware described and drivers loaded, user space applications can
interact with hardware through well-defined interfaces. The following sections
cover GPIO for direct pin control and IIO for sensor and converter devices.

GPIO Interface
~~~~~~~~~~~~~~

GPIO stands for **General-Purpose Input/Output** and refers to a set of pins on
a device (IC). These pins can send or receive electrical signals, but they
aren't designed for any specific purpose. This is why they're called
"general-purpose" IO.

GPIO is one of the most commonly used peripherals in an embedded Linux system.
The GPIO access is done using the GPIOLIB framework. Sysfs is a pseudo
filesystem provided by the Linux kernel that makes information about various
kernel subsystems, hardware devices, and device drivers available in user space
through virtual files. GPIO devices appear as part of sysfs.

The basic steps to use a GPIO pin from the sysfs interface:

1. Export the pin
2. Set the pin direction (input or output)
3. If an output pin, set the level to low or high
4. If an input pin, read the pin's level (low or high)
5. When done, unexport the pin

IIO Concepts
~~~~~~~~~~~~

The Linux kernel Industrial I/O (IIO) subsystem is intended to provide support
for devices that, in some sense, are analog-to-digital or digital-to-analog
converters.

Devices that fall into this category are:

- ADCs
- DACs
- Accelerometers, gyros, IMUs
- Capacitance-to-Digital converters (CDCs)
- Pressure, temperature, and light sensors
- RF Transceivers (like the AD9361 / AD9364 / AD9371 / ADRV9009)

The IIO framework can be used on ADCs ranging from a 1 MSPS SoC ADC to >5 GSPS
ADCs. It has been in the upstream Linux kernel for more than 10 years.

The IIO subsystem provides a cross-platform libiio library with bindings for
many programming languages including C, Python, and C++.

ADXL355 Part
~~~~~~~~~~~~

The ADXL355 is a Low Noise, Low Drift, Low Power, 3-Axis MEMS Accelerometer.

Features:

- Digital SPI and I2C interfaces supported
- 20-bit ADC
- Data interpolation routine for synchronous sampling
- Programmable high- and low-pass digital filters
- 0 g offset vs. temperature (all axes): 0.15 mg/°C maximum
- V\ :sub:`SUPPLY` with internal regulators: 2.25 V to 3.6 V

Hands-on activity
-----------------

By the end of this workshop, you will learn:

* How to interact with the Linux filesystem
* How to use a Linux demo running on specific hardware
* How to modify written code to perform certain tasks
* How to control LEDs via sysfs
* How to build and load kernel modules
* How to read sensor data through the IIO subsystem

Materials
~~~~~~~~~

* Cora Z7 FPGA Platform (CoraZ7S)
* EVAL-ADXL355-PMDZ accelerometer module
* Raspberry Pi 5 as development workstation
* USB cable for CoraZ7S UART connection
* Ethernet cable
* 2 SD cards

Pre-requisites
~~~~~~~~~~~~~~

**Common Linux commands:**

- ``cd``: used to change the current working directory in Linux
- ``ls``: lists files and directories within the file system
- ``cat``: displays the contents of one or multiple text files
- ``echo``: prints out its arguments as standard output; use ``>`` or ``>>``
  with the echo command to print the output to a file instead of displaying it
  in the terminal
- ``mv``: moves a file from one location to another, rename a file with or
  without moving it

Workshop Preparation
++++++++++++++++++++

This workshop requires two SD cards with pre-configured Kuiper images:

- One for the **Raspberry Pi 5** (development workstation)
- One for the **CoraZ7S** (target board)

Preparing the Raspberry Pi 5 SD Card
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Download the `Raspberry Pi 5 workshop image
   <https://swdownloads.analog.com/cse/kuiper/kuiperv2.0.0/university-workshops/image_pi5.zip>`__.

2. Unzip the downloaded file to extract the image.

3. Write the image to an SD card by following the
   :external+kuiper:ref:`Writing the Image to an SD Card
   <use-kuiper-image>` guide.

Preparing the CoraZ7S SD Card
^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

1. Download the `CoraZ7S workshop image
   <https://swdownloads.analog.com/cse/kuiper/kuiperv2.0.0/university-workshops/image_cora.zip>`__.

2. Unzip the downloaded file to extract the image.

3. Write the image to an SD card by following the
   :external+kuiper:ref:`Writing the Image to an SD Card
   <use-kuiper-image>` guide.

Once both SD cards are prepared, proceed to the hardware setup below.

Hardware Setup
++++++++++++++

**Raspberry Pi 5 (Workstation)**

Insert the Raspberry Pi 5 SD card and power on the board. Make sure it is
connected to the monitor, mouse, and keyboard. The monitor should show the
ADI-KUIPER-LINUX desktop.

**CoraZ7S (Target Board)**

1. Insert the CoraZ7S SD card in the SD port of CoraZ7S.

.. figure:: images/cora_sd_card.png
   :align: center
   :width: 400

2. Connect the ADXL355 to the JA port of the CoraZ7S board.

.. figure:: images/cora_adxl.png
   :align: center
   :width: 400

3. Connect the UART port of the CoraZ7S to a USB port of your Raspberry Pi 5
   using the USB cable.

.. figure:: images/cora_usb.png
   :align: center
   :width: 400

4. Connect the Ethernet port of the CoraZ7S to the Ethernet port of your
   Raspberry Pi 5.

.. figure:: images/cora_ethernet.png
   :align: center
   :width: 400

CoraZ7S Environment Exploration
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. On your Raspberry Pi 5, search for *Terminal* and open it.

2. Write the following command to discover the available IIO devices:

.. shell::
   :show-user:
   :user: analog
   :group: pi-workstation

   $iio_info -s

Look for the IP address corresponding to *workshop-cora* machine. You will need
it for the next steps. To find it easier you can search by the hostname of the
carrier:

.. shell::
   :show-user:
   :user: analog
   :group: pi-workstation

   $iio_info -s | grep workshop-cora

The output should look like this:

.. code-block:: text

   0: 169.254.35.68 (xadc) [ip:workshop-cora.local]

3. Write the following command to connect via SSH to the CoraZ7S (replace the
   IP address in the command with the address of your board):

.. shell::
   :show-user:
   :user: analog
   :group: pi-workstation
   
   $ssh -X root@169.254.35.68

The password is ``analog``.

At this point you should see this in your terminal:

.. code-block:: text

   analog@pi-workstation:~$ ssh root@169.254.35.68
   root@169.254.35.68's password:
   Linux workshop-cora 6.12.77-g4b2e305bc496 #1 SMP PREEMPT Wed Jul 15 15:52:08 UTC 2026 armv7l

   The programs included with the Debian GNU/Linux system are free software;
   the exact distribution terms for each program are described in the
   individual files in /usr/share/doc/*/copyright.

   Debian GNU/Linux comes with ABSOLUTELY NO WARRANTY, to the extent
   permitted by applicable law.
   Last login: Thu Sep 10 07:25:12 2026 from  169.254.35.68
   root@workshop-cora:~#

You are now connected to the CoraZ7S. Every command you run from this point
forward will be executed on the CoraZ7S, not your workstation.

4. Provision your working copy of the workshop exercises:

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $datax-workshops-embedded-linux-setup

You should see output similar to this:

.. code-block:: text

   '/usr/share/datax-workshops-embedded-linux/exercises/./game.py' -> '/root/datax-workshops/embedded-linux/./game.py'
   '/usr/share/datax-workshops-embedded-linux/exercises/./leds.sh' -> '/root/datax-workshops/embedded-linux/./leds.sh'

   Done. Your workshop files are in: /root/datax-workshops/embedded-linux
   To change your current working path run:
       cd /root/datax-workshops/embedded-linux

   See the workshop guide:
   https://analogdevicesinc.github.io/documentation/learning/workshops_embedded_linux/index.html

5. Move into the workspace folder created above:

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $cd /root/datax-workshops/embedded-linux

Example 1: Turn on LEDs and set colors manually through PWM
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

1. Move to the PWM chip directory using the command below:

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $cd /sys/class/pwm/pwmchip0

2. List the contents of the directory:

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $ls

You will see ``export`` and ``unexport`` files (used to make a PWM channel
available or release it) and ``npwm``, which reports how many channels the
chip provides. Each RGB LED on the board is driven by three of these PWM
channels, one per color:

.. list-table::
   :header-rows: 1
   :widths: 20 80

   * - Channel
     - LED / color
   * - ``pwm0``
     - LED0 red
   * - ``pwm1``
     - LED0 green
   * - ``pwm2``
     - LED0 blue
   * - ``pwm3``
     - LED1 red
   * - ``pwm4``
     - LED1 green
   * - ``pwm5``
     - LED1 blue

3. Export channel 0 (LED0 red) so it becomes accessible, then move into its
   folder:

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $echo 0 >> export
   $cd pwm0

4. List the contents of the folder:

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $ls

This folder contains the following attributes:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Attribute
     - Description
   * - ``period``
     - The total period of the PWM signal, in nanoseconds
   * - ``duty_cycle``
     - The active (high) time within each period, in nanoseconds; must be
       less than or equal to ``period``
   * - ``enable``
     - Starts (1) or stops (0) the PWM signal output
   * - ``polarity``
     - Configures the signal as ``normal`` or ``inversed``

The LED is fully on when ``duty_cycle`` equals ``period`` (signal high for the
whole period) and fully off when ``duty_cycle`` is 0. A value in between would
dim the LED, but for this exercise we will only use fully on or fully off to
combine colors.

5. A channel keeps whatever ``duty_cycle`` it powered up with until you set
   one yourself, so before doing anything else, export every remaining
   channel and bring all six to a known, fully-off state: set their period,
   set ``duty_cycle`` to 0, and only then enable them. If you enable a channel
   before its ``duty_cycle`` is set to 0, its LED can stay lit:

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $cd /sys/class/pwm/pwmchip0
   $echo 1 >> export
   $echo 2 >> export
   $echo 3 >> export
   $echo 4 >> export
   $echo 5 >> export
   $echo 1000000 >> pwm0/period
   $echo 1000000 >> pwm1/period
   $echo 1000000 >> pwm2/period
   $echo 1000000 >> pwm3/period
   $echo 1000000 >> pwm4/period
   $echo 1000000 >> pwm5/period
   $echo 0 >> pwm0/duty_cycle
   $echo 0 >> pwm1/duty_cycle
   $echo 0 >> pwm2/duty_cycle
   $echo 0 >> pwm3/duty_cycle
   $echo 0 >> pwm4/duty_cycle
   $echo 0 >> pwm5/duty_cycle
   $echo 1 >> pwm0/enable
   $echo 1 >> pwm1/enable
   $echo 1 >> pwm2/enable
   $echo 1 >> pwm3/enable
   $echo 1 >> pwm4/enable
   $echo 1 >> pwm5/enable

All six LEDs should now be off. From this point, turning a color on or off is
just a matter of setting that channel's ``duty_cycle`` back and forth between
0 and the period. For example, turn LED0 red on and back off:

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $echo 1000000 >> pwm0/duty_cycle
   $echo 0 >> pwm0/duty_cycle

6. A given LED's color is set by driving its red, green, and blue channels
   fully on or off together:

.. list-table::
   :header-rows: 1
   :widths: 30 70

   * - Color
     - Channels driven fully on
   * - Red
     - red
   * - Green
     - green
   * - Blue
     - blue
   * - Yellow
     - red + green
   * - Cyan
     - green + blue
   * - Purple
     - red + blue
   * - White
     - red + green + blue

**Challenge**

Make the first LED light continuously purple and the second LED light
continuously cyan.

**Hint**

For the first LED, drive its red and blue channels fully on. For the second
LED, drive its green and blue channels fully on.

**Challenge Solution**

With all six channels already exported, configured, and enabled from step 5,
turning on a color is just a matter of raising the right channels' duty
cycles:

For led0 (purple = red + blue, channels ``pwm0`` and ``pwm2``):

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $echo 1000000 >> pwm0/duty_cycle
   $echo 1000000 >> pwm2/duty_cycle

For led1 (cyan = green + blue, channels ``pwm4`` and ``pwm5``):

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $echo 1000000 >> pwm4/duty_cycle
   $echo 1000000 >> pwm5/duty_cycle

Example 2: Run a script
~~~~~~~~~~~~~~~~~~~~~~~

This example demonstrates how to execute a series of commands within a
script, rather than running each command individually in the terminal, to play
a color crossfade animation across both RGB LEDs through the same PWM
interface. You will also learn how any Linux script can be turned into a bash
command.

1. Run the script:

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $bash leds.sh

The animation takes about 20 seconds to complete on its own. If you want to
stop it early, press :kbd:`Ctrl+Z` to suspend it.

2. Now make the script available to be run from anywhere in the file system and
   run it again:

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $mv leds.sh /usr/local/bin
   $leds.sh

Example 3: Compute the temperature value from sysfs
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The task is allowing you to read the content of attribute files and compute the
temperature given by the on-board temperature sensor.

The temperature is computed using the following formula:

.. math::

   TEMPERATURE = (RAW + OFFSET) \cdot SCALE

**Steps**

1. Move to the iio:device folder:

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $cd /sys/bus/iio/devices/iio:device1

2. Here you can see all the attributes of the adxl355:

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $ls

3. Put temperature values in variables:

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $SCALE=$(cat in_temp_scale)
   $OFFSET=$(cat in_temp_offset)
   $RAW=$(cat in_temp_raw)

4. Compute the temperature and display it in the terminal (keep in mind that it
   will be in millidegrees Celsius):

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $echo "($RAW + $OFFSET) * $SCALE" | bc

Example 4: Play a Snake game with the accelerometer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Using the accelerometer module, you will have the chance to play a little oldie
but goldie SNAKE game.

1. Run the game controls in the background:

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $python3 /root/datax-workshops/embedded-linux/game.py &

2. Start the Snake game:

.. shell::
   :show-user:
   :user: root
   :group: workshop-cora

   $/usr/games/nsnake

Tilt the accelerometer board around and observe the output. Be careful not to
disconnect the USB and Ethernet cables

Have fun!

Takeaways
---------

* **Linux provides a robust embedded platform**: The combination of open-source
  licensing, lightweight resource requirements, and extensive hardware support
  makes Linux ideal for embedded systems.

* **Sysfs enables hardware interaction**: The pseudo filesystem allows
  user-space applications to interact with kernel subsystems, hardware devices,
  and device drivers through virtual files.

* **IIO simplifies sensor integration**: The Industrial I/O framework provides
  a standardized way to work with ADCs, DACs, accelerometers, and other sensors
  in embedded Linux applications.

* **Kernel modules extend functionality**: Building and loading kernel objects
  allows dynamic addition of device driver support without recompiling the
  entire kernel.

* **Scripting automates tasks**: Shell scripts enable automation of repetitive
  tasks and can be made available system-wide as commands.

Resources
---------

**Linux**

:git-linux:`main:/`

`Linux Kernel Documentation <https://docs.kernel.org>`_

**Hardware**

:adi:`EVAL-ADXL355-PMDZ Product Page <EVAL-ADXL355-PMDZ>`

`Digilent Cora Z7 Reference Manual <https://digilent.com/reference/programmable-logic/cora-z7/reference-manual>`_
