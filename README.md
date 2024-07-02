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

**Release Pipeline**

This Pipeline are generally used to deploy the build artifacts into the agent machines or target servers.

**Create Release**

This Pipeline helps us to build end to end Pipeline (connecting Build and Release Pipelines) for CICD implementation.

