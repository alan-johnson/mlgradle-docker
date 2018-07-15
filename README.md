# Docker Example Tasks for MarkLogic's ML-GRADLE plug-in

Utilize these Groovy tasks along with MarkLogic's ml-gradle Gradle plug-in to deploy MarkLogic configurations to Docker containers. The tasks encapsulate common Docker container tasks such as creating new Docker containers from existing images, removing containers, starting and stopping containers.

An example setting.gradle and build.gradle file along with an example MarkLogic configuration project demonstrate using the Docker tasks. The build.gradle file utilizes Gradle task dependencies to create a Docker container from an existing image then call the "ml-gradle" mlDeploy task to create an example database and MarkLogic REST application server. 

## Requirements

To successfully use these Docker Groovy tasks, make sure you have the following installed and/or created.

* Ensure you have a 64-bit Operating System. *Important: MarkLogic Server runs only on 64-bit operating systems.*
* The "**Red Hat Enterprise Linux / CentOS, Version 7**" version of the MarkLogic Server installer, Version 9.0-5 or greater downloaded. <http://developer.marklogic.com/products>
* Java, Version 1.8 or greater. <https://java.com/en/download/>
* Gradle, Version 3.5 or greater. <https://gradle.org/install/>
* An internet connection to pull dependencies, including the MarkLogic ml-gradle plug-in from Gradle repositories.

## How to add the `mlgradle-docker` tasks
Follow the steps below to add the `mlgradle-docker` task to your Gradle project.

1. Download or clone the `mlgradle-docker` GitHub project.
2. Copy the `mlgradle-docker` folder to your main Gradle project folder.
3. Create or edit a `settings.gradle` file in your main Gradle project folder.
4. Edit the `gradle.properties` properties in the `mlgradle-docker` folder.
5. Begin using the `mlgradle-docker` tasks defined in the `build.gradle` file within the `mlgradle-docker` folder. 

### List of Docker-related tasks
Below is a list of the Docker-related tasks in the mlgradle-docker's `build.gradle` file.

`createDockerContainer` - Create a Docker container from a previously created Docker image.

**Depends on the following properties:**

* ml-gradle properties:
	* mlAppServicesPort - Defined by ml-gradle. MarkLogic's Management port.
	* mlRestPort - Defined by ml-gradle. Application REST instance port.

* mlgradle-docker project properties:
	* dockerMapDataDirVolPath - optional host directory to map the Docker container's MarkLogic data directory.
	* dockerContainerName - name to give the Docker container.
	* dockerImageName - name of the Docker image to create the container.
	* dockerContainerCreateSleepTime - optional sleep time to allow MarkLogic to begin listening on the admin port (default=8001).

`deleteDockerContainer` - Stop then remove the named docker container.
 
**Depends on the following properties:**

* mlgradle-docker project properties:
	* dockerContainerName - name to give the Docker container.

`startDockerContainer` - Start a named docker container. Does not create the container.

**Depends on the following properties:**

* mlgradle-docker project properties:
	* dockerContainerName - name to give the Docker container.

`stopDockerContainer` - Stop a named docker container but do not delete the container.

**Depends on the following properties:**

* mlgradle-docker project properties:
	* dockerContainerName - name to give the Docker container.

`createDockerImage` - Create a Docker image.

**Depends on the following properties:**

* docker project properties:
	* dockerImageName - name to give the Docker container.
	* dockerFilename - name and path to the Docker build file (default: "DockerFile" in the current working directory).

`getMarkLogic` - Download MarkLogic from the MarkLogic product download URL. 

**Depends on the following properties:**

* mlgradle-docker project properties:
	* dockerMlDownloadUrl - MarkLogic product download URL.

To get the MarkLogic product download url, follow the following steps. 

1. From a browser, go to <http://developer.marklogic.com/products>. 
2. Click the link for one of the supported OS platforms.
3. Sign in with your MarkLogic Community credentials. If you do not have a MarkLogic Community credential, you may create one.
4. Click the "I accept the terms in the MarkLogic Developer License Agreement." checkbox.
4. Click the "Download via CURL" button.
5. Click either of the Copy to Clipboard buttons to copy the One-time use URL to the clipboard.
6. Paste the URL into the `dockerMlDownloadUrl` property in the mlgradle-docker's `gradle.properties` file.

## Running the example

The example demonstrates the following:

* Create a Docker Container based upon an existing Docker image using Gradle and the added "mlgradle-docker" tasks.
* Deploy a simple demo web application using Gradle and MarkLogic's ml-gradle plugin.

To run the demonstration, follow the steps below.

### Instructions For The`example` Project.  

1. Download or clone the `mlgradle-docker` GitHub project.
2. Create a Docker image and tag the image `ml:9`.
	* Download the "**Red Hat Enterprise Linux / CentOS, Version 7**" version of the MarkLogic Server installer from <http://developer.marklogic.com/products>.
	* Copy the downloaded MarkLogic .RPM installer file to the  `mlgradle-example` directory within the`example` directory of the downloaded `mlgradle-docker` project.
	* Edit the `Dockerfile` located in the `mlgrade-example` directory within the `example` directory.
	* In the `Dockerfile`, on Line 18, make sure the .RPM filename after the Docker `COPY` command is the same as the downloaded .RPM file from MarkLogic.
	* Save any changes to the `Dockerfile`.
	* Edit the `gradle.properties` file in the same directory as the `Dockerfile`.
	* Set the `dockerImageName` property to the value of `ml:9`.
		* Example: `dockerImageName=ml:9` 
	* Open a Terminal shell or Windows Command Prompt.
	* Change to the `mlgradle-docker/example/ml-gradle` directory.
	* Type `gradle createDockerImage` then press ENTER.
	* When finished, type `docker images` then press ENTER. Ensure the docker image tagged `ml:9` has been created.
3. In a Terminal shell or Windows Command Prompt, change to the `mlgradle-docker/example` directory.
4. Type `gradle ":mlgradle-docker:deployWithDocker"` then press ENTER.
5. After the build completes, open a browser. Chrome or Firefox is recommended.
6. In the browser, navigate to `http://localhost:8001`. On the right-hand side, note the MarkLogic application server named `starwars-gradle` on port 8090. Also, note a MarkLogic content database has been created named `starwars-content` and a MarkLogic module database has been created named `starwars-modules`. MarkLogic forests for these databases have also been created.
7. In the browser, navigate to `http://localhost:8090`. 
8. Log into the demonstration application with your MarkLogic administrator username and password.
9. Note the application displays data in the charts from the content that was also loaded in the `:mlgradle-docker:deployWithDocker` Gradle task.

## Next steps
Examine the build.gradle file in the `mlgradle-docker/example` directory. In the `deployWithDocker` Gradle task, note the dependancies on other tasks. The ordering of these tasks utilizes Gradle's `.mustRunAfter` property for these tasks. 

The Docker-related tasks can be run separately. Be sure to include the `mlgradle-docker` directory in your `settings.gradle` file. See the included `settings.gradle` file as an example.

I hope you find these Docker Gradle tasks useful.
