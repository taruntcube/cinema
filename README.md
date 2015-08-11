# What's In Theaters application overview
The What's In Theaters application showcases the best practices for building a Watson conversational solution by using the Watson Dialog service. The application allows users to converse with Watson to search for current or upcoming movies by using [themoviedb.org](https://www.themoviedb.org/) database. Here's a [quick demo](http://watson-movieapp-dialog.mybluemix.net/watson-movieapp-dialog/dist/index.html#/).

The application uses a conversational template that you can customize to suit your own application. This domain-independent template will help you to structure your dialogs according to how people naturally converse, thus helping you to bootstrap your future dialogs.

## How it works
Users talk in natural language to the system to find movies that match the search criteria they've specified. The system is built to understand natural language that relates to searching for and selecting movies to watch. For example, saying “I'd like to see a recent R rated drama” causes the system to search the movie repository and to return the names of all R-rated dramas that have been released in the last 30 days.

The system is designed to obtain the following types of information about movies from users before it searches the repository:

  * **Recency**. The system determines whether users want to know about currently playing movies or upcoming movies.
  * **Genre**. The system understands movie genres, such as action, comedy, and horror.
  * **Rating**. The system understands movie rating, such as G, PG-13, and R.

Users can search across all genres and ratings simply by answering "no" to the corresponding questions. Before the system searches the movie repository, it needs to know whether a user prefers current movies or upcoming movies. The system understands variations of text, so users can rephrase their responses and the system will still process them. For example, the system might ask, "Do you want to watch an upcoming movie or one that's playing tonight?" Users can say "tonight" or "Show me movies playing currently," and the system understands that both answers mean that users want to know about current movies.

## Before you begin
Ensure that you have the following prerequisites before you start:
* An IBM Bluemix account. If you don't have one, sign up for it [here](https://apps.admin.ibmcloud.com/manage/trial/bluemix.html?cm_mmc=WatsonDeveloperCloud-_-LandingSiteGetStarted-_-x-_-CreateAnAccountOnBluemixCLI). For more information about the process, see [Developing Watson applications with Bluemix](http://www.ibm.com/smarterplanet/us/en/ibmwatson/developercloud/doc/getting_started/gs-bluemix.shtml).
* [Java Development Kit](http://www.oracle.com/technetwork/java/javase/downloads/index.html) 1.7 or later releases
* [Eclipse IDE for Java EE Developers](https://www.eclipse.org/downloads/packages/eclipse-ide-java-ee-developers/marsr)
* [Apache Maven](https://maven.apache.org/download.cgi) 3.1 or later releases
* [Git](https://git-scm.com/downloads)
* [Websphere Liberty Profile server](https://developer.ibm.com/wasdev/downloads/liberty-profile-using-non-eclipse-environments/), if you want to run the app in your local environment
* Request an API key from themoviedb.org. For more info see the [API documentation](https://www.themoviedb.org/documentation/api).

## Setup
In order to run the What's In Theaters app, you need to have a Dialog service instance bound to an app in your IBM Bluemix account. The following steps will guide you through the process. The instructions use Eclipse, but you can use the IDE of your choice.

### Get the project from GitHub
1. Clone the movieapp-dialog repository from GitHub by issuing one of the following commands in your terminal:
   ```
   git clone https://github.com:watson-developer-cloud/movieapp-dialog.git
   ```
   ```
   git clone git@github.com:watson-developer-cloud/movieapp-dialog.git
   ```

2. Add the newly cloned repository to your local Eclipse workspace.

### Set up the Bluemix Environment
#### Creating an App
1. [Log in to Bluemix](https://console.ng.bluemix.net/) and navigate to the *Dashboard* on the top panel.
2. Create your app.
      1. Click **CREATE AN APP**.
      2. Select **WEB**.
      3. Select the starter **Liberty for Java**, and click **CONTINUE**.
      4. Type a unique name for your app, such as `dialog-sample-app`, and click **Finish**.
      5. Select **CF Command Line Interface**. If you do not already have it, click **Download CF Command Line Interface**. This link opens a GitHub repository. Download and install it locally.

#### Adding an instance of the Dialog service
Complete one of the following sets of steps to add an instance of the Dialog service. Bluemix allows you to create a new service instance to bind to your app or to bind to an existing instance. Choose one of the following ways:  

**Creating a new service instance to bind to your app**
  1. [Log in to Bluemix](https://console.ng.bluemix.net/) and navigate to the *Dashboard* on the top panel. Find the app that you created in the previous section, and click it.
  2. Click **ADD A SERVICE OR API**.
  3. Select the **Watson** category, and select the **Dialog** service.
  4. Ensure that your app is specified in the **App** dropdown on the right-hand side of the pop-up window under **Add Service**.
  5. Type a unique name for your service in the **Service name** field, such as `dialog-sample-service`.
  6. Click **CREATE**. The **Restage Application** window is displayed.
  7. Click **RESTAGE** to restart your app. If the app is not started, click **START**.

**Binding your app to an existing service instance**
  1. [Log in to Bluemix](https://console.ng.bluemix.net/) and navigate to the *Dashboard* on the top panel. Locate and click on the app you created in the previous section.
  2. Click **BIND A SERVICE OR API**.
  3. Select the existing Dialog service that you want to bind to your app, and click **ADD**. The **Restage Application** window is displayed.
  4. Click **RESTAGE** to restart your app.  
   
#### Setup required environment variables
  In order to run the What's In Theaters application on Bluemix two further environment variables are required:  
  * **DIALOG_ID**: The ID of the dialog instance you will create below in the **Upload a Dialog File** section. 
  * **TMDB_API_KEY**: An API key obtained from themoviedb.org.  

  1. Navigate to the application dashboard in Bluemix.
  2. Locate and click on the application you created previously. 
  3. Navigate to the _**Environment Variables**_ section of the UI. 
  4. Switch to the **USER-DEFINED** tab within the UI.
  5. Add these new environment variables:
    * **DIALOG_ID**: The value is the dialog ID returned when you upload the dialog file.
    * **TMDB_API_KEY**: The value is the API key obtained from themoviedb.org.  

To view the home page of the app, open [https://yourAppName.mybluemix.net](https://yourAppName.mybluemix.net), where yourAppName is the name of your app.

#### Upload a dialog file
After the service instance is bound to the app, use the credentials that you received in the previous step to author a dialog. The dialog file for this application is packaged with the project at `/movieapp-dialog/src/main/resources/dialog_files/movieapp-dialog-file.xml`. Use the following cURL command to upload this file to Bluemix:
```
curl -X POST -F "file=@*dialogFile*" -F "name=*dialogName*" https://gateway.watsonplatform.net/dialog-beta/api/v1/dialogs -u "*username*:*password*"
```
where, dialogFile is the name of the dialog file you are uploading, dialogName is a unique name you give to the dialog you are uploading, dialogServiceType is the type of service ("dialog-experimental" or "dialog-beta") and, the username and password are the credentials you obtained in the previous step. 


### Build the app
This project is configured to be built with Maven. To deploy the app, complete the following steps in order:
  1. In your Eclipse window, expand the *movieapp-dialog* project that you cloned from GitHub.
  2. Right-click the project and select `Maven -> Update Project` from the context menu to update Maven dependencies.
  3. Keep the default options and click **OK**.
  4. Navigate to the `movieapp-dialog/src/it/resources/` directory.
  5. Open the `server.env` file, and update the following entries:
    * **DIALOG_ID**. Specify the ID value that corresponds to your Dialog service account on Bluemix(the dialog id is a long alpha-numeric string).
    * **TMDB_API_KEY**: Specify the API key you received after you registered for API access on themoviedb.org.
    * **VCAP_SERVICES**: Specify the JSON object obtained from the *Environment Variables* section of your application on Bluemix. The following JSON object is an example:  
    ```json
    {
        "dialog": [
            {
               "name": "dialog-service",
               "label": "dialog-sample-service",
               "plan": "beta",
               "credentials": {
                  "url": "https://sampleplatformurl.net/samplePath",
                  "username": "unique_id",
                  "password": "password"
               }
            }
       ]
    }
    ```
    Specify the JSON object in the `server.env` file on one line, as shown in the following example:
  ```
  VCAP_SERVICES= {"dialog": [{"name": "dialog-service","label": "dialog-sample-service",....}]}
  ```
  6. Switch to the navigator view in Eclipse, right-click the `pom.xml`, and select `Run As -> Maven Install`. The Maven install invokes the build process. During the 'install', the following tasks are performed:
    * The JS code is compiled. That is, the various Angular JS files are aggregated, uglified, and compressed. Various other pre-processing is performed on the web code, and the output is copied to the `movieapp-dialog/src/main/webapp/dist` folder in the project.
    * The Java code is compiled, and JUnit tests are executed against the Java code. The compiled Java and JavaScript code and various other artifacts that are required by the web project are copied to a temporary location, and a `.war` file is created.
    * The Maven install instantiates a new Websphere Liberty Profile server, deploys the `.war` file to the server, starts the server, and runs a battery of integration tests against the deployed web application.

This WAR file that resides in */movieapp-dialog/target directory* will be used to deploy the application on Bluemix in the next section.


### Deploy the app
You can run the application on a local server or on Bluemix. Choose one of the following methods, and complete the steps:
#### Deploying the app on your local server in Eclipse
1. Start Eclipse, and click `Window -> Show View -> Servers`.
2. In the **Servers** view, right-click and select `New -> Server`. The *Define a New Server* window is displayed.
3. Select the **WebSphere Application Server Liberty Profile**, and click **Next**.  
4. Configure the server with the default settings.  
5. In the **Available** list in the **Add and Remove** dialog, select the *movieapp-dialog* project and click **Add >**. The project is added to the runtime configuration for the server in the **Configured** list.
6. Click **Finish**.
7. Copy the `server.env` file which was edited previously from `movieapp-dialog/src/it/resources/server.env` to the root folder of the newly defined server, `wlp/usr/defaultserver/server.env`.  
8. Start the new server.
9. Open [http://localhost:serverPort/watson-movieapp-dialog/dist/index.html#/](http://localhost:serverPort/watson-movieapp-dialog/dist/index.html#/) in your favorite browser, where yourAppName is the specific name of your app.
10. Chat with What's In Theaters!

#### Deploying the app on the Websphere Liberty Profile in Bluemix
Deploy the WAR file that you built in the previous section by using Cloud Foundry commands.
1. Open the command prompt.
2. Navigate to the directory that contains the WAR file you that you generated by running the following command in the terminal:
   ```
   cd movieapp-dialog/target
   ```

3. Connect to Bluemix by running the following command:
   ```
   cf api https://api.ng.bluemix.net
   ```

4. Log in to Bluemix by running the following command,
   ```
   cf login -u <yourUsername> -o <yourOrg> -s <yourSpace>
   ```
where yourUsername is your Bluemix ID, yourOrg is your organization name in Bluemix, and yourSpace is your space name in Bluemix.
5. Deploy the app to Bluemix by running the following command.
   ```
   cf push <yourAppName> -p watson-movieapp-dialog.war
   ```
where yourAppName is the name of your app.
6. Navigate to [Bluemix](https://console.ng.bluemix.net/) to make sure the app is started. If not, click **START**.
7. To view the home page of the app, open [https://yourAppName.mybluemix.net/watson-movieapp-dialog/dist/index.html#/](https://yourAppName.mybluemix.net/watson-movieapp-dialog/dist/index.html#/), where yourAppName is the specific name of your app.
8. Chat with What's In Theaters!

## Automation
Automation is designed both to run as a full suite of regression tests and to run a subset of regression tests for continuous integration process. Updates to dialogs require reciprocating changes to the JSON files, as described in the following sections.

### Setting up for full regression
Use `src/it/resources/testConfig.properties` to control the server under test, the browser type 
(Firefox and Chrome are supported), and the Selenium-Grid server to include, if you have one in your environment.

### Running the full regression suite
To run the full regression suite, you can run the GUI_TestSuite and Rest_TestSuite Junit Test suites directly in your Eclipse environment. 

### Accommodating changes to the dialog
The dialog questions and answers from the XML file, as described in [Upload Dialog File](#upload-a-dialog-file) map directly to question answer arrays in JSON files that are used by the automation. These files are located in the `src/it/resources/questions` directory. When you change dialogs, you must do reciprocating changes to the JSON config files to allow the regression test suite to pass.

## Reference information
* [Dialog service documentation](http://www.ibm.com/smarterplanet/us/en/ibmwatson/developercloud/dialog.html): Get an in-depth knowledge of the Dialog service.
* [Dialog service API documentation](http://www.ibm.com/smarterplanet/us/en/ibmwatson/developercloud/apis/dialog-apis.html): Understand API usage.
* [Dialog service tutorial](http://www.ibm.com/smarterplanet/us/en/ibmwatson/developercloud/doc/dialog/#tutorial_basic): This basic tutorial shows you a step-by-step process for designing a dialog for a specific scenario. 
* [Natural conversation tutorial](http://www.ibm.com/smarterplanet/us/en/ibmwatson/developercloud/doc/dialog/#tutorial_advanced): The What's In Theaters app uses a natural conversation template as the basis for the dialog. To design your own dialog in natural conversation, complete this tutorial. See the template here: `/movieapp-dialog/src/main/resources/dialog_files/CA_Trans_Template.xml`.
