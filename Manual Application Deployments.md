# Manual Application Deployment Guide

## Shared Instructions for First-Time and Re-deploy
### Publish
#### Create Publishing Profile
Publish your application using Visual Studio. Make Sure to have a location in mind to deploy the files, e.g. H:\Application\\. Afterwards, open your project in Visual Studio and right-click the startup project for your solution and click "Publish..." as shown below:
![Publish by right-clicking startup project and click Publish...](/General%20Development/Manual%20Application%20Deployment%20Images/pub_startup.png)

Now you will need to create a publishing profile. Select Folder and click Next as shown below:

![Select Folder and click Next](/General%20Development/Manual%20Application%20Deployment%20Images/select_publish_target.png)

Afterwards, referring to the location you decided upon earlier on this sub-guide, you will need to specify that location in the Folder Location field (create folders as needed) as shown below and click Finish:

![Specify directory in Folder Location Field (create folders as needed) and click Finish](/General%20Development/Manual%20Application%20Deployment%20Images/select_publish_directory.png)

Your window for creating a publishing profile will disappear when it is done unless you click the checkbox on the following screen.

#### Publish using Publishing Profile
If you are not coming from the previous step and already have a publishing profile, then open your project in Visual Studio and right-click the startup project for your solution and click "Publish..." as shown below:

![Publish by right-clicking startup project and click Publish...](/General%20Development/Manual%20Application%20Deployment%20Images/pub_startup.png)

You should be presented with your publishing profile that you created in an earlier step. I recommend to delete prior files before publishing to have a 'clean build' every publish. Click on the pencil next to 'false' for Delete existing files to open the following menu:

![Click on the pencil next to 'false' for Delete existing files to open the following menu](/General%20Development/Manual%20Application%20Deployment%20Images/open_publish_settings.png)


Make sure to expand "File Publish Options" and check off "Delete all existing files prior to publish" then click Save as shown below:

![Expand "File Publish Options" and check off "Delete all existing files prior to publish" then click Save](/General%20Development/Manual%20Application%20Deployment%20Images/select_publish_settings.png)

When you are finished configuring the publishing settings, click "Publish" and your files will be published to the specified directory, as shown below:

![click "Publish" and your files will be published to the specified directory](/General%20Development/Manual%20Application%20Deployment%20Images/publish_website.png)

After publishing, I recommend zipping the files to make the transfer process easier, as a zipped file will need one checksum versus the many thousand that can happen from an unzipped published website.

## First Time Deployment

### Transfer Website
Open file explorer on your destination web server and navigate to 'H:'. Your file you zipped should be contained within this directory, provided it was copied over or published and zipped within the H drive.

Copy the zip file over to a temp directory or your desktop and make a note of where you leave it. You will then want to extract all of the files.

After all of your files are extracted, you will want to create a directory within the www location for your website on the application server, for example on PDAPPDEVAPP15-C it is 'D:\www\'. You will want something looking like what is below:

![Copy Publish contents to wwwroot directory in application folder](/General%20Development/Manual%20Application%20Deployment%20Images/d_www_application.png)

### Create Server Hosting Certificate
Open IIS Manager and navigate to the node below Start Page on the left-hand side. Afterwards, select and open the Server Certificates module for your server by double-clicking it, as shown below:

![Select and open the Server Certificates module for your server by double-clicking it](/General%20Development/Manual%20Application%20Deployment%20Images/server_certificates.png)

Create a self-signed certificate using the server certificates module as shown below:

![Create a self-signed certificate by clicking the button by the same name on the right-hand side](/General%20Development/Manual%20Application%20Deployment%20Images/create_self_signed.png)

Name the certificate after your application in the first field and select 'Web Hosting' from the drop-down list as shown below:

![Name the certificate after your applicaton in the first field and select 'Web Hosting' from the drop-down list](/General%20Development/Manual%20Application%20Deployment%20Images/name_self_signed.png)

After clicking 'OK' you should have your certificate and be able to proceed to the next step.

### Create Website in IIS
Using the nodes on the left-hand side in IIS, navigate to the Sites node and right-click it. Select 'Add Website...' as shown below:
![Using the nodes on the left-hand side in IIS, navigate to the Sites node and right-click it. Select 'Add Website...'](/General%20Development/Manual%20Application%20Deployment%20Images/add_website.png)

Fill out the fields using as below:
1. your application name
2. the location of the files you moved earlier
3. https protocol as your protocol type
4. a port that is not currently in use by other applications and is ephemeral
5. the cerficate you created in the last step.

![Fill as specified above](/General%20Development/Manual%20Application%20Deployment%20Images/add_website_entry.png)

Click OK when finished and you will now have a website.

### Set up permissions
Navigate to the root folder of your application and right-click it and select Properties as shown below:

