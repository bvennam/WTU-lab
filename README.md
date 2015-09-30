# Lab I097
# Bluemix and Hybrid Cloud Integration: Building Your First Microservices App

### Step 0: Understand SmartBooks Architecture

In this lab you’ll be playing two different roles, an API Provider sharing their APIs with the world & an API Consumer who would like to use those shared APIs in their applications.  We’ll present this information in-person, so you can skip to Step 1 if you understood everything so far!#### API Provider: APIs Inc1. The API Provider has provided an Interests API.  This API takes as input a Twitter handle & outputs an array of interests.  See the Twitter Interest API documentation for more details.2. The API Provider wants to implement rate limits & interesting analytics around the APIs they own, so they decide to use API Management on Bluemix.  They can now share their API on Bluemix to API consumers like Bluemix Smartbooks.#### API Consumer: Bluemix Smartbooks1. Bluemix SmartBooks is an e-commerce application based on the microservices architecture design pattern.  The concept of microservices is simple – an application is broken into multiple independent applications, where each application has a clear purpose.  The applications talk to each other using HTTP over REST or a message queue. Each of these applications can be written in whatever language is best for its job.  Our application is based on the previous e-commerce sample, found here. For a more in-depth overview, check out the previous blog.  The SmartBooks application is divided into an orders service, a catalog service, and a UI layer to tie it all together.	1. A backend RESTful API to track all store orders. This JavaEE application stores orders in a persistence entity that can be used with an internet-accessible database or in conjunction with the Bluemix Secure Gateway service to securely access databases inside an enterprise.	2. The catalog service is an API to track all of the items available in the store.  This is written in Node.js with Express.  The catalog of items is persisted in a Cloudant database.	3. The UI application is a simple UI layer displaying all items in the store catalog, creating orders, and invoking the Interests API mentioned below.  This UI will call all three APIs, and is written in PHP.2. After the store application was written, the developers wanted to personalize the catalog suggestions3. Their API Provider business partner had provided a few different APIs, and they saw the interests API. Seemed like a perfect fit for personalizing the catalog, so they decided to pull it into their application.4. This interests API was provided through API Management, enabling the API provider to provide rate plans, get analytics on their API usage, as well as share their APIs to Bluemix.

## Part 1: You are the API Provider, APIs Inc., and you have an interests API that you would like to set up some analytics and management for.  You plan to use IBM’s API Maanagement service on Bluemix.

