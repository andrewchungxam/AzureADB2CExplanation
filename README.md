# AzureADB2CExplanation
Azure AD B2C Explanation with Swift and Node.js

The purpose of this doc is to create a complementary “explainer” to the following sample from the Azure-Samples repo: https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal .

In the end, you'll have a Swift app that has the ability for a user to SignUp/SignIn, Edit Profile, Logout, and the ability to call an API.  The API will be a Node.js application that you'll run locally on your Mac. 

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

### Here are relevant parameters:
Azure AD B2C
Tenant
ClientId
SignupOrSigninPolicy
EditProfilePolicy

### BackendService:
GraphURI
Scope

### Let’s define them in the simplest terms: <br />

*Azure AD B2C:* Tech that allows you to add/edit/delete users + and allows these users to have controlled access to your backend API.  <br />

*Tenant:* tenant is the “folder” or “container” where you define an Azure AD B2C instance and where you define “applications”.  
“Application” are either the web endpoints or mobile apps that you are trying to make available to certain users who authenticate. <br />

*ClientId:* Applications in the portal are assigned an Id, this is called the ClientId. <br />

*SignupOrSigninPolicy/EditProfilePolicy:* Policies define the types of interaction flows that users will see/have access to.  (e.g. allowing users to sign up for an account or allowing a user to edit an existing account.  Rather than coding any of these - common flows are defined as policies; policies can be slightly customized. <br /> <br />

BackendService: <br />

*GraphURI:* This would simply be a backend API - obviously this API will give your user access to useful data

*Scope:* Think of them as permissions in the world of OAuth2.0.  Backend services don’t need to do a binary “give access to everything” or “don’t have access to anything.”  Instead, they can define scopes which give access to a small slice of the full API.  As an example, let’s look at Microsoft Graph — 3 examples of scopes are “Calendars.Read”, “Calendars.ReadWrite”, “Mail.Send” (You can probably guess what each of these scopes give permission; however, of course, these scope definitions are defined by the creators of the backend services.)   <br />

*What are Roles?  Is it supported in Azure AD B2C?*  <br />
https://stackoverflow.com/questions/53602804/azure-active-directory-custom-roles-and-possible-scopes  <br />
There is no support today for custom roles in Azure Active Directory. Only the predefined Administrator Roles, as described in the documentation, are available for use.  <br />
https://docs.microsoft.com/en-us/azure/active-directory/users-groups-roles/directory-assign-admin-roles  <br />

*What is MSAL?* <br />
MSAL is a developer library that helps you to obtain tokens from MSA, Azure AD or Azure B2C for accessing protected resources – such as your own API, Microsoft's API (such as the Microsoft Graph) and any other 3rd party choosing to protect their API with Microsoft identity.  In OAuth2.0, transfer of Auth and Refresh tokens are core to how authenticated uses are permitted access to resources. Safely handling these tokens is hard, and by using the MSAL, you don’t have to code it yourself.  <br />

Running the sample:  <br /> 
The instructions are not the most clear on this point - but actually, the sample comes connected with a working Azure AD B2C instance already configured.  You can't change any of the values, but you can run the sample and use it to login etc. There are some minor steps/installations that you’ll need to do before you can get the sample running. <br />

Note - I’m running my sample from a Mac + Visual Studio for Mac.  I’ll assume you have access to a Mac as we are dealing with Swift/iOS in this sample.  <br />

In the sample, there are some installation instructions [here](
https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal/blob/master/README.md#if-youre-building-for-ios-tvos-or-watchos).

Firstly, I ran the first command:  <br />
brew install carthage  (it may update brew, and often you’ll have carthage already installed and so you’ll instead be asked to update carthage).

If you see the complaints - simply upgrade it: <br />
brew upgrade carthage

Then:<br />
```
carthage bootstrap
```

Then there are many instructions of dragging and dropping various frameworks - I found that these were already preset/done in the sample - let me include the screeshots so you can confirm you see the same:<br />

Step 4 from these [instructions](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal/blob/master/README.md#if-youre-building-for-ios-tvos-or-watchos):  (If you’re not familiar play attention to the what has been highlighted (from left to right): The blue folder, the solution highlighted, the target (MSALiOSB2C), General (in blue), and then scroll down until you see “Linked Frameworks and Libraries”)

<img width="1221" alt="283C4FDE-F5B1-4B6A-A41F-984C8C372C12" src="https://user-images.githubusercontent.com/3628580/55828550-ac347480-5ada-11e9-97a3-10e155d04f95.png">

<br />
Step 5 from these [instructions](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal/blob/master/README.md#if-youre-building-for-ios-tvos-or-watchos):  (If you’re not familiar play attention to the what has been highlighted (from left to right): The blue folder, the solution highlighted, the target (MSALiOSB2C), Build Phases (in blue), and see the Run Script.  

<img width="1100" alt="2-505888E7-E31B-4A7E-BB31-2C6C7AA46CFF" src="https://user-images.githubusercontent.com/3628580/55828654-e736a800-5ada-11e9-91fa-3dcc0f839336.png">

<br />
Notice this instruction [Step 1:](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal/blob/master/README.md#configure-your-application) <br />

```
xml
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

<img width="537" alt="3-ADF39104-CCB8-4509-9347-60841BE4A476" src="https://user-images.githubusercontent.com/3628580/55828739-1c42fa80-5adb-11e9-9c1f-3c7dd5861f2c.png">

<img width="596" alt="4-5411B8D4-E861-4E9F-A61F-807958047DEB" src="https://user-images.githubusercontent.com/3628580/55828806-3d0b5000-5adb-11e9-91ed-155719af78c6.png">

<br />
Run the sample.  You’ll see various things you can do including creating a profile:

<img width="482" alt="5-F22D6B63-6C93-43E0-8F5B-BFD0A22986E8" src="https://user-images.githubusercontent.com/3628580/55828836-4c8a9900-5adb-11e9-9ea5-c3a24414eebe.png">
<br />
## App Registration

[App Registration](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal/blob/master/README.md#app-registration) <br />

Now, let’s create our own B2C Tenant and App Registration.  The above app registrations points to the following: https://docs.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-tenant <br />

The first thing to realize about the steps that you’re about to see is that there can be specific points where permissions are necessary.  When you try to register your application, you’ll either be fine or realize that somebody is locking you out (to protect the security of the resources in that particular tenant.).<br />

If this is the first time you’re running through this tutorial, you may want to start from a personal account (“trying it out at home”) so you can go through the elements, see how they work, before getting the required permissions.<br />

Step 2:  Make sure that you are using the directory that contains your subscription by clicking the Directory and subscription filter in the top menu and choosing the directory that contains it. This directory is different than the one that will contain your Azure AD B2C tenant.<br />

Hmmmm….let’s unpack that shall we.<br />

Azure account:<br />
First - you’ll need to understand that you logged in with your Azure account. Not sure what I mean? Logout and then log back in.  You’ve just used your Azure account.<br />

Azure subscription:<br />
Your Azure account can have more than one subscription - to manage, for example, centrally-billed work expenses and then seperating project A resources from project B resources, and then one for the non-profit project sponsored by work. You can see how to do that [here](https://docs.microsoft.com/en-us/azure/billing/billing-create-subscription).<br />

By the way: An Azure subscription has a trust relationship with Azure Active Directory (Azure AD), which means that the subscription trusts Azure AD to authenticate users, services, and devices. Multiple subscriptions can trust the same Azure AD directory, but each subscription can only trust a single directory.<br />

If your subscription expires, you lose access to all the other resources associated with the subscription. However, the Azure AD directory remains in Azure, letting you associate and manage the directory using a different Azure subscription.
All of your users have a single home directory for authentication. However, your users can also be guests in other directories. You can see both the home and guest directories for each user in Azure AD.  See more [here](https://docs.microsoft.com/en-us/azure/active-directory/fundamentals/active-directory-how-subscriptions-associated-directory?toc=%2Fazure%2Fbilling%2FTOC.json)<br />

What is a Tenant?<br />
* A tenant is representative of an organization within Azure Active Directory. <br />
* It is a dedicated instance of the Azure AD service that an organization receives and owns when it signs up for a Microsoft cloud service such as Azure, Microsoft Intune, or Office 365.<br />
* Azure tenants that access other services in a dedicated environment are considered single tenant.<br />
* Azure tenants that access other services in a shared environment, across multiple organizations, are considered multi-tenant.<br />

What is a Directory?
So - before we answer this - let’s look at what we mean first.  In your Azure subscription, click on your email address/icon in the upper right:

<img width="323" alt="6-651CAE89-5020-441F-ADE3-56D5BA62EF5F" src="https://user-images.githubusercontent.com/3628580/55828897-6deb8500-5adb-11e9-9a6a-35769cc9a5fb.png">
<br />
Click: Switch directory
<br />
You’ll see this (and notice - No subscriptions in andrewchungxamoutlook which shows us we’re in the wrong directory to complete the next steps in the tutorial.).<br />

<img width="347" alt="7-6CD9E18C-7BA3-4028-826F-FF208A9F1319" src="https://user-images.githubusercontent.com/3628580/55828991-9d01f680-5adb-11e9-89ed-a846d9320822.png">

A directory refers to an Azure AD Directory.  This is the actual directory of users and the apps that the users will have access to.  The Azure AD Dictory will be used to perform identity and access management functionality for the resources of the organization (ie. of the tenant). <br />

At this point, all the terms you need to know are defined - so go through the rest of the tutorial [here](https://docs.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-tenant); 

<br />
I’m creating what I need here: 
<br />
So starting with a directory + subscription that looks like this: 

![9-1644D5A9-0205-471C-9727-515EBE910257](https://user-images.githubusercontent.com/3628580/55829182-11d53080-5adc-11e9-8371-046a39572ea7.png)

<br />
I’m now going to: 

![10-9175C845-28B0-4291-82C4-0A079907EAA6](https://user-images.githubusercontent.com/3628580/55829208-274a5a80-5adc-11e9-9175-831f630559d1.png)

![11-85154D82-3CC1-420C-9B37-C954E85659AC](https://user-images.githubusercontent.com/3628580/55829246-392bfd80-5adc-11e9-99c5-45e5a573ef45.png)

![12-817A5314-E6C3-4031-8008-6E06895CC528](https://user-images.githubusercontent.com/3628580/55829265-4517bf80-5adc-11e9-8040-2111531cac36.png)

![13-F989ACE6-8F4B-419E-AB93-288C2BB0B2FF](https://user-images.githubusercontent.com/3628580/55829285-4fd25480-5adc-11e9-8591-ad07fbf645aa.png)

<br />
So at this point - we’ve created a Tenant (which represents our organization) and then further tied that to a specific Subscription.  And then we’ve further placed that B2C Tenant into a Resource group - you can see it here: <br />

![14-5B970500-65B9-4759-85E9-48DB8BCC8882](https://user-images.githubusercontent.com/3628580/55829345-74c6c780-5adc-11e9-87a0-03b8987263c1.png)

<br />
Let’s use the new tenant (click again in the upper right on your email address/icon):

![15-46C7A84A-E7EB-464D-B3DC-9CAB8066239B](https://user-images.githubusercontent.com/3628580/55829359-8019f300-5adc-11e9-8ef6-3f1bacf19c1f.png)

<br />
For the next step - we’re going to need to register our application.

![16-2D1E739E-AFD6-4DAA-BB71-0B45F8A1235D](https://user-images.githubusercontent.com/3628580/55829466-b8213600-5adc-11e9-826f-2585c8cf19c1.png)

![17-8A104967-ADF6-41CC-9B70-450A3AC2F466](https://user-images.githubusercontent.com/3628580/55829502-cd966000-5adc-11e9-982a-7fe7899bc7f5.png)

![18-8708FD04-CDEF-42FE-A9F0-5335F1C06902](https://user-images.githubusercontent.com/3628580/55829528-dab34f00-5adc-11e9-90da-d19c8a202557.png)

<br />
Skip the next section "Create a client secret.  If you'd like to learn more, here is some background reading on some of the OAuth2.0 flows: https://aaronparecki.com/oauth-2-simplified/ and https://oauth.net/2/pkce/ <br />

Creating User Flows: <br /> 
https://docs.microsoft.com/en-us/azure/active-directory-b2c/tutorial-create-user-flows <br />

We’re going to add “New user flow":

![19-B2F2346B-414D-4375-89CC-8DEB396DC8C9](https://user-images.githubusercontent.com/3628580/55829692-45fd2100-5add-11e9-8ce8-81dbcb8d8840.png)

![20-A85CE894-0036-496B-8E72-31D6AD35372C](https://user-images.githubusercontent.com/3628580/55829716-56150080-5add-11e9-9e8f-471d63fbaff5.png)

![21-636FE99C-552B-46A6-A5AE-C8EE798AC7A9](https://user-images.githubusercontent.com/3628580/55829753-6fb64800-5add-11e9-9f93-ae2536ee1f85.png)

Then hit “Create”. 

<br />
Then, let’s click back into that newly created userflow and test it out.<br />

![22-9BC4B7EC-5B33-4BE4-A681-9D7A15C421EF](https://user-images.githubusercontent.com/3628580/55829802-88266280-5add-11e9-892b-00a00b508cac.png)

Everything should be set - but make sure the “Application” and the “Reply URL” is set: 

![23-F13B0345-2C59-4812-878D-97CE203775D2](https://user-images.githubusercontent.com/3628580/55829831-9aa09c00-5add-11e9-9926-c3dbf9a5ac4e.png)

<br />
Try running “Run user flow” - if it fails, give it a couple minutes and run it.<br />
<br />
I’ve created a new account - I’ll use:<br />

```
andrewchungxam@outlook.com
FooFIGHTERS15! (changed)
```
<br />
Once you signed up / or logged in, it will redirect you to the page you specified - in the sample it was jwt.ms
<br />

![24-BDEEE448-5BB6-44F9-A444-8C5F027F3FEF](https://user-images.githubusercontent.com/3628580/55829856-ae4c0280-5add-11e9-98f0-246447f0f781.png)

Notice that it returns info you submitted when you signed up -> name, country, and postal code.  These are returned key/values are called “Claims”. <br />

Next - we’ll add the other user flows including: Profile edition and Password reset<br />

![25-3-ADF39104-CCB8-4509-9347-60841BE4A476](https://user-images.githubusercontent.com/3628580/55829882-befc7880-5add-11e9-95cc-8ffed7cbdf26.png)

![26-177B730A-78CE-4453-B180-2E76A142E549](https://user-images.githubusercontent.com/3628580/55829927-db001a00-5add-11e9-8dfb-f0c7078470e1.png)

![27-AC19E76F-735E-4F23-A14A-9A14136310EC](https://user-images.githubusercontent.com/3628580/55829942-e7847280-5add-11e9-8185-dba182a2220b.png)

![28-4173949B-84B2-4A70-8809-4492CDB3EADB](https://user-images.githubusercontent.com/3628580/55829954-f10dda80-5add-11e9-826b-77e851c422ed.png)

![29-A9DFDFB2-06AA-447E-ABC2-7BC0101297D2](https://user-images.githubusercontent.com/3628580/55829971-f9661580-5add-11e9-97cc-d6483aafe217.png)

<br />
We’ve followed the tutorial - it works but it only works with a web application!<br />

<br />
So now, if you scroll around - we’re going to do a similar exercise to the one we’ve just completed, but we’re going to modify it slightly to make sure its optimized for mobile.<br />

<br />
If you scroll around on the left column, let’s click Concepts and Application types.  Scroll down to Mobile and native application to [here](https://docs.microsoft.com/en-us/azure/active-directory-b2c/active-directory-b2c-apps#mobile-and-native-applications). <br />

<br />
The explanation is clear.  <br />

<br />
Applications that are installed on devices, such as mobile and desktop applications, often need to access back-end services or web APIs on behalf of users. You can add customized identity management experiences to your native applications and securely call back-end services by using Azure AD B2C and the OAuth 2.0 authorization code flow.<br />

In this flow, the application executes policies and receives an authorization_code from Azure AD after the user completes the policy. The authorization_code represents the application's permission to call back-end services on behalf of the user who is currently signed in. The application can then exchange the authorization_code in the background for an access_token and a refresh_token. The application can use the access_token to authenticate to a back-end web API in HTTP requests. It can also use the refresh_token to get a new access_token when an older one expires. <br />

<br />
So let’s go back through the Native instructions [here](https://docs.microsoft.com/en-us/azure/active-directory-b2c/add-native-application). <br />

<br />
Click back into your portal and into your Azure AD B2C tenant (if you navigated away, search for Azure AD B2C).  Then click on Applications.<br />

<br />
We’ll follow the instructions but once we get to the redirect URI let’s go back to the original sample to see what instructions they asked for in terms of redirect URI [here](https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal#app-registration) <br />

```
msal<your-client-id-here>://auth  
so in the end, using your own custom ID it will follow a format like this:
msal90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6://auth
```

<br />
By the way, make sure there are no spaces after your entry, otherwise you’ll see an error about the URI’s format. <br />

<br />
This will correspond to this:

```
<key>CFBundleURLSchemes</key>
<array>
<string>msal90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6</string>
<string>auth</string>
</array>
```

<br />
In the end, it should look something like this:

![30-DA0A3E96-FCAF-4B83-B481-3C7DE41DE30F](https://user-images.githubusercontent.com/3628580/55830014-0e42a900-5ade-11e9-9392-8fc9da268386.png)

<br />
But you’ll notice, you can’t get the ID without first creating the app.  And you can’t create the app without putting in a custom Redirect URI - so as the first step simply put any Custom Redirect URI.  For example:<br />

![31-3A81B684-F808-4745-A309-C38C468D9F49](https://user-images.githubusercontent.com/3628580/55830041-231f3c80-5ade-11e9-871d-ad80aa25dfff.png)

<br />
Click Create.<br />

<br />
Now we’ll have the application and we can grab it’s ID:<br />

![34-0DD199FE-D5E0-46A4-B599-12932A19E6BD](https://user-images.githubusercontent.com/3628580/55830080-39c59380-5ade-11e9-80da-89125c19622d.png)

<br />
Finally - we’ll need start to make some changes to the actual sample with our actual data! <br /> 
<br />
The first change will be in the info.plist.  Again, right click and open as as Source Code.<br />
<br />
Make your modifications here:<br />

<img width="811" alt="35-10FAEE98-4C33-4F09-BA0A-A71E2142793F" src="https://user-images.githubusercontent.com/3628580/55830198-71ccd680-5ade-11e9-8dbf-de7e516b5b79.png">

<br />
Then, as we did for the web app, try running the policies that we already created (SignupOrSigninPolicy, EditProfile, PasswordReset).<br />
<br />
You see something like the following - and of course use the nativeapp1 as your application.<br />

![36-B62AE412-58B4-4039-BD42-58CFD0DD24BD](https://user-images.githubusercontent.com/3628580/55830221-7e512f00-5ade-11e9-8915-374fd10fa7a0.png)

<br />
Try for example, doing the password reset flow.<br />
<br />
You’ll be able to the steps - but bc of the format of the Reply URL, you won’t be able to see the “reply url”.  However, if you try a different flow, say Edit profile, and then try your new password, you’ll see that it will work fine. <br />
<br />
So now, let’s go back to the mobile app and see what we can change here:<br />

<br />
In the file: ViewController.cs<br />

```
    let kTenantName = "fabrikamb2c.onmicrosoft.com" // Your tenant name
    let kClientID = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" // Your client ID from the portal when you created your application
    let kSignupOrSigninPolicy = "b2c_1_susi" // Your signup and sign-in policy you created in the portal
    let kEditProfilePolicy = "b2c_1_edit_profile" // Your edit policy you created in the portal

    let kGraphURI = "https://fabrikamb2chello.azurewebsites.net/hello" // This is your backend API that you've configured to accept your app's tokens
    let kScopes: [String] = ["https://fabrikamb2c.onmicrosoft.com/helloapi/demo.read"] // This is a scope that you've configured your backend API to look for.

    // DO NOT CHANGE - This is the format of OIDC Token and Authorization endpoints for Azure AD B2C.
    let kEndpoint = "https://login.microsoftonline.com/tfp/%@/%@"
```  

<br />
Now, we have enough to change the first four variables.<br />
<br />

Let’s handle them one-by-one.<br />
```
    let kTenantName = "fabrikamb2c.onmicrosoft.com" // Your tenant name
```

<br />
This is the URL associated with your tenant.  Find this by going again to the Directory + subscription menu on the portal (Go the portal, click your email/photo in the upper right of the screen) and find the relevant directory that you’ve been working out of up to this point (ie. the tenant you created in the early part of this tutorial.  You want that URL at the bottom of the image.):<br />

![37-E8CE1981-7D30-4E1A-B53A-896B5E103BCB](https://user-images.githubusercontent.com/3628580/55830346-b35d8180-5ade-11e9-8442-25349d7a7803.png)

You can also double check to see you got the right one by go Azure AD B2C in the portal (in the search bar, search for Azure AD B2C, on the overview you’ll see:<br />

![38-4863BB00-813A-49C9-8C7E-BA4B3EB3D8A7](https://user-images.githubusercontent.com/3628580/55830421-de47d580-5ade-11e9-9c8b-67fde4e3bdee.png)

```
    let kClientID = "90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6" // Your client ID from the portal when you created your application
```
<br />
Your clientId can be seen when click your list of applications.  Copy the Application ID that corresponds to your nativeapp1:

![39-0DD199FE-D5E0-46A4-B599-12932A19E6BD](https://user-images.githubusercontent.com/3628580/55830440-ef90e200-5ade-11e9-9735-00cb5005d912.png)

```
    let kSignupOrSigninPolicy = "b2c_1_susi" // Your signup and sign-in policy you created in the portal
    let kEditProfilePolicy = "b2c_1_edit_profile" // Your edit policy you created in the portal
```
<br />
Grab these from the User flows (policies) menu item here:<br />

![40-E72EAA44-F450-4759-A673-98D5CDA7F2E0](https://user-images.githubusercontent.com/3628580/55830479-020b1b80-5adf-11e9-92e1-27c71cbdcd8f.png)

<br />
Becomes the following with our values:<br />

```
    let kSignupOrSigninPolicy = "B2C_1_signupsignin1" // Your signup and sign-in policy you created in the portal
    let kEditProfilePolicy = "B2C_1_profileediting1" // Your edit policy you created in the portal
```

<br />
If you prefer not to retype the policy name, simply click the policy and copy the policy name from the next screen.<br />
<br />
Now we’ve done all that we can with the Azure AD B2C tenant and policies we’ve set up.  The next times require us to work with a backend that we’ll need to setup <br />
<br />
We’re going to look and follow along a different tutorial [here](https://github.com/Azure-Samples/active-directory-b2c-xamarin-native#optional-step-4-create-your-own-web-api):<br />
<br />
So it has us download a sample app - Node.js Web API
https://github.com/Azure-Samples/active-directory-b2c-javascript-nodejs-webapi<br />

<br />
Download it, follow the instructions on installing node, and then run it with:

```
node index.js
```

Make note of a few values:

![41-747E926CF-3AFF-4F09-9783-873C72897782](https://user-images.githubusercontent.com/3628580/55830570-35e64100-5adf-11e9-9c6b-3fddb8165b41.png)

Let's review:
```
    let kTenantName = "2019azureadb2c.onmicrosoft.com" // Your tenant name
    let kClientID = "3a249b71-fd7b-45b8-abe2-05f28aea0293" // Your client ID from the portal when you created your application (THIS WILL BE YOUR NATIVE APP)
    let kSignupOrSigninPolicy = "B2C_1_signupsignin1" // Your signup and sign-in policy you created in the portal
    let kEditProfilePolicy = "B2C_1_profileediting1" // Your edit policy you created in the portal
```

Then, let's run the app.  You'll quickly see that we run into the folowing issue:

<img width="417" alt="42-6DAF6951-34B9-43E5-9CCD-369FA6FF686D" src="https://user-images.githubusercontent.com/3628580/55830990-21ef0f00-5ae0-11e9-9abf-935d3a8944d1.png">

<br />
Okay - we’re missing a step.  We downloaded and ran the web app - we configured a couple values there.  <br />
<br />
However, we have to configure a couple things in the portal to make this fully work.
https://github.com/Azure-Samples/active-directory-b2c-xamarin-native#optional-step-4-create-your-own-web-api <br />
<br />
There are things that aren’t super clear in the text so I’m going to screenshot my page: <br />
<br />
So I’ve created one called webapp2:<br />
<br />
I’ve created a web app - similar to the one we did earlier on - here are the values and selections:<br />

![43-E01A73C6-4E11-4C48-9A33-5F45FBB2EE0A](https://user-images.githubusercontent.com/3628580/55831043-3d5a1a00-5ae0-11e9-8da2-451deb70ae67.png)

<br />
You need this in your published scope:<br />

![44-946BDA93-D8A4-4ACC-8CAF-C31EEB3F05FD](https://user-images.githubusercontent.com/3628580/55831073-4cd96300-5ae0-11e9-806b-800f5954c782.png)

<br />
Now look at this issue: https://github.com/Azure-Samples/active-directory-b2c-ios-swift-native-msal/issues/2 <br />

<br />
What happened here is that you need to let the Native app, ie the iOS application (nativeapp1) know that this web app (webapp2) is out there. <br />

<br />
On the left side, click API access, click + Add, and then API Access:<br />

![45-C71AEEFD-8EA6-45FE-A3D6-1CEFA0EE2EBA](https://user-images.githubusercontent.com/3628580/55831192-87430000-5ae0-11e9-83c3-9763ffb8f96d.png)

<br />
Now it looks like this (and now, nativeapp1 - my iOS app has the webapp2 as an API and has 2 permitted scopes.  (remember scope from our definitions can be roughly translated as “permissions” - so your api has access to two sets of “permissions”)<br />

![46-86884B82-D1FF-4F2D-87C8-99CDF7DD6AD7](https://user-images.githubusercontent.com/3628580/55831222-9a55d000-5ae0-11e9-9361-b405233bded7.png)


<br />
Click on Published Scope to find your equivalent of "https://fabrikamb2c.onmicrosoft.com/helloapi/demo.read”
look for the Full Scope Value - you won’t see the full URL, simply click and then copy it and it will copy the full URL.<br />

<br />
Fill out the below values in your Xcode project:

```
    let kGraphURI = "https://localhost:5000/hello" // This is your backend API that you've configured to accept your app's tokens
    let kScopes: [String] = ["https://2019azureadb2c.onmicrosoft.com/helloapi/demo.read"] // This is a scope that you've configured your backend API to look for.
```

<img width="403" alt="47-56080DB6-BEDE-4A55-B985-ABC5A14DD31F" src="https://user-images.githubusercontent.com/3628580/55831262-b194bd80-5ae0-11e9-9656-ad7c186a1aee.png">


<br />
However, when you press - Call API - it still doesn’t work due to security issues - let’s add the following: (Look at the small + - button to add values and you’ll see drop down lists…if you need to add values “under” other in hierarchy, just select the carrot to the left of the value you care about.  Note, we're allowing insecure transfers of data - this should be done with caution and certainly not be allowed in production scenarios!  <br />

<img width="648" alt="48-FAF1D923-5CA9-4B5A-8BA1-BD9D449F8756" src="https://user-images.githubusercontent.com/3628580/55831282-bd807f80-5ae0-11e9-9d68-5c2d26db1d11.png">


<br />
Now it should work:<br />

<img width="392" alt="49-414DB552-E342-4353-A9A5-85E918A2C769" src="https://user-images.githubusercontent.com/3628580/55831301-c6715100-5ae0-11e9-82e6-5ab8d1e162b3.png">


<br />
By the way, in your terminal check out the claims returned - make sure the claim your referencing in your Node.js code matches a what you’re looking for in the Node.js code.  Note if you’re changing code in node you just both save the file you’ve modified (this won’t happen automatically) and then rerun the command "node index.js”.<br />

<img width="866" alt="50-678E9AAF-08FF-4AC8-B659-89988DE7DC66" src="https://user-images.githubusercontent.com/3628580/55831320-d0934f80-5ae0-11e9-8ef9-ab170e30e215.png">


<br />
You can change the claim you’re referencing here in the Node.js code:  Look at the last two lines - two examples of claims you can be looking for:<br />

![51-BDCB4AB3-CCEA-4DCE-B667-B5246A40F987](https://user-images.githubusercontent.com/3628580/55831345-de48d500-5ae0-11e9-9c83-0f42103c5563.png)

<br />
And if you want to change or add, claims - you can do this in the portal.<br />

<br />
Click under the User flows (policies) and then under each policy - you can change the User attribute and Application claim.  (You have do this for each policy and you have to change User attribute and Application claim separately.)<br />

<br />
For example, below you can add City.  Simply select it below (it is the first selection box) and then press Save.<br />

![52-A3587DA2-8148-4A6D-A4E7-11129689D206](https://user-images.githubusercontent.com/3628580/55831370-ed2f8780-5ae0-11e9-8fb1-dbf59a4ffcc0.png)

<br />
then you do it again for Application claims.<br />

![53-B2C6B002-F03D-41F0-9843-4D609ED36AF8](https://user-images.githubusercontent.com/3628580/55831391-fb7da380-5ae0-11e9-8933-a3c728a93589.png)

<br />
Now that is done for the SignupSignIn policy - if you’d like to do it for the other policies (like EditProfile, then click into that policy and repeat the same process.).<br />

<br />
Review, in the end, it should look like this: <br />
Xcode (clientID represents your native app in the AD B2C portal): <br />

<img width="1091" alt="54-A324315D-89DA-4321-800A-E162FB747755" src="https://user-images.githubusercontent.com/3628580/55831496-37186d80-5ae1-11e9-81b2-3985e3ac26e0.png">


<img width="616" alt="55-F7F9B610-99DC-4661-8597-B4E3E47B0389" src="https://user-images.githubusercontent.com/3628580/55831490-3253b980-5ae1-11e9-9ffa-9c591cb05cd5.png">


<br />
Node.js project: (clientID represents your web app)<br />

![56-D5566FD7-1453-44AD-A0BE-B9FEE7F56E32](https://user-images.githubusercontent.com/3628580/55831475-2bc54200-5ae1-11e9-8242-087c2e7c1b6b.png)


![57-EC30C8E7-3FD2-46FE-8635-E186D3FAB0C2](https://user-images.githubusercontent.com/3628580/55831467-27008e00-5ae1-11e9-8075-52b0a8226812.png)

