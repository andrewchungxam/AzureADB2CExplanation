# AzureADB2CExplanation
Azure AD B2C Explanation with Swift and Node.js

The purpose of this doc is to create a complementary “explainer” to this sample: https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal

In the end, you'll have an Swift access that has a SignUp/SignIn, Edit Profile, Logout, and the calling of a API.  The API will be a Node.js application that you'll run locally on your Mac. 

Please always consider check-in/update-dates when working through samples and examples - and try to work with the most up-to-date samples.  The document you are reading now was created in April 2019 (latest update: April 2019); though this explainer may get stale, many of the overviews and principles will still hold.

When you go through the sample, you’ll notice that there are places where you’ll need to add several of your own values.  There have been pre-baked values included in the sample which you’ll see below (and you can run the sample with just those values).

However, we’ll explain what these values mean below - so you get a handle on what they mean.

```
    let kTenantName = "fabrikamb2c.onmicrosoft.com" // Your tenant name
    let kClientID = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" // Your client ID from the portal when you created your application
    let kSignupOrSigninPolicy = "b2c_1_susi" // Your signup and sign-in policy you created in the portal
    let kEditProfilePolicy = "b2c_1_edit_profile" // Your edit policy you created in the portal
    let kGraphURI = "https://fabrikamb2chello.azurewebsites.net/hello" // This is your backend API that you've configured to accept your app's tokens
    let kScopes: [String] = ["https://fabrikamb2c.onmicrosoft.com/helloapi/demo.read"] // This is a scope that you've configured your backend API to look for.

    // DO NOT CHANGE - This is the format of OIDC Token and Authorization endpoints for Azure AD B2C.
    let kEndpoint = "https://login.microsoftonline.com/tfp/%@/%@“
```

Here are relevant parameters:

Azure AD B2C
Tenant
ClientId
SignupOrSigninPolicy
EditProfilePolicy

BackendService:
GraphURI
Scope

