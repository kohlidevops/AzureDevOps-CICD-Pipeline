# To Deploy the Java Application on Azure Web App Service using Azure CICD Build and Release Pipleine

![AzureCICDPipeline](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/4f2d6cf3-7dcf-4895-84bc-e9dc1c13d31e)

**Azure Pipelines - Types of Pipelines**

**Build Pipeline**

This Pipeline clone the code from the repository and build the project based on the instructions provided in the yaml file.
Typically we use Build Pipelines to publish the Artifacts of the project.

**How to Build Pipelines?**

Login to Azure DevOps console, create organization and projects

Afterwards, You can navigate to Pipelines

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/b0e718e1-8bc6-4d77-9aea-09c337508bbe)

Go to Pipelines

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/865f1075-a90f-4a49-8de4-61d48881f0af)

Create a New Pipeline - Select your source code (my case it would be GitHub)

```
https://github.com/kohlidevops/DevopsBasics
```

Select your Repository

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/a9bfb187-5c91-4325-949c-e9f82324134e)

Now you have to provide the build tool to build the pipeline - This page contains default yaml file which provides information to build the source code. For my case, it is Java based project. I good to go with Maven.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/96980819-64b9-4018-a7e0-734fc2c0cd1e)

Now you have readymade yaml file contains which os image are going to use build, Java version and goals would be "packages" which build a war file for you.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/d5dafd32-0dc7-4b10-a3ca-acca399fc8ea)

Once you save your Pipeline, then Azure will commit this Yaml file to your source code repository.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/c10eff6b-ebfa-4366-82b9-162bfdee116c)

I just saved and created the Pipeline. Now you can see the YAML file in the repository.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/cf6859f8-5c83-42d4-b755-706aafb257e9)

I can see the option to run this pipeline.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/9d96d16b-42ef-4a70-b671-d4307f430a51)

If you start the Build Pipeline,

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/7a93f851-ad63-4639-849c-82d050a5354e)

You can check the build logs if you click on your job. Even we can check the raw log files to analyse.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/eb02f353-270b-4876-ab0c-6c62521fbade)

Below logs are sample log which shows how its assembling, processing and building war file. 

