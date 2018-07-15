# mlgradle-docker Example Project for MarkLogic's ML-GRADLE plug-in

The `example` folder contains an sample project that creates a Docker container with MarkLogic installed. The project, then, deploys a demonstration MarkLogic project and content. 

Within the `example` folder, there are 2 important files, `settings.gradle` and `build.gradle`. 

The project's `settings.gradle` Gradle script:

* Demonstrates including the `mlgradle-docker` ml-gradle plugin extension tasks to a project.

The project's `build.gradle` Gradle script:

* Creates a Docker container and installs MarkLogic inside the container.
* Initializes the MarkLogic Server.
* Creates a MarkLogic administrator account.
* Deploys the demonstration project, creating MarkLogic application servers and databases.
* Loads demonstration content to the MarkLogic database.

## Requirements

To successfully run the `example` demonstration, make sure you have the following installed and/or created.

* Ensure you have Docker installed, version 18.03 or higher. Also, ensure that Docker is currently running. <https://www.docker.com/community-edition#/download>
* If you are using Windows, ensure that Docker is set to use "Linux Containers" rather than "Windows Containers". See <https://docs.docker.com/docker-for-windows/#switch-between-windows-and-linux-containers> and scroll to the topic, "**Switch between Windows and Linux containers**".
* Download the "**Red Hat Enterprise Linux / CentOS, Version 7**" version of the MarkLogic Server installer, Version 9.0-6 or greater from <http://developer.marklogic.com/products>
* Java, Version 1.8 or greater. <https://java.com/en/download/>
* Gradle, Version 3.5 or greater. <https://gradle.org/install/>
* An internet connection to pull dependencies, including the MarkLogic ml-gradle plug-in from Gradle repositories.

## Running the example

To run the example demonstration, follow the steps below.

### Instructions For The`example` Project.  

1. Download or clone the `mlgradle-docker` GitHub project.
2. If you have MarkLogic currently running on your host computer, please stop MarkLogic. 
3. Ensure the following ports are unused on your host computer:

	* 8000 through 8010, for MarkLogic
	* 8090, for the sample project's MarkLogic application server.
 
4. Configure a Docker build file for building a Docker image.
	* Download the "**Red Hat Enterprise Linux / CentOS, Version 7**" version of the MarkLogic Server installer from <http://developer.marklogic.com/products>.
	* Copy the downloaded MarkLogic .RPM installer file to the  `example/mlgradle-docker` directory inside the `mlgradle-docker` project folder.
	* Edit the `Dockerfile` located in the `example/mlgrade-docker` directory.
	* In the `Dockerfile`, on Line 18, make sure the .RPM filename after the Docker `COPY` command is the same as the downloaded .RPM file from MarkLogic.
	* Save any changes to the `Dockerfile`.
5. Edit the `gradle.properties` file in the `example/mlgradle-docker` directory.
	* Set the `dockerImageName` property to the value of `ml:9`.
		* Example: `dockerImageName=ml:9`
	* Save any changes to the `gradle.properties` file. 
	* Open a Terminal shell or Windows Command Prompt.
	* Change to the `mlgradle-docker/example/ml-gradle` directory.
	* Type `gradle createDockerImage` then press ENTER.
	* When finished, type `docker images` then press ENTER. Ensure the docker image tagged `ml:9` has been created.
6. In a Terminal shell or Windows Command Prompt, change to the `mlgradle-docker/example` directory.
7. Type `gradle deployWithDocker` then press ENTER.
8. After the build completes, open a browser. Chrome or Firefox is recommended.
9. In the browser, navigate to `http://localhost:8001`. 
10. Log into MarkLogic with the username of `admin` and the password of `admin`.
>	The username and password are configured in the `mlgradle-docker/example` gradle.properties file. The `mlUsername` and `mlPassword` properties in the gradle.properties file are properties of the `ml-gradle` plugin control the administrator account that is created with the ml-gradle `mlInstallAdmin` task. 
11. On the right-hand side, note the MarkLogic application server named `starwars-gradle` on port 8090. Also, note a MarkLogic content database has been created named `starwars-content` and a MarkLogic module database has been created named `starwars-modules`. MarkLogic forests for these databases have also been created.
12. In the browser, navigate to `http://localhost:8090/index.xqy`. 
13. Log into the demonstration application with your MarkLogic administrator username of `admin` and password of `admin`.
14. The application displays data in the charts from the content that was also loaded in the `deployWithDocker` Gradle task.
>	The `deployWithDocker` task is defined in the `mlgradle-docker/example` build.gradle file. This demonstrates a simple project deployment to a Docker container with MarkLogic installed. The `deployContent` task, in the build.gradle file, is an example of using MarkLogic's Content Pump via a Gradle task to load content to a MarkLogic database.
15. Deleting the Docker container will delete the application, data and MarkLogic. There's no need to undeploy the application or data from MarkLogic before deleting the container.
	* In a Terminal shell or Windows Command Prompt, change to the `mlgradle-docker/example` directory.
	* Type `gradle :mlgradle-docker:deleteDockerContainer` then press ENTER.

## Next steps
To learn more about MarkLogic's `ml-gradle` plugin:

* Visit the GitHub project at <https://github.com/marklogic-community/ml-gradle>.
* See the `ml-gradle` Wiki at <https://github.com/marklogic-community/ml-gradle/wiki>.
* Learn more about extending `ml-gradle` at <https://github.com/marklogic-community/ml-gradle/wiki/Writing-your-own-task> and <https://github.com/marklogic-community/ml-gradle/wiki/Writing-your-own-command>.
* Learn about `ml-gradle` with MarkLogic's On Demand video series at <http://mlu.marklogic.com/ondemand>. Select the **ml-gradle** topic under **Series**. 