### Step 1: As the API Provider (APIs Inc), go check out your already created interests API.
1. Go to [Interests API] (http://interestsapi.mybluemix.net/api) to see your interests API & test it out in the swagger documentation.2. Enter a twitter handle to see a list of interests for that twitter handle.![picture alt](https://github.com/beemarie/WTU-lab/blob/master/images/swagger_doc_interests.png "interests API")
3. This is great, but there’s no analytics or rate limiting around this API!  Luckily, we can use Bluemix’s API Management solution for the API we just created.

### Step 2: As the API Provider, Provision an instance of IBM’s API Manager service on Bluemix.
1. Open the Bluemix console and log in.2. Navigate to the catalog.![picture alt](http://www.brightlightpictures.com/assets/images/portfolio/thethaw_header.jpg “interests API”)3. Select the Integration filter from the left-hand menu to reduce the number of displayed options, then locate and click the API Management tile. The service information page will appear.
4. Click the Create button to provision and launch the API Management service.5. The Getting started page will appear.   Click GO TO API MANAGER.

### Step 3: Import the Interests API swaggerThe API Manager allows users to create APIs from scratch or import an existing API based on a Swagger or WSDL definition. For the purposes of this tutorial, we’ll be importing the Pet Store sample API via swagger.io.
1. Click the Import APIs, or compose a new one link from the Getting started page in Bluemix. The API Manager will open in a new tab and may take a moment to load.2. Click the blue +API button, then select Import Swagger.3. Next, we can upload a Swagger document or reference one via a URL. For tutorial purposes, we’ll simply provide the URL to import. Paste the following URL into the Swagger URL field: https://raw.githubusercontent.com/mhamann/apim-sample/master/petstore.json. Leave the Username and Password fields blank, then click Load.4. API Manager will list all of the APIs and resources contained in the Swagger document. Click Add to create the API.
5. The new API is now displayed in the API list. Click the API title to open the API editor. The API editor allows the modification of various API attributes. We can change the name, description, path, add and remove resources, and adjust options like authentication and authorization.6. The API has been created successfully.

### Step 4: Create a new plan for this APIEvery API must be part of a plan before it can be published and invoked. API Management uses plans to manage access to API resources, set rate limits, and stage APIs into various environments (e.g. a sandbox, test, production, etc). One plan may contain resources from any number of APIs, thereby enabling access to groupings of resources at varying rate limits. Later on, we’ll publish these plans for usage in Bluemix.  In this step, we’ll create a new plan, add our API resources to it, set rate limits, and deploy it.

1. Open the Plans page by clicking the plans icon in the API Manager or by returning to the getting started page and selecting the next step.2. Click the + Plan button.3. Enter a name for the plan and click Add.4.The new plan will appear in the list. Click the plan’s title to open the plan editor. 5. First, add the resources from the Pet Store API to the plan. Click the + Resource button.6. The list of APIs will appear on the left and the API’s resources on the right. If not already selected, pick the Interests API, then select all of the resources that are part of the API. Then, click Add.  The selected resources will appear in the lower portion of the plan editor.
7. Next, set a rate limit for the plan. Rate limits may be set either individually for each resource -or- for the entire plan. If a rate limit is applied at the plan-level, it is applied to all of the resources in that plan.Set the rate limit for the plan by clicking the edit icon under the Rate limit section. Set the rate limit to: 10,000 requests per 1 Days. Then, click Apply. 
8. Click Save in the upper-right corner of the page to persist the changes to the plan.
9. Finally, deploy the plan.Click the Deploy icon to display the deployment menu. Wait until the Sandbox option appears, then select it.  After a few moments, a success notification should appear. At this point, the plan has been deployed
### Step 5: Publish the API to Bluemix1. Open the Management page by clicking the management icon in API Manager, or by returning to the getting started page and selecting the final step.  The list of deployed plans will be displayed.
2. Click the Pet Store – Gold plan to expand the plan and display the list of currently deployed versions.3. Click the icon to launch the plan publishing wizard:  4. Ensure Publish this version is selected, then click Next.
5. Ensure Select group of developer organizations and communities is selected, then enter or select Bluemix in the field below. If you invited another Bluemix organization back in step #4, you can also enter their organization name here to make the API available to them as well. (Note: if the entry field isn’t showing up right away, try clicking the blue circle next to the option “Select group of developer organizations and communities” even if it’s already selected.)
6. Click Publish. A notification should appear after a few moments indicating that the publish was successful.  You should be able to see your API appear in your Bluemix Catalog under “Custom APIs.”  This API could be shared with any other Bluemix org so that your API Consumer can access it.  For this lab, in the interest of time, we’ll skip sharing the API & just keep the provider & the consumer in the same organization. ### Step 6: Bind the API to an AppWe’ve successfully created our API in API Manager and published it to Bluemix. It will now show up in our organization’s Bluemix catalog. The final step in the process is to provision the API, bind it to an app, and watch the data flow!For the purposes of this tutorial, we’ll create a new Node.js app and then provision and bind our API to it. (Any application type— not just Node.js —may be used to consume this API.)1. Open the Bluemix dashboard and click Create an app.
2. Click Web.
3. Click SDK for Node.js, then click the Continue button. Give the app a name, then click Finish. (This name should be unique, so be creative or just use your initials.)
4. Once the app has been created, click the Overview section.
5. Next, click Add a service or API. The Bluemix catalog will appear. Select the filter for Custom APIs on the left-hand sidebar, then click the Swagger Petstore tile to open the API details. Details about the API will be displayed, including all of the resources it provides, parameters, sample responses, and so on.
6. Notice, the app we created is already selected, so simply press the Create button to provision the API and bind it to the application. If you are asked to restage the application, click Restage. The Swagger Petstore is now bound to the application. We need only to test it and ensure that it works.
7. Finally, let’s obtain the APIs credentials and test it out. On the left, click Environment Variables.
The variables for VCAP_SERVICES should be displayed, which include the details for our Swagger Petstore API. Primarily of interest are the credentials, which contain the Client ID, Client Secret, and URL we need in order to invoke the API.
## Part 2: You are the API Consumer (Smartbooks), and you want to set up an instance of the Smartbooks Application, which will consume the Interests API provided by APIs Inc.
### Step 1: As the API Consumer (Smartbooks), set up your own instance of the Smartbooks Application.1. The source is provided as a set of three separate applications: A catalog API app, an orders API app, and a UI application.  You can check out, clone, and play around with the code for each of these individual pieces from their git repositories on IBM DevOps Services.2. Deploy Catalog API by clicking the Deploy to Bluemix button below.  
[![Deploy to Bluemix](https://bluemix.net/deploy/button.png)](https://bluemix.net/deploy?repository=https%3A%2F%2Fhub.jazz.net%2Fgit%2Fintegration%2Fbluemixhybrid-smartbooks-catalog-api&amp;cm_mmc=developerWorks-_-dWdevcenter-_-bluemix-_-lp)3. Click View Your App and Note the route of this deployed application for later use: http://someCatalogRoute.mybluemix.net4. Deploy Orders API by clicking the Deploy to Bluemix button below.  [![Deploy to Bluemix](https://bluemix.net/deploy/button.png)](https://bluemix.net/deploy?repository=https%3A%2F%2Fhub.jazz.net%2Fgit%2Fintegration%2Fbluemixhybrid-smartbooks-orders-api&amp;cm_mmc=developerWorks-_-dWdevcenter-_-bluemix-_-lp)5. Note the route of this deployed application: http://someOrdersRoute.mybluemix.net6. Deploy UI application by clicking the Deploy to Bluemix button below.  
[![Deploy to Bluemix](https://bluemix.net/deploy/button.png)](https://bluemix.net/deploy?repository=https%3A%2F%2Fhub.jazz.net%2Fgit%2Fintegration%2Fbluemixhybrid-smartbooks-ui&amp;cm_mmc=developerWorks-_-dWdevcenter-_-bluemix-_-lp)
### Step 2: After deploying, You will also need to edit a few of values in the UI application.1. In index.php change the variable $catalogRoute to the route of your catalog API.2. In submitTwitter.php, change the variable for $twitterAPIURL to reflect the URL you were given from APIm, as well as the client id & secret.  All of this information can be found in the VCAP_SERVICES for the Node.js app that your interests API is associated to from the API Provider instructions.3. In submitOrders.php, change the variable $ordersRoute to point to the orders route of your orders API.4. Once these values are added, check in your changes by clicking the git icon in the left side bar.
5. You will need to commit and then push your changes.  6. After this is done, you can now Build and Deploy the new version of your app by selecting the Build and Deploy button in the upper right corner.
7. At this point, your SmartBooks application should run, with suggestions providing books from the Google Books API!  We will leave the storage of data to an on-premises database as an extension to this lab, with instructions found below.  8. Go to the URL of your smartbooks application to try out the app!9. As the API Provider, you can go check APIm to see analytics information on API Usage!Optional extension•	[SKIP FOR NOW, left as an extension] The Orders API can be connected to any JDBC compatible database you want to store your order records in.  To configure a database connection, you will again need to edit the code using the same process as with the Twitter API.•	[SKIP FOR NOW, left as an extension] In the file tree on the left, navigate to /src/META-INF/persistence.xml and modify the <properties> list with either the direct database connection parameters or the host and port information of a Bluemix Secure Gateway destination that points to your database. •	Commit and push the persistence changes to git, and then redeploy the application to start using the new database connection.