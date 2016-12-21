## About Docker

[Docker](https://hub.docker.com/) provides a way to run applications securely isolated in a container, packaged with all its dependencies and libraries. Because your application can always be run with the environment it expects right in the build image, testing and deployment is simpler than ever, as your build will be fully portable and ready to run as designed in any environment. And because containers are lightweight and run without the extra load of a hypervisor, you can run many applications that all rely on different libraries and environments on a single kernel, each one never interfering with the other. This allows you to get more out of your hardware by shifting the “unit of scale” for your application from a virtual or physical machine, to a container instance.



## Components
- Docker/config: Script that contains DOCKER_ENGINE_URL,registryServerAddres, registryUsername, registryPassword, and registryEmail.
- Docker/httpclient : generic http client that handles the communication between scriptr.io and Docker's APIs.
- Docker/DockerManager : this is the main object to interact with. It has methods that create instances of other objects to use, along with wideInformation, version information, and ping system methods.
- Docker/ImageManager: This object can be used to interact with Docker images (Docker images are the basis of containers. Each time you’ve used docker run you told it which image you wanted. This object can list, search, create, remove, and inspect images from docker.
- Docker/ContainerManager: This object can be used to interact with Docker containers (A container is a running instance of an image). This object can list, create, inspect, get stats, start, stop,	restart, update, and remove a container from docker.
- Docker/VolumeManager: This object can be used to interact with Docker volumes (Volumes are directories that are stored outside of the container’s filesystem and which hold reusable and shareable data that can persist even when containers are terminated. This data can be reused by the same service on redeployment, or shared with other services). This object can list, create, remove, and inspect a volume from docker.

## How to use
- Use the Import Modules feature to deploy the aforementioned scripts in your scriptr account, in a folder named "modules/Docker".
- Make sure to have a docker engine running which is exposed through HTTP protocol, so that you can interact with using an HTTP url
- Once done, make sure to specify the DOCKER_ENGINE_URL,registryServerAddres, registryUsername, registryPassword, and registryEmail in "config" file.
- Create a test script in scriptr, or use the script provided in modules/Docker/test/dockerEngine/test. 


### Use the connector

In order to use the connector, you need to import the main module: ```modules/Docker/DockerManager``` as described below:
```
var dockerModule = require("modules/Docker/DockerManager");
```
Then create a new instance of the DockerManager class, defined in this module(Note: this instance can take a customized config file):
```
var myManager = new dockerModule.DockerManager(config);
```
The DockerManager class provides many methods, such as:
```
getImageManager
getContainerManager
getVolumeManager
getSystemWideInformation
getSystemVersion
pingDocker

```
Once you create new instance of the DockerManager class, you can create other instances of other objects in this connector by simply calling ```getContainerManager()``` to create an instance of ContainerManager, or by calling ```getImageManager()``` to create an instance of ImageManager, or by calling ```getVolumeManager()``` to create an instance of VolumeManager.
```
var config = require("Modules/Docker/test/dockerEngine/testConfig");
var dockerManager = require("Modules/Docker/DockerManager");
//create instance of DockerManager.
var myDockerManager = new dockerManager.DockerManager(config);
//create instance of image, container, and volume managers from the instance of DockerManager.
var cm = myDockerManager.getContainerManager();
var IM = myDockerManager.getImageManager();
var VM = myDockerManager.getVolumeManager();

var createVolumeParams ={
  "Size":500,
  "VolumeType":"gp2"
}
vm.createVolume(createVolumeParams,[{"Key":"Name","Value":"batata"}],{"text":"test"});

var attachVolumeParams={
  "VolumeId":"vol-07ba3c44cc9daceee",
  "InstanceId":"i-0e0096b47bbb17b38",
  "Device":"/dev/xvdh"
}
vm.attachVolume(attachVolumeParams);
vm.deleteVolume({"VolumeId":"vol-0ee3787787bd61b98"},{"text":"test"});
```