![Navigate to the root folder of your application and right-click it and select Properties](/General%20Development/Manual%20Application%20Deployment%20Images/folder_properties.png)

Select the Security tab and then click Advanced to open the advanced security menu as shown below:

![Select the Security tab and then click Advanced to open the advanced security menu](/General%20Development/Manual%20Application%20Deployment%20Images/properties.png)

As the advanced security screen opens, click Add at the bottom to proceed to the next step as shown below:

![As the advanced security screen opens, click Add at the bottom to proceed to the next step](/General%20Development/Manual%20Application%20Deployment%20Images/advanced_security-1.png)

As the next pane opens you will need to add a principal, click the named button as below:

![add a principal](/General%20Development/Manual%20Application%20Deployment%20Images/add_principal.png)

In new principal screen, select the web server that you are adding the web site to in the 'From this location:' field using the Locations... button. It will be at the top usually in the pop-out window. Afterwards, fill in the object name as 'IIS AppPool\APPLICATION_NAME_HERE' and click Check Names. Provided you followed the previous steps correctly, it should appear as an object with just your application name. Click OK when you are finished as shown below:

![Adding a principal](/General%20Development/Manual%20Application%20Deployment%20Images/new_principal.png)

Now that you have your principal object, give it the Modify attribute by clicking the checkbox as shown below and hit OK:

![added principal](/General%20Development/Manual%20Application%20Deployment%20Images/added_principal.png)

Now apply the new security rules by clicking the Apply button and hitting OK as shown below:

![return to advanced security screen](/General%20Development/Manual%20Application%20Deployment%20Images/advanced-security-2.png)

After the final step, you should have all folder permissions required to proceed.

### Configure Website in IIS
Navigate to the website that represents your application in IIS on the left hand side and open the feature list for it using the Connections pane on the left-hand side. Select the 'Authentication' feature and open as shown below: 

![Website features](/General%20Development/Manual%20Application%20Deployment%20Images/website_features-1.png)

As you see the next screen you will want to select Anonymous authentication and edit that attribute using the 'Edit...' button on the right-hand side. You will then want to assign the Application pool identity to Anonymous authentication as shown below:

![assigning application pool to anonymous browsing permissions](/General%20Development/Manual%20Application%20Deployment%20Images/authentication_feature.png)

Once you are finished configuring the website with any other additional configurations, you will be ready to start the site as shown below:

![start website](/General%20Development/Manual%20Application%20Deployment%20Images/start_website.png)

### Verification of Website Deployment
Using your own desktop that you are remoted into on the DIN network, you will open Google Chrome and navigate to the following URL, customized based on what server you are connecting to and the ephemeral port you assigned in a previous step:

**https://your-application-server.app.pddev.net:your-ephemeral-port**

Provided you followed the previous steps correctly and applied server-specific configurations correctly, you should be greeted by the splash page of the application you have been trying to deploy. ***Congratulations***

## Re-deploy Application

### Turn off Website
Open IIS Application and using the Connections pane on the left-hand side, navigate to Sites to see all of the Websites being hosted. Search for the website that you plan on re-deploying and click the 'Stop' button as shown below:

![sites on the web server](/General%20Development/Manual%20Application%20Deployment%20Images/manage_sites-1.png)

### Transfer Website
Open file explorer on your destination web server and navigate to 'H:'. Your file you zipped should be contained within this directory, provided it was copied over or published and zipped within the H drive.

Copy the zip file over to a temp directory or your desktop and make a note of where you leave it. You will then want to extract all of the files.

After all of your files are extracted, you will want to clear the directory within the www location for your website on the application server. Select all files and folders that are not logs folders or application configuration files, for example appsettings.config, web.config, connection.config, log4net.config, etc. and delete them.or example on PDAPPDEVAPP15-C it is 'D:\www\'. Afterwards, you will move all non-configuration files into your web application folder from the folder that you just extracted. You will want something looking like what is below:

![Copy Publish contents to wwwroot directory in application folder](/General%20Development/Manual%20Application%20Deployment%20Images/d_www_application.png)

### Turn on application
Open up IIS again, and navigate to the Sites node using the Connections pane on the left-hand side. Search for the application you plan on bringing back up and click the 'Start' button as shown below:

![manage sites](/General%20Development/Manual%20Application%20Deployment%20Images/manage_sites-2.png)

### Verification of Website Deployment
Using your own desktop that you are remoted into on the DIN network, you will open Google Chrome and navigate to the following URL, customized based on what server you are connecting to and the ephemeral port you assigned in a previous step:

**https://your-application-server.app.pddev.net:your-ephemeral-port**

Provided you followed the previous steps correctly and applied server-specific configurations correctly, you should be greeted by the splash page of the application you have been trying to deploy. ***Congratulations***