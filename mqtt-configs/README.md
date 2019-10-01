# MQTT Device Service Setup

This folder contains the MQTT `device profiles` for the simulated and real `sensor`data that will published to the common `MQTT Broker`.

* device-pos (POS events)
  * basketopen (simulated)
  * positem (simulated)
  * paymentstart (simulated)
  * paymentsuccess (simulated)
  * basketclose (simulated)
* scale-device (Weight scale events)
  * scaleitem (simulated)
* rfid-device (RFID events)
  * heartbeat (not simulated)
  * alert (not simulated)
  * events (simulated)
* roi-device (CV Region of Interest events)
  * roi-enter (real data)
  * roi-exit (real data)
  * roi-count (real data)

###  Configuring MQTT Device Service

See https://www.edgexfoundry.org/blog/2019/09/26/edgex-open-hackathon-snaps/ for details on installing `edgex-device-mqtt`

Once `edgex-device-mqtt` is installed copy the configuration and device profiles from this folder to `/var/snap/edgex-device-mqtt/current/config/device-mqtt/res`

Then start the `edgex-device-mqtt`

`sudo snap start --enable edgex-device-mqtt`

`edgex-device-mqtt`pushes its configuration into the `Registry`, aka `Consul` the first time it is run. On subsequent runs it pulls the configuration from the `Registry`. Use `Consul UI` @`localhost:8500` from your browser to make simple configuration changes, like changing `Writeable/LogLevel`. 

### Add a new MQTT device profile

* Copy your new `device profile` to `/var/snap/edgex-device-mqtt/current/config/device-mqtt/res`
* Add the `Device` to the `DeviceList` section in the `configuration.toml` located in `/var/snap/edgex-device-mqtt/current/config/device-mqtt/res`
* Remove the configuration from `Consul` by deleting `edgex/devices/1.0/edgex-device-mqtt` via 
* run `sudo snap restart edgex-device-mqtt`
  * New device profile will be added and updated `configuration.toml` will be push into `Consul UI`  @`localhost:8500`

### Modify a MQTT Device Profile

If you need to modify a `device profile` to add a new `device resource`, change a name, etc.

* Make the change to the `device profile` in `/var/snap/edgex-device-mqtt/current/config/device-mqtt/res`

* Run the following curl commands to clear out the old `device profile`

  ```
  curl -X DELETE http://localhost:48081/api/v1/deviceservice/name/edgex-device-mqtt
  curl -X DELETE http://localhost:48081/api/v1/deviceprofile/name/device-dummy
  ```

* Restart the `Device MQTT`

  `sudo snap restart edgex-device-mqtt`

  This will push your `device profile` changes into the DB.