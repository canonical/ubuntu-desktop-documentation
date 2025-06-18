# Architecture of the accessibility stack

## Software Interfaces for assistive technologies

### AT-SPI

The Assistive Technology Service Provider Interface (AT-SPI) is a platform-neutral framework for bi-directional communication between assistive technologies (AT) and applications. 

AT-SPI defines a set of interfaces for assistive technologies which allow to get names, roles and states of interface objects, listen for events, or perform actions. 

AT-SPI uses D-Bus method calls and signals. D-Bus is an inter-process communication system in Linux, which makes AT-SPI a default accessibility framework for Linux.  

AT-SPI is made of two components:

`at-spi2-core`
: A collection of XML interfaces for accessibility

`at-spi2-atk`
: A library that allows applications made with GTK 3 to register accessible objects with AT-SPI

If you are building custom assistive technology tools or want to test the accessibility of custom applications that were not built with GTK on Ubuntu Desktop, you must use {ref}`at-spi-dbus-xml-interfaces`.

### GTK4 and Gtk.Accessible

GTK is a cross-platform widget toolkit for creating graphical user interfaces. It offers a set of user interface elements that allow developers to build apps quickly and in a consistent manner. The majority of applications within GNOME desktop environment are built with GTK.       

GTK4 is a newer version of GTK that implements its own [Gtk.Accessible](https://docs.gtk.org/gtk4/iface.Accessible.html) interface instead of the previously used ATK.

If you are building an application for Ubuntu Desktop and want to make it accessible for Ubuntu Desktop's default assistive technologies such as Orca, it is recommended that you use GTK. Desktop's assistive solutions are able to access information about the apps built with both GTK3 and GTK4.

If you are using a different UI toolkit, you must ensure it implements the AT-SPI D-Bus interfaces; otherwise, Desktop's assistive technologies wouldn't be able to interact with it. 

### GTK3 and ATK 

GTK3 version of the GTK toolkit that integrates with Accessibility Toolkit (ATK). Applications built with GTK3 expose information about UI objects using the ATK. ATK is an Application Programming Interface (API) that allows developers to avoid working with AT-SPI API directly, as it provides a set of special input methods. 

### Flutter

A selection of applications on Ubuntu Desktop such as [App Center](https://github.com/ubuntu/app-center) are built with Flutter. Flutter engine has compatibility issues with AT-SPI. To learn how to make Fluuter applications that would be compatible with Ubuntu Desktop's default assistive solutions, reach out to the development team at [Discourse](https://discourse.ubuntu.com/tag/flutter).


### Overview of Ubuntu Desktop applications and their interfaces

| Application              | Built with    | API                                                                |
| ------------------------ | ------------- | ------------------------------------------------------------------ |
| Gnome Initial Setup      | GTK4          | [Gtk.Accessible](https://docs.gtk.org/gtk4/iface.Accessible.html)  |        
| Login and Lock screens   | GTK3          | [ATK](https://docs.gtk.org/atk/) |
| System Settings app      | GTK4          | [Gtk.Accessible](https://docs.gtk.org/gtk4/iface.Accessible.html) |
| App Center               | Flutter       | [Flutter API](https://docs.flutter.dev/ui/accessibility-and-internationalization/accessibility) |
| Software and Updates     | GTK3          | [ATK](https://docs.gtk.org/atk/) |



## Hardware interfaces for assistive technologies

### USB 

Ubuntu Desktop supports USB out of the box. 

In Ubuntu, kernel USB drivers handle communications with USB devices such as, for example, `usbhid` which provides support for USB Human Interface Devices (HID) class. Kernel supports all [standard USB classes](https://www.usb.org/defined-class-codes).

`udev` is a device manager in Ubuntu that detects, adds and removed devices. `udev` stores the information about detected USB devices in the `/dev` directory.

If your device conforms to a USB class, it will be assigned a /dev entry and recognized correctly.

#### USB device requirements

You can use USB to connect assistive devices for input/output such as Braille displays, sip-and-puff, foot pedals.

To be able to work on Ubuntu properly, a USB device must:

* use a standard USB class 
* provide a proper USB descriptor so that Ubuntu can identify it
* not require proprietary drivers

If the device uses a custom class, it must provide a `udev` .rules file instead so that Ubuntu can identify it correctly, see [udev documentation](https://www.man7.org/linux/man-pages/man7/udev.7.html) 

### Bluetooth 

While the Ubuntu Desktop team continuously tries to improve the experience with Bluetooth, the Bluetooth stack is complex and consists of many dependencies on the kernel, firmwave, middleware, and userspace levels. This complex set of dependencies makes it difficult to guarantee consistent compatibility with Bluetooth devices.

[BlueZ](https://www.bluez.org/) is the official Bluetooth stack for Linux distributions, including Ubuntu Desktop.

BlueZ implements various Bluetooth profiles such as HID, A2DP, HFP, SPP, and others. See [BlueZ supported profiles](https://www.bluez.org/profiles/). If a device is compliant with one of the supported profiles, BlueZ automatically detects, pairs, and exposes it as an input device on Linux. 

#### Bluetooth device requirements

You can use Bluetooth to connect wireless assistive devices such as switch control devices or head trackers.

To be able to work on Ubuntu properly, a Bluetooth device must:

* use HID for input devices 
* use A2DP/HFP for audio output

### Troubleshooting Bluetooth issues

If you experience issues with your Bluetooth devices, first consult the [Hardware problems](https://help.ubuntu.com/stable/ubuntu-help/bluetooth.html.en#problems) section or create a [thread on the Discourse](https://discourse.ubuntu.com/c/support-and-help/306) with a detailed description of your problem.

#### Check for proprietary firmware

Many Bluetooth adapters require proprietary firmware that may not be included in Desktop by default for licensing reasons. 
To resolve this, you need to install download and install drivers for your device manually. Consult your device manufacturer's documentation to find where the drivers can be downloaded from.

Check the logs to see if the issue is caused by missing firmware:

    dmesg | grep -i 'firmware load'

The log entry might look similar to this example:

    bluetooth hci0: Direct firmware load for <path>/<driver-name> failed with error -2

This error indicates what driver the kernel attempts to load and where it expect to find it. Once you have the name, you can download it from manufcacturer and place in `/lib/firmware/<path>` . For example, Broadcom drivers are expected to be placed in under `/lib/firmware/brcm`.