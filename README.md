# Docker Example Tasks for MarkLogic's ML-GRADLE plug-in

Utilize these Groovy tasks along with MarkLogic's ml-gradle Gradle plug-in to deploy MarkLogic configurations to Docker containers. The tasks encapsulate common Docker container tasks such as creating new Docker containers from existing images, removing containers, starting and stopping containers.

An example setting.gradle and build.gradle file along with an example MarkLogic configuration project demonstrate using the Docker tasks. The build.gradle file utilizes Gradle task dependencies to create a Docker container from an existing image then call the "ml-gradle" mlDeploy task to create an example database and MarkLogic REST application server. 

## Requirements

To successfully use these Docker Groovy tasks, make sure you have the following installed and/or created.

* Ensure you have a 64-bit Operating System. *Important: MarkLogic Server runs only on 64-bit operating systems.*
* A Docker image created with MarkLogic Server, Version 9.0-5 or greater installed and started. <http://developer.marklogic.com/products>
* Java, Version 1.8 or greater. <https://java.com/en/download/>
* Gradle, Version 3.5 or greater. <https://gradle.org/install/>
* An internet connection to pull dependencies, including the MarkLogic ml-gradle plug-in from Gradle repositories.

## Using the Example Project

This episode is divided into 2 different folders. These folder represent the final results of each part. We will be running Gradle tasks on the command prompt (Windows) or in Terminal (CentOS/RedHat or MacOS).

To follow along, do the following.

### `part1-app-deploy` instructions.  

1. Copy the folder called `starwarsproject` in the `part1-app-deploy` folder to the same level as the `episode3` folder. Replace any existing `starwarsproject` folder.  

2. The `episode3` folder should now have the following folders:  
	* `part1-app-deploy`
	* `part2-mlcp`
	* `starwarsproject` <-- created when you copied the folder from the `part1-app-deploy` folder in **Step 1**.
3. In command prompt (Windows) or terminal session prompt (CentOS/RedHat or MacOS), change to your new `starwarsproject` folder.
	* Type `cd <path to the episode3 files >/episode3/starwarsproject` then press ENTER.

4. If you entered a port number other than **8090** during the **Episode 2** `mlNewProject` task, be sure to edit the `mlRestPort` value in the `gradle.properties` file and change the value.

5. Note the inclusion of our project application code in the `src/main/ml-modules/root` and `src/main/ml-modules/services` directories.
	* `root` - contains our web page code and javascript needed to display Google Charts in the browser.
	* `services` - contains our MarkLogic Server-side JavaScript code to extend MarkLogic's REST api to query our content database for Star Wars character names and heights. The data will be returned to the browser in JSON format.

5. Run the **ml-gradle** `mlDeploy` task.
	* Type `gradle mlDeploy` then press ENTER.

6. In a web browser, go to <http://localhost:8090/index.xqy> to see the project web page. Edit the port number (:8090) if your value of `mlRestPort` in the `gradle.properties` file is different.
	* No data will display in the charts since we have not yet loaded content to the content database.

7. Do **not** run the `mlUndeploy	` task. We will load content in the next section then refresh our web page to display the data in our Google Charts.

### `part2-mlcp` instructions

1. Copy the folder called `starwarsproject` in the `part2-mlcp` folder to the same level as the `episode3` folder. Replace the existing `starwarsproject` folder.

2. The `episode3` folder should still have the following folders:  
	* `part1-app-deploy`
	* `part2-mlcp`
	* `starwarsproject` <-- created when you copied the folder from the `part1-mlcp` folder in **Step 1**.

3. In command prompt (Windows) or terminal session prompt (CentOS/RedHat or MacOS), change to your new `starwarsproject` folder.
	* Type `cd <path to the episode3 files >/episode3/starwarsproject` then press ENTER.

4. View the Gradle task that will load our content.
	* View the `build.gradle` file in a text editor of your choice.
	* We have added a `repositories` section. This tells Gradle where to look for dependencies in addition to Gradle's default repository.
	* We have added a `configuration` label called `mlcp`. This segregates our mlcp-related dependencies.
	* In the `dependencies` section, we point to where to download the MLCP Java libraries.
	* Finally, we have created a new Gradle task called `deployContent`.

5. Run the **ml-gradle** `mlDeploy` task.
	* Type `gradle mlDeploy` then press ENTER.
	* This will ensure we have all dependencies for loading our content.

6. Deploy the Star Wars character content. Run our custom Gradle  `deployContent` task.
	* Type `gradle deployContent` then press ENTER.

7. In a web browser, refresh the url <http://localhost:8090/index.xqy> to see the project web page. Edit the port number (:8090) if your value of `mlRestPort` in the `gradle.properties` file is different.
	* The Google Charts will now display data from your `starwars-content` database.

8. Undeploy the configuraitons.  
	* Type `gradle mlUndeploy -Pconfirm=true` then press ENTER.
	* Your deployed configurations have been removed from your MarkLgoic Server. You can go to the Admin Interface in your browser to verify. <http://localhost:8001>

## Next Steps
When you have completed `part2-mlcp`, you have finished with the Episode 3 hands-on exercises.

We hope you have enjoyed this episode.
