## Developer Setup and Resources

### Installing EdgeX

See https://www.edgexfoundry.org/blog/2019/09/26/edgex-open-hackathon-snaps/ for details on install `edgexfoundry` via snaps.

### App Functions SDK 

See https://github.com/edgexfoundry/app-functions-sdk-go/blob/master/README.md for details on writing custom Application Service using the app-functions-sdk-go.

Examples:
https://github.com/edgexfoundry/app-functions-sdk-go/tree/master/examples

### Application Service Configurable

See https://github.com/edgexfoundry/app-service-configurable/blob/master/README.md for details on usage.

##### Installing and running multiple instance 

See `Extending EdgeX` section of https://www.edgexfoundry.org/blog/2019/09/26/edgex-open-hackathon-snaps/ for details.

### Device Service SDK

See https://docs.edgexfoundry.org/Ch-GettingStartedSDK-Go.html for details on creating custom device service using the device-sdk-go

Examples:
https://github.com/edgexfoundry/device-random
https://github.com/edgexfoundry/device-virtual-go

### REST Device Service

This is a new device service that simplifies pushing data to EdgeX via HTTP POSTs.  If interested in using this you will have to clone the repo, checkout the `hack`branch and the build and run it locally.

https://github.com/lenny-intel/device-rest-go

See https://github.com/lenny-intel/device-rest-go/blob/initial/README.md for details on usage.

### RFID Card Readers

Two RFID Card readers and card sets are available for teams to use. This will required creating a USB Device Service to communicate with the card reader, which are HID devices. The USB VID/PID for these readers is:

* VID = 0xffff 
* PID = 0x35
* VendorName = "Sycreader USB Reader"

The [github.com/gvalkov/golang-evdev](https://github.com/gvalkov/golang-evdev) go package can be used to read the data from these card readers.

> *Note: Each character from the card is read one at a time, rather than as one complete string.*