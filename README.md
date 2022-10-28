# CVDS-Lab6
**Nombre:** Sebastian Rojas Bueno

## Prerequisites
* An Azure account (free from https://azure.com/free).
* Complete task 1 from the [prerequisite](https://azuredevopslabs.com/labs/azuredevops/prereq/) instructions.

## Exercise 1: Embracing Continuous Delivery with Azure DevOps
### Task 1: Setting up Azure resources
1. Start off by creating the Azure resources needed for this lab. This includes a database and two app services: one for QA and one for production. Log into your account at https://portal.azure.com.
2. Click Create a resource and search for “SQL Database”. Select SQL Database.

3. Click Create.

4. Under Resource group, click Create new.

5. Enter a Name of “partsunlimited” and click OK.

   ![image](https://user-images.githubusercontent.com/62759668/197402558-135403c0-a575-479b-8eaf-e6d20b349a53.png)

6. Enter a Database name of “partsunlimited” and click Create new to create a new server.

7. Enter a unique name for Server name, such as by including your name. Enter an admin username and password you can remember. Note that “P2ssw0rd” meets the password requirements. Click OK to confirm these options.
   
   ![image](https://user-images.githubusercontent.com/62759668/197402479-8950264e-172b-49cd-88b7-01a7825a57ce.png)

8. In Compute + storage select Configure Database. Select Basic service tier and click Apply.
    
   ![image](https://user-images.githubusercontent.com/62759668/197402676-72016c15-b241-4824-964a-6e02264cee04.png)

9. Click Review + create.

10. Click Create. It’ll take some time to complete, but you can move on to the next step while it works in the background.
   
    ![image](https://user-images.githubusercontent.com/62759668/197402707-f8055f72-d88a-44f7-b0b8-f8bbc71a36bb.png)
    
    ![image](https://user-images.githubusercontent.com/62759668/197403043-f4649db3-d514-4f8f-bb07-34ad2af8657a.png)


11. From Azure Portal home page search for “App Services” and select App Services.

12. Click Create in App Services page.

13. Under Project Details, select the same Subscription and Resource Group used for the database.

14. For Name, enter a unique name, such as by using your name as part. Since this will be for our QA deployment, append the name with “-qa”. Select the ASP.NET 4.8 Runtime stack.
    
    ![image](https://user-images.githubusercontent.com/62759668/197403285-57c1cc7d-a0ab-4348-a4d9-ae4f1e857b78.png)

15. On App Service plan select create new and name it plan. Change the size to S1 (under Production).

16. Click Review and create.

17. Click Create.
    
    ![image](https://user-images.githubusercontent.com/62759668/197404246-08d5c355-4369-4052-ae5c-380322bbbfa2.png)

18. Repeat the process above to create a second app service for the production stage. This time, append the Name with “-prod” instead. You can use the same App Service Plan created before for QA.
    
    ![image](https://user-images.githubusercontent.com/62759668/197404760-b31f38e7-fd05-4fc8-86f2-7638f058712a.png)
    
19. While the Azure resources are created, go ahead and configure the SQL server.
20. Click the Resource groups tab from the left menu. Locate and click the partsunlimited group.
21. Click the SQL server resource created earlier.
22. Select the Firewalls and virtual networks tab from the Security section.
23. Set “Allow Azure services and resources to access this server” to Yes to allow the App Services to access this SQL server.
    
    ![image](https://user-images.githubusercontent.com/62759668/197406544-e00157db-9ef5-4d3d-bd2c-66074afe6207.png)
    
24. Click Save on the top of the tab.
25. Continue on to the next task. Leave this browser tab open for later.
### Task 2: Creating a continuous release to the QA stage
1. Navigate to your team project on Azure DevOps in a new browser tab. It should be at https://dev.azure.com/YOURACCOUNT/Parts%20Unlimited.
2. Navigate to Pipelines | Releases.

3. Delete the existing PartsUnlimitedE2E release pipeline. We’ll start fresh here.

4. Click New pipeline.

5. There are many starting templates to choose from, or you can even begin with an empty process template. In this case, select the Azure App Service Deployment and click Apply.

6. Rename the default stage to “QA”. This template will deploy to QA, and then to a production stage. We’ll set up this one first.

7. Rename the release pipeline to “PUL-CICD”.
   
   ![image](https://user-images.githubusercontent.com/62759668/197409579-85dad5ba-8a54-4636-9586-cfc6785359f7.png)

8. The first thing to define is exactly what should be deployed. Click Add in the Artifacts section to specify the artifact to deploy.

9. There are many types of artifacts, but this one will be pretty simple: a project built from the PartsUnlimitedE2E build pipeline that already exists in this team project. Click Add.
   
   ![image](https://user-images.githubusercontent.com/62759668/197409645-dfc5f138-2b4a-4dff-a5a8-1948461a45ff.png)

10. Now that the artifact has been defined, it’s time to configure the deployment to QA. Click 1 job, 1 task in the QA stage.

    ![image](https://user-images.githubusercontent.com/62759668/197409750-51cfb4b7-15f7-4d65-9333-0f8a133715dd.png)

11. Select the Azure subscription you used earlier to create the resources and click Authorize. If you need to create a connection to an Azure account associated with a different Microsoft account, click New and follow that workflow before continuing.

12. Follow the workflow to authorize access to your Azure account.
    
    ![image](https://user-images.githubusercontent.com/62759668/197409811-d59bff91-14d6-4b0c-bafa-5ca164c89858.png)

13. Enter the App service name used earlier when creating the QA app service.
    
    ![image](https://user-images.githubusercontent.com/62759668/197409842-652f9559-c094-4408-a34d-e5f2da9833c7.png)

14. Return to the Pipeline tab.

15. Click the Triggers button to define what triggers will invoke this deployment.

16. Enable the Continuous deployment trigger. Add a Build branch filter that points at the The build pipeline’s default branch. This will kick off the deployment when the build completes.
    
    ![image](https://user-images.githubusercontent.com/62759668/197409957-f0b1889d-fa90-4d8c-96ac-e86e4225ac3a.png)

17. Save the release pipeline.

### Task 3: Configuring the Azure app services
1. Return to the browser tab open to the Azure portal.
2. Click the Resource groups tab from the left menu. Locate and click the partsunlimited group created earlier.

   ![image](https://user-images.githubusercontent.com/62759668/197906267-9a494fd4-be73-4c26-8097-c9801e229797.png)

3. Click your SQL database (something like pul-johndoe/partsunlimited). Make sure you click the database you created and not the server. Note that it may take a few minutes for the database and server to become available, so click the Refresh button every once in a while to check in.
   
   ![image](https://user-images.githubusercontent.com/62759668/197906398-984c2b78-351a-4f47-a872-ae8b7c4507a8.png)

4. In the new blade, click Show database connection strings.

   ![image](https://user-images.githubusercontent.com/62759668/197906990-21a04125-d096-47e3-b209-e23d4db8441a.png)

5. This will provide you with a list of connection strings based on platform. Copy the ADO.NET string to your clipboard so you can configure your new web site to use it. Close this blade.

6. Open a new instance of Notepad and paste the connection string into it. This will make it easier to edit and retrieve later on in case anything happens to the clipboard copy.

7. Use the breadcrumb navigation to return to the partsunlimited resource group.

8. Click the QA app service created earlier.

9. Select the Configuration tab from the Settings section.

10. On this blade you can configure settings for your app, such as connection strings. Click New connection string.

11. Locate the Connection strings section and add a new entry with the key “DefaultConnectionString” and the value pasted from the clipboard. You’ll need to locate the “{your_username}” and “{your_password}” sections and replace them (including braces) with the actual SQL credentials entered earlier. Be sure the Type is set to SQLAzure and click OK.

12. Click Save to commit.

    ![image](https://user-images.githubusercontent.com/62759668/197909122-1340c7f3-2a0a-4f37-8b5d-5ac256792e0b.png)

13. Repeat the process above to add the same connection string to the production app service.

    ![image](https://user-images.githubusercontent.com/62759668/197909289-818dc754-1fbe-4620-baac-b99256efcfcb.png)

### Task 4: Invoking a continuous delivery release to QA
1. Return to the browser tab open to your Azure DevOps project.

   https://aex.dev.azure.com/
   https://dev.azure.com/
   
3. Now that the release pipeline is in place, it’s time to commit a change in order to invoke a build and release. You’ll need to make a few changes like this over the course of this lab, so it’s recommended that you use a separate tab for Code | Files to keep that part of the process separate.

   ![image](https://user-images.githubusercontent.com/62759668/197911313-e74c3677-06bd-4206-88df-0e581b9729e7.png)

3. Navigate to PartsUnlimited-aspnet45/src/PartsUnlimitedWebsite/Views/Shared/_Layout.cshtml. This is a file that defines the general layout of the site and is a good place to make a change that will be easily visible after deployment.

4. Click Edit to edit the file inline.

5. Locate the Parts Unlimited Logo and add the text “v2.0” after it. This will be an easy thing to check for after deployment.

   ![image](https://user-images.githubusercontent.com/62759668/197911538-e374fb99-193c-4015-8443-7c3bee4b7f1e.png)

6. Commit the changes back to the repo. This will kick off a build based on the preconfigured build pipeline.

7. Navigate to the Pipelines hub.

8. Open the most recent build for PartsUnlimitedE2E. It may be queued, in progress, or already completed.

9. Follow the build until completion.

   ![image](https://user-images.githubusercontent.com/62759668/198413845-f52a6103-b125-4f40-95d9-834b70aae066.png)

10. Click the Releases tab to see the new release get going.

11. Click the release. As with the build, it may be queued, in progress, or already completed when you get here.

12. Follow the release until it succeeds.

    ![image](https://user-images.githubusercontent.com/62759668/198413966-d660f29b-361c-442f-9273-059447d7d2d1.png)

13. You can now close this tab if you prefer.

14. In a new browser tab, navigate to the QA site. It will be the name of your app service plus “.azurewebsites.net”. It should show the v2.0 added earlier.

    En este punto nos enfrentamos a este error
    ![image](https://user-images.githubusercontent.com/62759668/198423531-ba4a1972-32c1-410a-a59c-bff0212a20ef.png) \
    para solucionarlo en nos dirigimos a Git en azure, src, y encontramos el archivo `Web.config`, ya aca modificamos la siguiente linea de la siguiente manera sustituyendo su respectiva contrasena
    ```
    <add name="DefaultConnectionString" connectionString="Server=tcp:pul-sebas.database.windows.net,1433;Initial Catalog=partsunlimited;Persist Security Info=False;User ID=sysadmin;Password='Tian0369*';MultipleActiveResultSets=False;Encrypt=True;TrustServerCertificate=False;Connection Timeout=30;" providerName="System.Data.SqlClient" />
    ```
    ![image](https://user-images.githubusercontent.com/62759668/198424245-834106c7-5119-404c-a5fb-ff0f1cde9d72.png) \
    Procediendo a eliminar la base de datos \
    ![image](https://user-images.githubusercontent.com/62759668/198424926-1e94b345-4d6c-4cbd-8a24-17904de1861d.png) \
    Para crearla de nuevo \
    ![image](https://user-images.githubusercontent.com/62759668/198425273-2111b8e2-cabb-4956-8248-6e384f642066.png) \
    Con sus debidas propiedades, aparece en rojo porque ya tengo mi base creada \
    ![image](https://user-images.githubusercontent.com/62759668/198425928-ead73788-fbcd-4dad-bda2-e0e8f41795e7.png)
    Una vez creada, corremos un nuevo release \
    ![image](https://user-images.githubusercontent.com/62759668/198426521-fa305904-196a-4e53-b894-c0dbbe412730.png) \
    Recargamos la pagina obteniendo el resultado esperado \
    ![image](https://user-images.githubusercontent.com/62759668/198426790-4f5efc0d-25aa-4ac7-97d9-7ac14f02cde7.png)
### Task 5: Creating a gated release to the production stage
1. Return to the Azure DevOps browser tab.
2. Click on Pipelines>Pipelines and click on Edit (top-right) to modify the release definition.
3. As release pipelines get more sophisticated, it becomes important to define gates to ensure quality throughout the release pipeline. Since the next stage we’re deploying to is production, we’ll need to be sure to include both automated quality gates as well as a manual approver gate. Return to the release pipeline browser tab and click Clone in the QA stage. Since the production stage is virtually the same, we can reuse almost all of the existing configuration.

   ![image](https://user-images.githubusercontent.com/62759668/198437384-5cee2d0e-cd33-4fb4-b59b-6bda10b66edf.png)
   ![image](https://user-images.githubusercontent.com/62759668/198438429-a14131d1-ca34-4607-8ccc-43c9c3d292de.png)
   ![image](https://user-images.githubusercontent.com/62759668/198450975-a00cba92-5c98-4a8b-8d2c-7691df64addb.png)
   ![image](https://user-images.githubusercontent.com/62759668/198451301-32b15fc2-33bb-4e97-999e-9b62c25bfe9d.png)

4. The new stage is added after the current one, which is what we want. However, before we can consider the QA deployment successful, we’ll need to define a post-deployment condition. Click the Post-deployment conditions button on the QA stage.

5. Enable the Gates option.

   ![image](https://user-images.githubusercontent.com/62759668/198451554-7ef11194-9909-4415-a53e-893a0afec6af.png)

6. There are several kinds of gates available that can automatically test virtually anything you need to make sure a deployment is in good shape. These could be the return values of Azure functions or REST APIs, queries to Azure for alerts, or work item queries in Azure DevOps. You can also configure how long the platform should delay before evaluating the gates for the first time. In this case, change that to 0 so it will test them immediately after deployment. Then click Add | Query Work Items.

   ![image](https://user-images.githubusercontent.com/62759668/198452227-56d8a935-5763-418c-aad2-ab15d521616e.png)
   ![image](https://user-images.githubusercontent.com/62759668/198457663-3a17bfb6-d6dd-4677-8e6a-fbd4f6c3181b.png)

8. Select the Query for Shared Queries | Critical Bugs. We will make it our policy that the QA deployment cannot be considered a success until all critical bugs have been resolved.

   ![image](https://user-images.githubusercontent.com/62759668/198452648-97aeec2a-bc7f-4fb9-87c4-bec3067c1394.png)

8. Expand Evaluation options and update the Time between re-evaluation of gates to 5. If this gate fails, we want it to reevaluate the query every 5 minutes until it clears because engineers will need some time to confirm those critical bugs are fixed in the current version. However, if those bugs aren’t cleared and the release isn’t manually failed, this configuration will automatically fail the gate after 1 day.

   ![image](https://user-images.githubusercontent.com/62759668/198453352-7f2433c8-b679-4a3d-a7e5-cde224f4c9bc.png)

9. For the Query Gate to work, Project Build Service would require Read permission to queries. Go to Azure Boards > Queries > All > Shared Queries > “…” > Security.

10. Search for the “(YOUR PROJECT NAME) Build Service “, if not included by default (not Project Collection Build Service!) and give it Read > Allow permission.

11. We can now turn our focus to the production stage. Select the Copy of QA stage.

12. Rename it to “Prod”.

13. Click its Pre-deployment conditions button.

14. Enable the Pre-deployment approvals and add yourself as an Approver. The idea here is that you won’t be asked to approve the production deployment until after the QA deployment has succeeded. At that point, someone on this list will need to approve the deployment to production. Also clear the box for User requesting a release of deployment should not approve if it’s checked. For the purposes of this lab, you can approve releases you have requested.

    ![image](https://user-images.githubusercontent.com/62759668/198461611-4213a0ed-885f-4c8e-96dd-b1d21f81361d.png)

15. Click the Prod stage’s 1 job, 1 task.

    ![image](https://user-images.githubusercontent.com/62759668/198462708-c36f20d1-fb55-4fa1-bd70-3510985f8edc.png)

16. Update the App service name to reflect the “-prod” (instead of “-qa”).

17. Save the release pipeline.

18. Repeat the process for changing the codebase at “PartsUnlimited-aspnet45/src/PartsUnlimitedWebsite/Views/Shared/_Layout.cshtml” followed earlier in a new tab. This time, update the version number from “2.0” to “3.0”. This will invoke the release pipeline.
19. As before, follow the release until it is deploying to QA. Click to follow the release itself once available.
20. Eventually there will be an issue with the QA deployment. While it deployed out successfully, one of the quality gates failed. This will need to be resolved for that stage to be approved. Click the View post-deployment gates button.

    ![image](https://user-images.githubusercontent.com/62759668/198471998-078526d3-2246-4736-9320-f5160f179494.png)

21. It looks like the Query Work Items gate is failing. This means that there is a critical bug that must be cleared.

22. Open a new tab to Boards | Queries to locate the bug.

23. Use the query list on the All tab to open the Shared Queries | Critical Bugs query.

    ![image](https://user-images.githubusercontent.com/62759668/198472986-baf54918-789d-4c26-b59d-0e5eafbaa025.png)

24. Open the one bug by clicking it.

    ![image](https://user-images.githubusercontent.com/62759668/198473224-77a521b5-ebd7-4d18-a021-5bcb35199932.png)

25. Ordinarily you’d check the site to confirm the bug was fixed, but we’ll skip ahead here and mark it Done. Click Save and close the tab.

    ![image](https://user-images.githubusercontent.com/62759668/198473621-209eed14-8739-46cd-a12d-65713bcd6dc6.png)

26. Return to the release pipeline tab. Depending on timing, it may take up to five minutes for Azure DevOps to check the query again. Once it does, the bugs will be cleared and the release will be approved. You can then click Approve to approve deployment to production.

27. Confirm the deployment.

28. Click the In progress link to follow the release workflow.

29. It may take a moment for the release to kick off and complete.

30. Once the release has completed, open a new tab to the production app service URL. It should be the same as your QA URL, but with “-prod” instead of “-qa”. Note the v3.0.

### Task 6: Working with deployment slots
1. Return to the browser window open to the Azure portal.
2. Azure App Services offer deployment slots, which are parallel targets for application deployment. The most common scenario for using a deployment slot is to have a staging stage for your application to run against productions services, but without replacing the current production application. If the staging deployment passes review, it can immediately be “swapped” in as the production slot with the click of a button. As an additional benefit, the swap can be quickly reversed in the event an issue is uncovered with the new build. Locate the resource group created earlier and click the prod app service.

3. Select the Deployment slots tab. If you see a message indicating that your current pricing tier does not support slots, follow the workflow to upgrade this service to a Production tier or higher and then return here. You may need to refresh the browser for the Add Slot option to become enabled.

4. Click Add Slot. Note that the production slot is considered a “default” slot and is not shown as a separate slot in the user experience.

5. Enter a Name of “staging” and select the Configuration Source that matched your existing deployment (there should be only one). Click Add to create the slot.

6. Return to the Azure DevOps tab with the Prod stage pipeline editor.

8. Select the Deploy Azure App Service task.

8. Check Deploy to Slot… and set the Resource group and Slot to those created earlier.

9. Save the release pipeline.

10. Follow the workflow from earlier to commit a change to the codebase at “PartsUnlimited-aspnet45/src/PartsUnlimitedWebsite/Views/Shared/_Layout.cshtml” by updating the layout template from “3.0” to “4.0”.
11. Follow the release pipeline through deployment and approve the release to production when requested.
12. When the production deployment has completed, refresh that browser tab. Note that there shouldn’t be any change since the deployment was pushed to a different slot.

13. Open a new tab to the staging slot. This will be the same as your production URL, but with “-staging” appended to the app service name within the domain. This should reflect the new v4.0.

14. Return to the browser window open to the Azure portal. Click Swap in the deployment slots blade.

15. The default options here are exactly what we want: to swap the production and staging slots. Click Swap. Note that if your apps rely on slot-level configuration settings (such as connection strings or app settings marked “slot”), then the worker processes will be restarted. If you’re working under those circumstances and would like to warm up the app before the swap completes, you can select the Swap with preview swap type.

16. Return to the prod browser window (not the staging slot) and refresh. It will now be the 4.0 version.

## Fuente
https://azuredevopslabs.com/labs/azuredevops/continuousdeployment/
