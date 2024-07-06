# AzureDevOps-CICD-Pipeline

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

![Uploading image.png…]()

Its deployed successfully!
