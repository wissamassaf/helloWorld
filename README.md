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
var config = require(CONFIG_PATH);
var dockerManager = require(DockerManager_PATH);
var myDockerManager = new dockerManager.DockerManager(config);
var im = myDockerManager.getImageManager();
var cm = myDockerManager.getContainerManager();
var vm = myDockerManager.getVolumeManager();
//1: pull an image from docker registry.
var createImageParams = {
  "fromImage":"hello-world"//"IMAGE_TO_PULL"
}
return im.createImage(createImageParams);
//2: create a container from the pulled image.
var createContainerName = "TEST_CONTAINER";
var createContainerParams = {
  "Image":"hello-world"//"PULLED_IMAGE"
}
var createContainer = cm.createContainer(createContainerName,createContainerParams);
return createContainer;
//3:start the created container.
return cm.startContainer(createContainerName);
//4:get the created container's logs
return cm.getContainerLogs(createContainerName,{"stderr":1,"stdout":1});
//5: stop the container created previously.
return cm.stopContainer(createContainerName);
//6: remove the created container.
return cm.removeContainer(createContainerName);
//7: list Volumes.
return vm.listVolumes();
//8: removeImage
return im.removeImage("hello-world");
```
