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

**Release Pipeline**

This Pipeline are generally used to deploy the build artifacts into the agent machines or target servers.

**Create Release**

This Pipeline helps us to build end to end Pipeline (connecting Build and Release Pipelines) for CICD implementation.

