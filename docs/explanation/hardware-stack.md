(hardware-stack)=
# Hardware stack

This section provides detailed explanations about various components that comprise Ubuntu Desktop hardware stack, a role of each component and how they work together. Where applicable, links to documentation on common issues with specific components are included.

## Bluetooth

### Kernel modules

`bluetooth` is a kernel module that manages core functionality of the Bluetooth stack and implements core Bluetooth protocols. It does not directly interact with hardware but acts as an abstraction layer for hardware drivers.

`btusb` is a kernel module that acts as a generic hardware driver. It provides support for Bluetooth devices connected via USB. It depends on `bluetooth` for its core functionality. `btusb` is the module that detects the device and loads its firmware.

### Firmware

A firmware blob (commonly referred to simply as firmware) is a binary file that defines the logic of specific operations for the device, for example, initialization sequence, power management, support for Bluetooth profiles or their specific versions.

Firmware is created and developed by manufacturers and, to provide support for Linux distributions, must be made included in the `linux-firmware` package. 

Once a Bluetooth device is connected to the machine, the hardware driver such as `btusb` reads the identifying data from the device and requests a matching firmware blob. If there is a matching firmware blob, the driver then uploads the firmware into the device's RAM and the device initializes and becomes discoverable to the host. 

#### Common issues

* {ref}`proprietary-firmware` 

### HCI (Host Controller Interface)

HCI (Host Controller Interface) is a Bluetooth specification that defines how a host can communicate with a device. HCI defines commands which the host can send to the device such as *Connect* or *Reset* and events that the device can send back. For example, *Connection established* in response to the *Connect* command. 

Once the device is initalized, it is ready to accept the HCI commands. The `bluetooth` module sends a basic HCI command to the device to verify that the device can be communicated with. If it is successful, `bluetooth` then registers the device as an HCI controller, assigns it the name `hciX` and adds a special file with this name to `/sys/class/bluetooth/`.   

### Bluetooth profiles

For most devices, being able to receive and send HCI commands and events is not enough to ensure appropriate fucntionality. A keyboard and a hands-free device for a car must perform drastically different operations and exchange different data with the host. HCI defines only the core communications that are universal for all Bluetooth devices.

Bluetooth [profiles](https://www.bluez.org/profiles/) are specifications that define *specific functionality* of the device. They define the device's behavior, data formats they can use, how authentication or reconnection should work, and many other specific behaviors.

Bluetooth profiles serve different purposes, for example HID (Human Interface Device) is a profile standard used for  keyboards, mice, and game controllers. A2DP (Advanced Audio Distribution Profile) is used for headphones and speakers. 

For a full list of standard profiles, see [Bluetooth specifications](https://www.bluetooth.com/specifications/specs/).

### Middleware 

[BlueZ](https://www.bluez.org/) is a set of services, libraries, plugins, and utilities. It manages the communications with the device by translating high-level application requests into HCI commands sent through the kernel. BlueZ exposes a D-Bus API to applications, which allows applications such as *Settings* or CLI tools such as `bluetoothctl` to perform various operations with the device.

BlueZ also handles device profiles: it identifies profiles associated with the devices, loads appropriate plugins that provide support for the profiles, and register devices in the `/dev/` directory. `/dev/` is a collection of files that represent the devices.  See [BlueZ supported profiles](https://www.bluez.org/profiles/) to learn which Bluetooth profiles BlueZ supports.

Once a device has been successfully identified by BlueZ, registered and a correspnding plugin has been loaded, it is fully functional.
