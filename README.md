
# EdgeX Open: Training

## What is EdgeX Foundry?
The EdgeX Foundry is a Linux Foundation project with the goal of providing an open and extensible platform for IoT edge computing. As a platform, EdgeX doesn’t try to solve business problems directly, but rather to provide a solid base for such a solution to be built using off the shelf components, provided by the EdgeX Foundry project or 3rd parties, which all interoperate using the same open APIs.

EdgeX is built as a collection of microservices which communicate with each other over a set of standard, open REST APIs. This design gives it a few  distinct advantages over other approaches to edge computing. First, it allows you to run the microservices on different machines, or even on different parts of the network. For example, you can run device controlling services on the devices themselves, with the data collection and processing services on a computer with more storage and CPU capability. This design also means that individual components can be substituted with alternative implementations that fill a specific need, without having to replace or modify any of the other services. Finally, it allows you to add new services that bring in new capabilities. The most common types of services to add into EdgeX are custom Device and Application services.

## How EdgeX fits into a smart edge solutions
Every IoT project needs to address the difficulties of connecting multiple, disparate devices together and to controlling software, often over slow or unreliable connections. As such, managing that communication, providing fault tolerance, security, and near-device compute capabilities are a functions any successful IoT project must provide. This common plumbing is precisely what EdgeX Foundry provides.

By using EdgeX components in your solution, you get a mature, tested, stable foundation on which you can build your own value-add products or services. 

If you are building smart IoT devices, you can use EdgeX to connect to existing cloud offerings or datacenter software without having to build those connections into your device. 
If you are developing analytics, automation or other kinds of business intelligence software, EdgeX lets you ingest data from devices that speak any number of protocols and use many different data models, without having to build those translations and transformations yourself.
If you provide custom deployments, EdgeX gives you an ecosystem of plug-and-play components that you can use to build up complex solutions quickly and reliably.

## Running EdgeX in a development environment
`Training Tip:` Installation of EdgeX can be completed using Snaps or Docker.

You can build the EdgeX Foundry services using the open source code on Github, but more often than now you just need to get these services running so that you can connect your own services to them. To support that, the project publishes Docker images based on the latest stable release of the open source code, as well as docker-compose.yml files that will run all the necessary services together on your development machine.

1. Install Docker and Docker Compose
2. Download the latest stable [docker-compose.yml](https://github.com/edgexfoundry/developer-scripts/raw/master/releases/edinburgh/compose-files/docker-compose-edinburgh-1.0.1.yml) file
3. Run `docker-compose up -d`
4. Verify that the services are all working with `docker-compose ps`
5. Test the APIs with http://localhost:48080/api/v1/ping

Once you have the EdgeX services running, you can take our [Quick Start guide](https://docs.edgexfoundry.org/Ch-QuickStart.html), or more in-depth [API Walkthrough](https://docs.edgexfoundry.org/Ch-Walkthrough.html) to get a feel for how EdgeX services work together.

## Alternative EdgeX installation using Canonical Snaps
[Snaps](https://snapcraft.io/docs) are self-contained application packages designed to run on any system that supports them. Each snap is a compressed SquashFS package (bearing a .snap extension), containing all the assets required by an application to run independently, including binaries, libraries, icons, etc. 

Snaps are managed by snap, the command-line userspace utility of the snapd service. With the snap command, users can query the [Snap Store](https://snapcraft.io/store), install and refresh (update) snaps, remove snaps, and perform many other tasks such as managing application snapshots, setting command aliases, enabling or disabling services, etc.

The EdgeX Foundry project publishes a  snap named [edgexfoundry](https://snapcraft.io/edgexfoundry), which includes all of the EdgeX core, export, security and support services, Mongo/Redis, and all of the 3rd party runtime dependencies (Consul, Vault, Kong, …). Hackathon participants should use the version from the latest/stable channel in the Snap Store, which is the 1.0.1 release (Edinburgh). Read more about the use of Snaps for the EdgeX Open Hackathon on the [Edgex Foundry Blog](https://www.edgexfoundry.org/blog/2019/09/26/edgex-open-hackathon-snaps/).

## Extending EdgeX to hardware with Device Services
In order to connect new devices to your EdgeX instance you will need to use a Device Service. These specialized services are responsible for communicating to devices over their own protocol, sending data from them into the EdgeX core-data service, and taking commands from the EdgeX core-command service and using them to control the device.

The EdgeX Foundry provides reference device services for a number of common IoT and smart device protocols, including MQTT, Modbus and plain HTTP, and you can use those pre-written services if your device supports those protocols. But if you need something more, the EdgeX Foundry Device SDKs make it easy for you to build a new device service capable of supporting your device.

Using either the Go or C SDKs, you can bootstrap a new Device Service that will already handle all of the setup and configuration for your new service, including uploading your Device Profiles into EdgeX core-metadata, provisioning any devices you have pre-defined, and scheduling automatic readings from your devices. The SDK also provides functions for calling all the common EdgeX APIs in a language-friendly way. All you have to do is add your device-specific connection and control code.

`Training Tip:` You can do end-to-end testing of your new Device Service by connecting your local EdgeX deployment up to a cloud service or NodeRed instance, and watch the readings flow in and commands go out.

## Extending EdgeX to the cloud with Application Services
Since EdgeX Foundry itself doesn’t try to do anything other than efficiently moving data off the edge, you’re eventually going to want to send that data to some software for further processing. To do that, EdgeX provides a couple of ways to publish data to software running either on the edge, in a data center, or the cloud.

The first method is to use the pre-built Export Services, which are capable of sending data as JSON or XML over simple HTTP or MQTT to another service. This is a good option if you are using a service that already knows how to handle these common protocols, and can do any data transformations you need.

But if those aren’t suitable, the EdgeX Foundry also provides an Application Functions SDK for writing custom handlers, which can do anything from publishing data to a remote host, to performing local transformations, analytics and responses right from the edge. The SDK allows you to trigger you application functions whenever data comes into EdgeX, or on-demand, and even provides functions for publishing your data (via HTTP or MQTT) when you’re done handling it locally. Application Functions can even be chained together to build complex pipelines, reusing the same Application Functions in different ways.

`Training Tip:` You can do end-to-end testing of your new Application Service by connecting the [Virtual Device Service](https://docs.edgexfoundry.org/Ch-ExamplesVirtualDeviceService.html) to your local EdgeX deployment, which can simulate device data coming in.