Let’s define them in the simplest terms:
Azure AD B2C: Tech that allows you to add/edit/delete users + and allows these users (controlled) access to your backend API.  
Tenant: tenant is the “folder” or “container” where you define an Azure AD B2C instance and where you define “applications”.  “Application” are either the web endpoints or mobile apps that you are trying to make available to certain users who authenticate.
ClientId: Applications are assigned an Id, this is called the ClientId.
SignupOrSigninPolicy/EditProfilePolicy: Policies define the types of interaction flows users will see/have access to.  (ie. allowing signing up for an account or allowing a user to edit an existing account.  Rather than coding any of this - common flows are defined as policies; policies can be slightly customized. You assign policies to an Application.

BackendService:
GraphURI: This would simply be a backend API - obviously this api will give your user access to useful data
Scope: Are sometimes called Permissions (in the world of OAuth2.0).  Backend services don’t need to do a binary “give access to everything” or “don’t have access to anything.”  Instead, they can define scopes which give access to a small slice of the full API.  As an example, let’s look at Microsoft Graph — 3 examples of scopes are “Calendars.Read”, “Calendars.ReadWrite”, “Mail.Send” (You can probably guess what each of these scopes give permission to do but of course, these scope definitions are defined by the creators of the backend services.)

What are Roles?  Is it supported in Azure AD B2C?
https://stackoverflow.com/questions/53602804/azure-active-directory-custom-roles-and-possible-scopes
There is no support today for custom roles in Azure Active Directory. Only the predefined Administrator Roles, as described in the documentation, are available for use.
https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-assign-admin-roles

What is MSAL?
MSAL is a developer library that helps you to obtain tokens from MSA, Azure AD or Azure B2C for accessing protected resources – such as your own API, Microsoft's API (such as the Microsoft Graph) and any other 3 rd party choosing to protect their API with Microsoft identity.  In OAuth2.0, transfer of Auth and Refresh tokens are core to how authenticated uses are permitted access to resources. Safely handling these tokens is hard, and by using the MSAL, you don’t have to do it, and the MSAL does it for you.

Running the sample:
The instructions are not the most clear on this point - but actually, the sample comes connected with a working Azure AD B2C. There are some minor steps/installations that you’ll need to do before you can get the sample running.

Note - I’m running my sample from a Mac + Visual Studio for Mac.  I’ll assume you have access to a Mac in some of your work as we are dealing with Swift/iOS in this sample.

In the sample there are some installation instructions here:
https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal/blob/master/README.md#if-youre-building-for-ios-tvos-or-watchos

Firstly, I ran there first command: 
brew install carthage  (it may update brew, and often you’ll have carthage already installed and so you’ll instead be asked to update carthage).

If you see the complaints - simply upgrade it:
brew upgrade carthage

Then:
carthage bootstrap

Then there are many instructions of dragging and dropping various frameworks - I found that these were already preset/done in the sample - let me include the screeshots so you can confirm you see the same:

Step 4 from these [instructions](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal/blob/master/README.md#if-youre-building-for-ios-tvos-or-watchos):  (If you’re not familiar play attention to the what has been highlighted (from left to right): The blue folder, the solution highlighted, the target (MSALiOSB2C), General (in blue), and then scroll down until you see “Linked Frameworks and Libraries”)



Step 5 from these [instructions](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal/blob/master/README.md#if-youre-building-for-ios-tvos-or-watchos):  (If you’re not familiar play attention to the what has been highlighted (from left to right): The blue folder, the solution highlighted, the target (MSALiOSB2C), Build Phases (in blue), and see the Run Script.



Notice this instruction [Step 1:](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal/blob/master/README.md#configure-your-application)

```xml
    <key>CFBundleURLTypes</key>
    <array>
        <dict>
            <key>CFBundleTypeRole</key>
            <string>Editor</string>
            <key>CFBundleURLName</key>
            <string>$(PRODUCT_BUNDLE_IDENTIFIER)</string>
            <key>CFBundleURLSchemes</key>
            <array>
                <string>msalyour-client-id-here</string>
            </array>
        </dict>
    </array>
```

Look back at your sample (in Xcode) and then open the Info.plist as source code (see screenshot below):




Run the sample.  You’ll see various things you can do including creating a profile:


## App Registration

[App Registration](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal/blob/master/README.md#app-registration) 

Now, let’s create our own B2C Tenant and App Registration.  The above app registrations points to the following: https://docs.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-tenant

The first thing to realize about the steps that you’re about to see is that there can be specific points where permissions are necessary.  When you try to register your application, you’ll either be fine or realize that somebody is locking you out (to protect the security of the resources in that particular tenant.).

If this is the first time you’re running through this tutorial, you may want to start from a personal account (“trying it out at home”) so you can go through the elements, see how they work, before getting the necessary permissions to do it for “real”.

Step 2:  Make sure that you are using the directory that contains your subscription by clicking the Directory and subscription filter in the top menu and choosing the directory that contains it. This directory is different than the one that will contain your Azure AD B2C tenant.

Hmmmm….let’s unpack that shall we.

Azure account:
First - you’ll need to understand that you logged in with your Azure account. Not sure what I mean - logout and then log back in.  You’ve just used your Azure account.

Azure subscription:
Your Azure account can have more than one subscription - to manage for example centrally-billed work expenses and project A resources, project B resources, and the non-profit project sponsored by work. You can see how to do that [here](https://docs.microsoft.com/en-us/azure/billing/billing-create-subscription).

By the way: An Azure subscription has a trust relationship with Azure Active Directory (Azure AD), which means that the subscription trusts Azure AD to authenticate users, services, and devices. Multiple subscriptions can trust the same Azure AD directory, but each subscription can only trust a single directory.

If your subscription expires, you lose access to all the other resources associated with the subscription. However, the Azure AD directory remains in Azure, letting you associate and manage the directory using a different Azure subscription.
All of your users have a single home directory for authentication. However, your users can also be guests in other directories. You can see both the home and guest directories for each user in Azure AD.  See more [here](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory?toc=%2Fazure%2Fbilling%2FTOC.json)

What is a Tenant?
* A tenant is representative of an organization within Azure Active Directory. 
* It is a dedicated instance of the Azure AD service that an organization receives and owns when it signs up for a Microsoft cloud service such as Azure, Microsoft Intune, or Office 365.
* Azure tenants that access other services in a dedicated environment are considered single tenant.
* Azure tenants that access other services in a shared environment, across multiple organizations, are considered multi-tenant.

What is a Directory?
So - before we answer this - let’s look at what we mean first.  In your Azure subscription, click on your email address/icon in the upper right:


Click: Switch directory

You’ll see this (and notice - No subscriptions in andrewchungxamoutlook which shows us we’re in the wrong directory to complete the next steps in the tutorial.).


A directory refers to an Azure AD Directory.  This is the actual directory of users and the apps that the users will have access to.  The Azure AD Dictory will be used to perform identity and access management functionality for the resources of the organization (ie. of the tenant).

At this point, all the terms you need to know are defined - so go through the rest of the tutorial [here](https://docs.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-tenant);

I’m creating what I need here:

So starting with a directory + subscription that looks like this:


I’m now going to:








So at this point - we’ve created a Tenant (which represents our organization) and then further tied that to a specific Subscription.  And then we’ve further placed that B2C Tenant into a Resource group - you can see it here:


Let’s use the new tenant (click again in the upper right on your email address/icon):


For the next step - we’re going to need to register our application.






In the next step - it says:

Create a client secret
If you’re application exchanges a code for a token, you need to create an application secret.

You’ll need to click back into the webapp1.  

Then you’ll need to click “Generate Key” and press “Save” — you’ll see your App key.



Here is some background reading on some of the OAuth2.0 flows:
https://aaronparecki.com/oauth-2-simplified/

Questions to address:  
• Should I allow implicit flow?  What is implicit flow?  What is the appropriate way to set it up?
• Client Secret - should I do this?  (According to this —> PKE Code exchange is now the standard best practice: https://oauth.net/2/pkce/

It takes you here:
https://docs.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-user-flows

Questions to address:
• If you are looking for information about how to set up a resource owner password credentials (ROPC) flow in your application

We’re going to add “New user flow"





Then hit “Create”.

Then, let’s click back into that newly created userflow and test it out.



Everything should be set - but make sure the “Application” and the “Reply URL” is set:


Try running “Run user flow” - if it fails, give it a couple minutes and run it.

I’ve created a new account - I’ll use:

andrewchungxam@outlook.com
FooFIGHTERS15!

Once you signed up / or logged in, it will redirect you to the page you specified - in the sample it was jwt.ms


Notice that it returns info you submitted when you signed up -> name, country, and postal code.  These are returned key/values are called “Claims”.

Next - we’ll add the other user flows including: Profile edition and Password reset











We’ve followed the tutorial - it works but it only works with a web application!

So now, if you scroll around - we’re going to do a similar exercise to the one we’ve just completed, but we’re going to modify it slightly to make sure its optimized for mobile.

If you scroll around on the left column, let’s click Concepts and Application types.  Scroll down to Mobile and native application to [here](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-apps#mobile-and-native-applications).

The explanation is clear.  

Applications that are installed on devices, such as mobile and desktop applications, often need to access back-end services or web APIs on behalf of users. You can add customized identity management experiences to your native applications and securely call back-end services by using Azure AD B2C and the OAuth 2.0 authorization code flow.
In this flow, the application executes policies and receives an authorization_code from Azure AD after the user completes the policy. The authorization_code represents the application's permission to call back-end services on behalf of the user who is currently signed in. The application can then exchange the authorization_code in the background for an access_token and a refresh_token. The application can use the access_token to authenticate to a back-end web API in HTTP requests. It can also use the refresh_token to get a new access_token when an older one expires.

So let’s go back through the Native instructions [here](https://docs.microsoft.com/en-us/azure/active-directory-b2c/add-native-application).

Click back into your portal and into your Azure AD B2C tenant (if you navigated away, search for Azure AD B2C).  Then click on Applications.

We’ll follow the instructions but once we get to the redirect URI let’s go back to the original sample to see what instructions they asked for in terms of redirect URI [here](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal#app-registration)

```
 msal<your-client-id-here>://auth  

so in the end, using your own custom ID it will follow a format like this:
msal90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6://auth
```

By the way, make sure there are no spaces after your entry, otherwise you’ll see an error about the URI’s format.

This will correspond to this:
```
<key>CFBundleURLSchemes</key>
<array>
<string>msal90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6</string>
<string>auth</string>
</array>
```
In the end, it should look something like this:


But you’ll notice, you can’t get the ID without first creating the app.  And you can’t create the app without putting in a custom Redirect URI - so as the first step simply put any Custom Redirect URI.  For example:

Click Create.

Now we’ll have the application and we can grab it’s ID:

Finally - we’ll need start to make some changes to the actual sample with our actual data!  

The first change will be in the info.plist.  Again, right click and open as as Source Code.

Make your modifications here:


Then, as we did for the web app, try running the policies that we already created (SignupOrSigninPolicy, EditProfile, PasswordReset).

You see something like the following - and of course use the nativeapp1 as your application.




Try for example, doing the password reset flow.

You’ll be able to the steps - but bc of the format of the Reply URL, you won’t be able to see the “reply url”.  However, if you try a different flow, say Edit profile, and then try your new password, you’ll see that it will work fine. 

So now, let’s go back to the mobile app and see what we can change here:


In the file: ViewController.cs
    let kTenantName = "fabrikamb2c.onmicrosoft.com" // Your tenant name
    let kClientID = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" // Your client ID from the portal when you created your application
    let kSignupOrSigninPolicy = "b2c_1_susi" // Your signup and sign-in policy you created in the portal
    let kEditProfilePolicy = "b2c_1_edit_profile" // Your edit policy you created in the portal

    let kGraphURI = "https://fabrikamb2chello.azurewebsites.net/hello" // This is your backend API that you've configured to accept your app's tokens
    let kScopes: [String] = ["https://fabrikamb2c.onmicrosoft.com/helloapi/demo.read"] // This is a scope that you've configured your backend API to look for.

    // DO NOT CHANGE - This is the format of OIDC Token and Authorization endpoints for Azure AD B2C.
    let kEndpoint = "https://login.microsoftonline.com/tfp/%@/%@"

Now, we have enough to change the first four variables.

Let’s handle them one-by-one.

    let kTenantName = "fabrikamb2c.onmicrosoft.com" // Your tenant name

This is the URL associated with your tenant.  Find this by going again to the Directory + subscription menu on the portal (Go the portal, click your email/photo in the upper right of the screen) and find the relevant directory that you’ve been working out of up to this point (ie. the tenant you created in the early part of this tutorial.  You want that URL at the bottom of the image.):


You can also double check to see you got the right one by go Azure AD B2C in the portal (in the search bar, search for Azure AD B2C, on the overview you’ll see:



    let kClientID = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" // Your client ID from the portal when you created your application

Your clientId can be seen when click your list of applications.  Copy the Application ID that corresponds to your nativeapp1:


    let kSignupOrSigninPolicy = "b2c_1_susi" // Your signup and sign-in policy you created in the portal
    let kEditProfilePolicy = "b2c_1_edit_profile" // Your edit policy you created in the portal


Grab these from the User flows (policies) menu item here:


Becomes the following with our values:
    let kSignupOrSigninPolicy = "B2C_1_signupsignin1" // Your signup and sign-in policy you created in the portal
    let kEditProfilePolicy = "B2C_1_profileediting1" // Your edit policy you created in the portal

If you prefer not to retype the policy name, simply click the policy and copy the policy name from the next screen.

Now we’ve done all that we can with the Azure AD B2C tenant and policies we’ve set up.  The next times require us to work with a backend that we’ll need to setup 

We’re going to look and follow along a different tutorial [here](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native#optional-step-4-create-your-own-web-api):

So it has us download a sample app - Node.js Web API
https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi



Download it, follow the instructions on installing node, and then run it with:
```
node index.js
```

    let kTenantName = "2019azureadb2c.onmicrosoft.com" // Your tenant name
    let kClientID = "3a249b71-fd7b-45b8-abe2-05f28aea0293" // Your client ID from the portal when you created your application (THIS WILL BE YOUR NATIVE APP)
    let kSignupOrSigninPolicy = "B2C_1_signupsignin1" // Your signup and sign-in policy you created in the portal
    let kEditProfilePolicy = "B2C_1_profileediting1" // Your edit policy you created in the portal




Okay - we’re missing a step.  We downloaded and ran the web app - we configured a couple values there.  

However, we have to configure a couple things in the portal to make this fully work.
https://github.com/Azure-Samples/active-directory-b2c-xamarin-native#optional-step-4-create-your-own-web-api

There are things that aren’t super clear in the text so I’m going to screenshot my page:

So I’ve created one called webapp2:
I’ve created a web app - similar to the one we did earlier on - here are the values and selections:

You need this in your published scope:



Now look at this issue: https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal/issues/2

What happened here is that you need to let the Native app, ie the iOS application (nativeapp1) know that this web app (webapp2) is out there.
Ont the left side, click API access, click + Add, and then API Access:


Now it looks like this (and now, nativeapp1 - my iOS app has the webapp2 as an API and has 2 permitted scopes.  (remember scope from our definitions can be roughly translated as “permissions” - so your api has access to two sets of “permissions”)


Click on Published Scope to find your equivalent of "https://fabrikamb2c.onmicrosoft.com/helloapi/demo.read”
look fro the Full Scope Value - you won’t see the full URL, simply click and then copy it and it will copy the full URL.

Fill out the below values in your Xcode project:
    let kGraphURI = "https://localhost:5000/hello" // This is your backend API that you've configured to accept your app's tokens
    let kScopes: [String] = ["https://2019azureadb2c.onmicrosoft.com/helloapi/demo.read"] // This is a scope that you've configured your backend API to look for.




Call API - still didn’t work due to security issues - let’s add the following: (Look at the small + - button to add values and you’ll see drop down lists…if you need to add values “under” other in hierarchy, just select the carrot to the left of the value you care about.



Now it should work:




By the way, in your terminal check out the claims returned - make sure the claim your referencing in your Node.js code matches a what you’re looking for in the Node.js code.  Note if you’re changing code in node you just both save the file you’ve modified (this won’t happen automatically) and then rerun the command "node index.js”.


You can change the claim you’re referencing here in the Node.js code:  Look at the last two lines - two examples of claims you can be looking for:



And if you want to change or add, claims - you can do this in the portal.

Click under the User flows (policies) and then under each policy - you can change the User attribute and Application claim.  (You have do this for each policy and you have to change User attribute and Application claim separately.)

For example, below you can add City.  Simply select it below and then press Save.



then you do it again for Application claims.


Now that is done for the SignupSignIn policy - if you’d like to do it for the other policies (like EditProfile, then select into that policy and repeat the same process.).

Review, in the end, it should look like this:
Xcode (clientID represents your native app in the AD B2C portal):



Node.js project: (clientID represents your web app)


DONE

My localServer  project is here:
/Users/andrewc/Github/active-directory-b2c-javascript-nodejs-webapi copy

run with node index.js

Notice the clientID corresponds to: 




My local project is here: 
/Users/andrewc/Projects/active-directory-b2c-ios-swift-native-msal-master copy

I care about this:


And the client ID corresponds to this:






And the webapp2:






We’re going to move the Node.js project to the cloud.
https://docs.microsoft.com/en-us/azure/app-service/app-service-web-get-started-nodejs
Make sure, you’re in the correct folder


zip -r myAppFiles.zip .

It lists both the Bath / PowerShell - but you don’t have to do both.

Then, Cloud Shell in the Azure portal -> the button to the right of the search bar:


Once in the cloud shell, you can select either Bash or PowerShell.







az group create --name myResourceGroup --location “East US"



az appservice plan create --name myAppServicePlan --resource-group myResourceGroup --sku FREE



az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name <app_name>

az webapp create --resource-group myResourceGroup --plan myAppServicePlan --name myB2CAppName2019



# Bash and Powershell
az webapp config appsettings set --resource-group myResourceGroup --name <app_name> --settings WEBSITE_NODE_DEFAULT_VERSION=10.14.1
# Bash and Powershell
az webapp config appsettings set --resource-group myResourceGroup --name myB2CAppName2019 --settings WEBSITE_NODE_DEFAULT_VERSION=10.14.1



The next step says - Bash -  but actually, you want to go to this website from your browser.
http://<app_name>.azurewebsites.net

So in my case:
http://myB2CAppName2019.azurewebsites.net

Next, you can deploy via ZIP - notice the “scm” in the website:
https://<app_name>.scm.azurewebsites.net/ZipDeployUI

So in my case:
https://myb2cappname2019.scm.azurewebsites.net/zipdeployui



Drag the file anywhere below the hostingstart.html - and you’ll see a pop-up that says, “Drag file here to deploy your ZIP"

In the end, it ends like this:


Go back to the website:
http://<app_name>.azurewebsites.net



Let’s go back to the default directory:


And then, look back in our resource groups:

Click the cube icon on the left hand column:






Take a look at your app service and make sure it says:


Let’s go back - into both Azure portal around B2C and then the iOS App.

iOS app back in the ViewController.swift
    let kGraphURI = "http://localhost:5000/hello" // This is your backend API that you've configured to accept your app's tokens

So we’ll change this now:
    let kGraphURI = “a/hello" // This is your backend API that you've configured to accept your app's tokens


Notice - nothing is returned.












So now, we’ll go through the Edit profile and then add then Given Name:




Then, in a new terminal window switch to Powershell.  



NOTE TO SELF:


CHANGED THE PASSWORD: 
andrewchungxam@outlook.com
FooERS1!!!    >>> old FooERS1!!

Doing this on local:  local == local swift project that i created
Lstrongworkandrewchung@gmail.com
Local1!Hi

I used this for the fabrican-tenant:
applicationreview@gmail.com
Local1!Hi

Questions to address:
<Imagine something like Facebook> How do I give Jimmy access to only his and his friend’s posts?  How do I make sure the access token I give to Jimmy cannot be (maliciously) used to grab everybody’s data? 


Good overview of OAuth2.0
https://developer.okta.com/blog/2017/07/25/oidc-primer-part-1
https://aaronparecki.com/oauth-2-simplified/




https://stackoverflow.com/questions/37371595/cannot-start-nodejs-web-api-hosted-in-azure

copying this from downloads into the GIT file.






THREE VERSIONS OF WEB CONFIG FILES:


NOTES SELF:

SHOWABLE:
2019azureadb2c.onmicrosoft.com
3a249b71-fd7b-45b8-abe2-05f28aea0293

REAL:
    let kTenantName = "2019azureadb2c.onmicrosoft.com" // Your tenant name
    let kClientID = "57d029a1-afb0-4125-b252-46248a2eadf5" // Your client ID from the portal when you created your application
    let kSignupOrSigninPolicy = "B2C_1_signupsignin1" // Your signup and sign-in policy you created in the portal
    let kEditProfilePolicy = "B2C_1_profileediting1" // Your edit policy you created in the portal


Doing this on local:
strongworkandrewchung@gmail.com
Local1!Hi

    <key>NSAppTransportSecurity</key>
    <dict>
        <key>NSExceptionDomains</key>
        <dict>
            <key>localhost:5000</key>
            <dict>
                <key>NSExceptionAllowsInsecureHTTPLoads</key>
                <true/>
                <key>NSIncludesSubdomains</key>
                <true/>
            </dict>
        </dict>
    </dict>