```
2024-07-02T03:16:27.7617816Z Downloaded from central: https://repo.maven.apache.org/maven2/com/thoughtworks/xstream/xstream/1.3.1/xstream-1.3.1.jar (431 kB at 3.8 MB/s)
2024-07-02T03:16:28.0012348Z [INFO] Packaging webapp
2024-07-02T03:16:28.0048410Z [INFO] Assembling webapp [webapp] in [/home/vsts/work/1/s/webapp/target/webapp]
2024-07-02T03:16:28.0128577Z [INFO] Processing war project
2024-07-02T03:16:28.0211971Z [INFO] Copying webapp resources [/home/vsts/work/1/s/webapp/src/main/webapp]
2024-07-02T03:16:28.0328952Z [INFO] Webapp assembled in [28 msecs]
2024-07-02T03:16:28.0541831Z [INFO] Building war: /home/vsts/work/1/s/webapp/target/webapp.war
2024-07-02T03:16:28.0648439Z [INFO] WEB-INF/web.xml already added, skipping
2024-07-02T03:16:28.0728054Z [INFO] ------------------------------------------------------------------------
2024-07-02T03:16:28.0730606Z [INFO] Reactor Summary for Maven Project 1.0-SNAPSHOT:
2024-07-02T03:16:28.0731663Z [INFO] 
2024-07-02T03:16:28.0743617Z [INFO] Maven Project ...................................... SUCCESS [  0.007 s]
2024-07-02T03:16:28.0750259Z [INFO] Server ............................................. SUCCESS [ 14.825 s]
2024-07-02T03:16:28.0754609Z [INFO] Webapp ............................................. SUCCESS [  2.150 s]
2024-07-02T03:16:28.0755824Z [INFO] ------------------------------------------------------------------------
2024-07-02T03:16:28.0761758Z [INFO] BUILD SUCCESS
2024-07-02T03:16:28.0763102Z [INFO] ------------------------------------------------------------------------
2024-07-02T03:16:28.0770477Z [INFO] Total time:  17.163 s
2024-07-02T03:16:28.0777030Z [INFO] Finished at: 2024-07-02T03:16:28Z
2024-07-02T03:16:28.0778220Z [INFO] ------------------------------------------------------------------------
````

**Copying Build Artifacts from the Project to Azure staging directory**

In an Azure DevOps pipeline, the artifacts should copied from the source directory (/1/s) to the artifact staging directory (/1/a). Then, the artifacts from the artifact staging directory are published to the pipeline.

For this, We have to go to our pipelines and edit the pipeline and we have to perform two tasks - one is copy the artifatcs from source directory to artifact staging directory and next task is from artifact staging directory we need to publich the artifacts to pipelines.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/4af072d5-1ace-4746-a24b-61138ae8a06f)

First Task - Copy the artifacts from  source to artifact directory.

We have to choose the "Copy files" from tasks - you can get the assistance from the right side and your cursor should be in last line of the yaml file.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/8bb71f70-717d-49ee-83ef-5fac37d72f89)

```
contents - **/*.war
Target - $(build.artifactstagingdirectory)
```

You can get the Target folder variable from the help button

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/fc3157a3-97f1-4060-bfa4-2f924bb94c1a)

Second Task - Publish build Artifacts from Tasks

```
Path to publish - $(build.artifactstagingdirectory)  //where we need to get the artifacts
Artifact name - warfile  //can be meaningful name
Artifact publish location - Azure Pipelines
Max Artifact Size - 0  //Put 0 if you don't want to set any limit
```

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/42c96d0a-0a76-40e5-9058-fb0f9be0a05f)

Cursor would be in the last line - Then Add.

Finally my yaml files looks below

```
# Maven
# Build your Java project and run tests with Apache Maven.
# Add steps that analyze code, save build artifacts, deploy, and more:
# https://docs.microsoft.com/azure/devops/pipelines/languages/java

trigger:
- master

pool:
  vmImage: ubuntu-latest

steps:
- task: Maven@3
  inputs:
    mavenPomFile: 'pom.xml'
    mavenOptions: '-Xmx3072m'
    javaHomeOption: 'JDKVersion'
    jdkVersionOption: '1.8'
    jdkArchitectureOption: 'x64'
    publishJUnitResults: true
    testResultsFiles: '**/surefire-reports/TEST-*.xml'
    goals: 'package'
- task: CopyFiles@2
  inputs:
    Contents: '**/*.war'
    TargetFolder: '$(build.artifactstagingdirectory)'
- task: PublishBuildArtifacts@1
  inputs:
    PathtoPublish: '$(Build.ArtifactStagingDirectory)'
    ArtifactName: 'warfile'
    publishLocation: 'Container'
