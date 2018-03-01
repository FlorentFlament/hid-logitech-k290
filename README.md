HID Logitech K290 Linux Driver
==============================

This drivers allows to configure the K290 keyboard's function key
behaviour (whether function mode is activated or not by default).

Logitech custom commands are taken from [Marcus Ilgner
k290-fnkeyctl][1]

To build this module, one needs to have a built kernel tree (for
instance see [Installing a Vanilla Linux Kernel on Fedora][2])

Type `make` to build the module:
```
$ make
CPATH=/lib/modules/4.16.0-rc3-flw1+/build/drivers/hid make -C /lib/modules/4.16.0-rc3-flw1+/build M=/home/florent/src/k290-linux-driver modules
make[1]: Entering directory '/home/florent/src/linux'
  CC [M]  /home/florent/src/k290-linux-driver/hid-logitech-k290.o
  Building modules, stage 2.
  MODPOST 1 modules
  CC      /home/florent/src/k290-linux-driver/hid-logitech-k290.mod.o
  LD [M]  /home/florent/src/k290-linux-driver/hid-logitech-k290.ko
make[1]: Leaving directory '/home/florent/src/linux'
```

Then load the module with the function key desactivated:
```
$ sudo insmod hid-logitech-k290.ko fn_mode=0

$ dmesg
...
[20140.214517] input: Logitech USB Keyboard as /devices/pci0000:00/0000:00:14.0/usb1/1-4/1-4:1.0/0003:046D:C31F.0002/input/input79
[20140.266405] hid-logitech-k290 0003:046D:C31F.0002: input,hidraw1: USB HID v1.10 Keyboard [Logitech USB Keyboard] on usb-0000:00:14.0-4/input0
[20140.266997] input: Logitech USB Keyboard as /devices/pci0000:00/0000:00:14.0/usb1/1-4/1-4:1.1/0003:046D:C31F.0003/input/input80
[20140.319419] hid-logitech-k290 0003:046D:C31F.0003: input,hiddev96,hidraw2: USB HID v1.10 Device [Logitech USB Keyboard] on usb-0000:00:14.0-4/input1
```

Note that if no parameter are provided when loading the module the
function key will be activated by default.

To have the driver always loaded with the appropriate parameter one
can use the modprobe configuration mechanism:
```
$ sudo sh -c "echo 'options hid_logitech_k290 fn_mode=0' > /etc/modprobe.d/hid_logitech_k290.conf"
```

[1]: https://github.com/milgner/k290-fnkeyctl
[2]: http://www.florentflament.com/blog/installing-a-vanilla-linux-kernel-on-fedora.html