```

This Yaml file will start the build and package the war file, then copy the war file from source directory to artifact staging directory. From there this yaml file will publish the artifacts to Azure Pipelines.

Then validate and save this yaml - It will automatically update into the repository.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/60316df0-a714-4f1d-a999-7d4e6be5048d)

Everything updated in my GitHub repository.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/005a2531-1eac-4bca-b5f8-7b9d660b66a4)

If i run the updated pipeline, then we can see the pipeline stages.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/9be679a1-a565-4b53-82fb-38083025e48d)

Awesome! my pipeline is successfully completed.

The artifacts was published after pipeline has been completed.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/d13b7ce5-2ebf-47f1-825d-519392f1f42f)

If you click on the published artifacts, then it will land to the below page.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/4a287ff7-df32-4ef8-94bc-9e5bbbf61a4c)

**Apply Continuous Integration to the Build pipeline for every commit**

I was surprised when I see my pipelines, I triggered manually very first time that is not published anything. The last one pipeline are triggered manually that contains copy files and publish artifacts to pipelines.

Then what is the middle one that I didnt trigger manually. It has been started automatically.

**How it was happened?**
Yes, I have commit the code to the master branch. The yaml file contains below things.

```
trigger:
- master
```

**What does it mean?**

When you commit the code to the master branch then Azure pipeline will trigger and upload the artifatcs to the pipelines.

This pipeline is triggered by my source code repository, not manually.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/95707f3d-5d7c-42de-91d5-d56279f225ac)

After build the Artifacts using Build Pipeline, then we have to create a Release a Pipeline to deploy the Artifacts to the agent or target machine.

**Release Pipeline**

This Pipeline are generally used to deploy the build artifacts into the agent machines or target servers.

**To create a Release Pipeline**

Navigate to Azure devops, then choose - Releases - Create a new Pipeline

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/61503dd8-6a83-4b52-bea5-ad3c0d1f1b0d)

Select - Azure App Service deployment - Which is used to deploy your Artifacts to Azure App Service.

Let me call this stage as QA Environment and save it.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/fbc3342f-b842-4817-a83d-b09837910c65)

As of now I didn't add anything.

**To create a Azure Web App**

Login to portal.azure.com - then create a new service - Choose Azure Web App

```
subscription - Free Trail
Resource Group - Mydemo
Instance name - latchudevopsdemo  //name should be unique
Publish -  Code
Runtime stack - Java 8
Java web server stack - Apache Tomcat 9.0
Operating System - Linux
Region - Central US
Pricing plan - As per your requirement
Then Review and Create // Rest of things let it to be as default
```

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/fa63eba2-f84f-4d03-94b0-eedfcd919582)

This Web App Service deployment has been completed. (Web App will not create when you are in free trail)

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/9ff274cd-a1f6-42d3-bbd9-87a479e7546a)

After deployment you can access the default Web App page with the help of URL given by Azure.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/42e9c15b-bbc3-480b-acee-e2bbc2be3de0)

**To deploy the Artifacts to Web App**

Go back to Release pipeline - Select your QA Environment and Add the task.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/d5797258-ed3c-43f6-8831-ad7918028333)

```
Stage name - QA Environment
Azure subscription - Select your subscription - Authorize it.
App type - Web App on Linux
App service name - latchudevopsdemo  //as you defined while creating Web App
Save it
```

Again Select your Agent what we created just now.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/a0019437-4bc3-4b80-b3f4-5acaa4934d16)

```
Package or folder -  $(System.DefaultWorkingDirectory)/**/*.war
// DefaultWorkingDirectory is a Pipeline working directory
// From this working directory we can get the war file which was published in Build Pipeline stage
Change the pipeline as Deploy QA - Then Save it
```

We can add the deployment stage as Deploy QA. Then where is the Artifacts stage.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/57404c66-edba-42c9-9713-117446dc5a8c)

**To add the Artifact stage**

```
Project - mydevops
Source (build pipeline) - kohlidevops.DevopsBasics (2)
Default version - Latest
Source alias - _kohlidevops.DevopsBasics (2)
// These things are auto populated once you select your project in Artifacts stage
```

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/d1f3ca45-7ebe-42fc-8433-81d78ddb4082)

Finally save this project from accidental deletion.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/aa756175-a0e4-4b07-9e26-b6439d076457)


**Create Release**

This Pipeline helps us to build end to end Pipeline (connecting Build and Release Pipelines) for CICD implementation.

To start a Create Release

Once we create a Release Pipeline and add the Artifact stages then we can start the Create Release Pipeline.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/57e5f785-9f43-46fc-988c-69dd822b11dc)

Then Create a new Release and Create it. If you click a your release then you can see the running pipeline.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/a20f9679-f0ad-4cb6-b4ee-d338679c6aa0)

If you click on the logs, then we can see successfully deployed artifatcs to our Azure Web App.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/73fd7e14-d528-49e0-8b68-8d7ca84ce1f0)

Everything completed as per below image shown.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/bb707d99-8964-44c6-b4bf-d08d57fa204f)

Now I can access the Java App using below Web App URL

```
https://latchudevopsdemo.azurewebsites.net/webapp/
```

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/1dcdb917-8d6e-42ad-9957-4ab596bc09bd)

Its deployed successfully!

**How to achieve Continuous Deployment?**

**For CI** - When we make changes to the source code repository, then Build Pipeline will automatically trigger and creates a new artifacts that is war file.

**For CD** - When a new artifacts are created then we have to trigger a Release Pipeline. To achieve this, we can simply enable the Continuous Deployment in the Pipeline as per below image - Its very simple.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/5281d630-e1e0-4705-b7a8-d44556239514)

Go to your Pipeline and edit and enable the Continuous Deployment. Thats it.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/fa6635a4-3d92-47f8-aa5c-eaf6a3cba3d7)

Now do some changes in the source code repository and lets check how build and release piplines are working together and how artifacts automatically deployed on to the Azure Web App Service which is going to act as a QA Envirnonment for testing.

Build Pipeline has been completed and my Release-2 has been started.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/b1a50745-539a-4a6e-ad80-75ece0ac95c1)

Yup! My updated source code has been deployed on the target machine that is QA Environment.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/0855fd66-519d-426f-bd96-bd19cd6c1bbd)

I would like to test my QA Environment with selenium. For this, I have a code to test the app when artifacts deployed on Azure Web App. I can use below code for automation testing.

```
https://github.com/kohlidevops/Automation.git
```

Here I need to import this repo to Azure DevOps Repos. Its very simple.

How to add the Automation testing repository to Azure DevOps Repos?

Navigate to Azure DevOps panel and select the repos then import the below url.

```
https://github.com/kohlidevops/Automation.git
```

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/27f15a74-fe06-4282-abf3-580042606676)

It has been imported successfully!

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/ad250798-7d09-45a9-b7dc-093548fc94b6)

Once deployed, you have to changes this two lines highlighted. 

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/1f6df2ab-0be1-433d-bfcb-845577519447)

```
location - src/test/java/Academy/BrowserTest.java
driver.get - should be - your Web App URL
Assert.assertTrue(text.equalsIgnoreCase("Hi Latchu"));
\\ It will check the your web page and will pass if this string equals
```

**How to add the Test Automation Jobs to the Azure Release Pipeline?**

To start a empty job after the QA Environment deployment

Navigate to your Release Pipeline and edit the Pipeline - add the new stage after QA Environment stage.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/24171628-e2c8-4ca6-8346-8685a5064443)

Add a empty job and name it as AutomationTest and save the Release Pipeline.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/5c80a970-6208-4275-9347-f13cc226fe0a)

To add the AutomationTest repo in the Artifact stage

After adding the AutomationTest repo into the Artifact stage, then only this Release Pipeline has access to the AutomationTest repo.

```
Add an Artifact
Source type - Azure Repos
Project - mydevops //because my project name is mydevops
Source (repository) - mydevops
Default branch - master
Default version - Latest from the default branch
Source alias - _mydevops
Add
```

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/c714c301-5ad3-4811-b649-04df4af67041)

Now, This AutomationTest Artifact has been added successfully.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/ee5a534b-9f09-4989-acec-f361daddfa4f)

From the Release Pipeline, the AutomationTest stage can call this repo. Now you can add your tasks in the AutomationTest stage with the help of View stage tasks.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/5ce2e42d-dc9b-49a8-897c-d18f82299769)

To click on the Agent Job with select '+' symbol and search Maven then add this.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/6bf2f694-551f-4ae8-85f7-24fd9865b172)

Then click on the Maven pom.xml which you have been added now, 

```
Display name - Maven pom.xml
Maven POM file - select your pom.xml //with the help of 3 dots and select your pom.xml of AutomationTest repo.
Goals - test  //we have test the project and make the test results as xml file.
```

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/55d30146-74f7-49f2-910e-5f4121f6d051)

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/63c46e39-97ae-4312-a4b5-2814d99f0016)

Lets save this stage. Now do some changes in the project source code repository and ensure all the stages are running automatically that is build the artifacts, artifacts deplyoed on the QA Environment and Regression AutomationTest has been invoked on the QA Environment.

Everything working fine - Arifacts deployed on the QA Evironment, then Regression testing has been started.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/20cd5a35-f58b-468e-937c-ee2f6ba30cd3)

My App has updated code.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/df0fdf1a-2b0c-42cc-a453-1415310591c5)


Perfect! Regression Test has been completed.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/a2b3da9d-d73c-488c-afc4-27371174bafe)

Time to push this artifact to Producion Environment.

**How to deploy the Artifact to the Production Environment?**

Lets create one more Azure Web App and we can treat as Production Environment. Navigate to portal.azure.com and create one more Web App.

```
Subscription - Azure Subscription 1
Resource Group - demo
Name - latchudevopsprod
Publish - code
Runtime stack - Java 8
Java web server stack - Apache Tomcat 9.0
Operating System - Linux
Region - Central US
```

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/b7f7ce28-bf80-44ad-89af-1c0fbee77475)

The deployment has been completed.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/3ac2e8b8-e09b-48eb-b8a3-d1a140c838b9)

**To add the Production Web App Environment to the Release Pipeline stage**

Go back to Release Pipeline and edit the stage.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/d5b2e054-00a2-4b5d-85cd-cd845bff094d)

Add a Task - Stage in the RegressionAutomationTesting stage. Select Azure App Service deployment and Name it as ProductionDeployment.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/c23a9cff-7bfe-4a3d-8b63-f4783860756a)

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/c183bd70-de76-4ff7-87d1-d6d57d8458ce)

The view the tasks in ProductionDeployment and add the parameters given below.

```
Azure subscription - subscription id
App type - Web App on Linux
App service name - latchudevopsprod
Package or folder - $(System.DefaultWorkingDirectory)/**/*.war
```

Then select the agent and provide the war file location into the package and save it.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/cf514a8d-dc48-4ef0-88d8-09f7363e53e3)

Now, How looks like our CICD pipeline?

Let me update my Pipeline name as Azure CICD Pipeline

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/84e99777-7f4d-4e7a-9e9f-2224ca5d089c)

1. When source code has been changed, the Build Pipeline will generate the Artifacts.
2. Then Artifacts are deployed on QA Environment for testing.
3. RegressionTesting will happen on the QA Environment using Azure Repos code.
4. If test has been passed, then this Artifacts will deploy on the Production Environment.
5. This is the way I have created CICD Pipeline for Azure App Service.

As of now, My Production Environment URL has no application.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/fca6836b-c807-41fe-bd4c-0abe69ab00e3)

Now I'm going to Create Release Pipeline and let see what happen

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/f627ce36-b21e-4aec-a119-195e0c03875f)

As I manually triggered, its performing all the actions and now artifatcs are deploying on the Production Environment.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/df9059c2-0715-4212-a62a-989e2d4fc4fb)

Finally, My Pipeline has been completed.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/5d805723-66bb-4fad-96e4-f469335841bd)

If I hit my Production Environment URL, then

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/9b426052-c367-46ca-aa08-ba62a292bfad)

If I'm going to update my source code, then I ensure whether it is automatically deployed when changes occur in the source code repository.

![image](https://github.com/kohlidevops/AzureDevOps-CICD-Pipeline/assets/100069489/ccefbd1c-d8b1-4a4e-954d-7f452985b6b0)

That's it. I have successfully deployed the Artifacts on the ProductionEnvironment using Azure CICD Pipeline.
