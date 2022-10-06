# Okta Customer Identity for Developers Lab Guide

Copyright 2022 Okta, Inc. All Rights Reserved.

## Table of Contents

  - [Lab 1.1: Access Your Okta Org](#lab-11-access-your-okta-org)

  - [Lab 1.2: Create Okta Accounts](#lab-12-create-okta-groups)

  - [Lab 1.3: Create an Okta Group](#lab-13-create-okta-users)

  - [Lab 1.4: Create an Application Integration](#lab-14-create-an-okta-application-integration)

  - [Lab 2.1: Configure a Custom Domain](#)

  - [Lab 2.2: Customize the Okta Sign-In Page with the Branding UI](#)

  - [Lab 2.3: Customize the Okta Sign-In Page Using the Sign-In Page Code Editor](#)
  
  - [Lab 3: ](#)

  - [Lab 4.1: Get an API Token and Set Up the Postman Environment](#lab-41-get-an-api-token-and-set-up-the-postman-environment)

# Lab 1.1: Access Your Okta Org

🎯 **Objective**:    Sign in to your virtual machine and authenticate to your Okta organization.

⏱️ **Duration**:    15 minutes


## Access Your VM

## Access Your Okta Admin Dashboard

1. Launch the Chrome browser.

2. Copy the Okta Org assigned to you (e.g. `https://oktaiceXXX.oktapreview.com`)

3.  On the **Okta Sign In** page, sign in with the credentials assigned to you.

4.  Select a recovery question and enter in a recovery answer.

5.  Click `Create My Account`

## ✅ Checkpoint

At this point, you have access to your lab environment to complete the rest of the labs.

# Lab 1.2: Create Okta Groups

  🎯 **Objective**   Create Okta Groups -- one for Okta Ice Franchisees and one for Okta Ice customers.

  🎬 **Scenario**   Franchisees and customers require a distinct groups for application access and access policies.

  ⏱️ **Duration**     10 minutes

## Create a Franchisee Group
1.  Ensure you are logged in as your Okta Super Admin account `oktatraining` and that you are on the `Admin` dashboard.

2.  Navigate to `Directory > Groups`

3.  Click `Add Group`

4.  In the `Name` field, enter `Franchisees`

5. Click `Save`

## Create a Customer Group

1.  Click `Add Group` again.

2.  This time, enter `Customers` in the `Name` field.

3. Click `Save`

## ✅ Checkpoint

You now have two Okta Groups that you will use to manage access to applications.

# Lab 1.3 Create Okta Users
  
  🎯 **Objective**:   Create some End User accounts and assign them to Okta Groups for testing configurations.

  🎬 **Scenario**:    We'll need some End Users to test out the Franchisee and Customer experience.

  ⏱️ **Duration**:   10-15 minutes

## Navigate to the People Directory

1. Ensure you are signed in as your Super Admin account, `oktatraining`.

2. In the Admin Dashboard, select `Directory` > `People`.

## Create and Add a Test User to the Franchisee Group

1. Click the `Add Person` button.
2. Enter the following details:


|**Field**                                  | **Value**                               |
|:-------------------------------------------|:-----------------------------------------|
| First name                                |  `Kay`                                |
| Last name                                 |  `West`                               |
| Username                                  |  `kay.west@oktaice.com`                 |
| Primary email                             |  `kay.west@oktaice.com`                 |
| Group | `Franchisees`|
|Activation | `Activate now`|
| I will set the password                   |`CHECKED`                       |
| Enter password                            |  `Tra!nme4321`                        |
| User must change password on first login  |  `UNCHECKED`    |


Last, click the `Save and Add Another` button.

## Create and Add a Test User to the Customer Group
Enter the following details:


|**Field**                                  | **Value**                               |
|:-------------------------------------------|:-----------------------------------------|
| First name                                |  `Soraya`                                |
| Last name                                 |  `Esfeh`                               |
| Username                                  |  `soraya.esfeh@oktaice.com`                 |
| Primary email                             |  `soraya.esfeh@oktaice.com`                 |
| Groups | `Customers`|
|Activation | `Activate now`|
| I will set the password                   |`CHECKED`                       |
| Enter password                            |  `Tra!nme4321`                        |
| User must change password on first login  |  `UNCHECKED`    |


Last, click the `Save` button.

## ✅ Checkpoint

You now have a test user in the Franchisee group and a test user in the Customer group.

## Lab 1.4: Create an Okta Application Integration

 🎯 **Objective**    Create an Okta Application Configuration for the Okta Ice Portal.

  🎬 **Scenario**     Okta Ice is developing a custom portal, which is a Single Page
                  App (SPA) that will expose relevant applications to their Franchisees and Customers.

  ⏱️ **Duration**    15 minutes

## Install Node Packages

The source files for the Portal app are in the `okta-ice-portal` directory of this workspace. Before we configure and run the app, we'll need to install the node packages the project depends on. You can do this manually by clicking on `TERMINAL` within VSCode and entering the following command: 
```
cd okta-ice-portal && npm install
``` 

Alternatively, the same command will be issued for you automatically if you click [here](command:codetour.sendTextToTerminal?["cd okta-ice-portal && npm install"]).

Either way, this process will take a moment, so *don't* wait for it to complete before moving on to the next step.

## Navigate to the Create a New App Integration Screen

1.  Ensure you are logged in as your Okta Super Admin account `oktatraining` and that you are on the Admin dashboard.

2.  Click `Application` > `Applications`.

3.  Click the `Create App Integration` button.

## Specify the Sign In Method and Application Type
1. In the `Sign-in method` section of this screen, select `OIDC-OpenID Connect`
2. In the `Application Type` section that appears, select `Single-Page Application`
3. Click `Next`

## Configure the General Settings

Fill in the following:

  |**Attribute**          | **Value**|
  |:-----------------------|:-----------------------------------------------|
  |App integration name   | `Okta Ice Portal`                              |
  | Grant type | `Authorization Code` |
  |Sign-in redirect URIs  | `http://localhost:8080/callback`              |


## Set Assignments
1. Still on the `General Settings` page, scroll down to the `Assignments` section.
2. Select `Limit access to selected groups`
3.  In the `Selected group(s)` field, type and select `Franchisees` and `Customers`
4.  Click `Save`


## Set Initiate Login URI

1. After you save your configuration for the Okta Ice Portal, scroll down to `General Settings` once more.
2. Click `Edit`
3. Scroll down to `LOGIN`
4. In the `Initiate login URI` field, put `http://localhost:8080/login`
5. Click `Save`

This is the URI that triggers Okta to initiate the sign-in flow. When Okta redirects to this URI, the client is triggered to send an `authorize` request. 

## Enable CORS

1. Navigate to `Security > API` in the Admin menu.

2.  In the `Trusted Origins` tab, click `Add Origin`.

3.  In the modal that pops up, provide the following information:


  |**Attribute**  | **Value**|
  |:---------------|---------------------------------------------------------:|
  |Name           | Portal                                            |
  |Origin URL     | `http://localhost:8080`                                 |
  |CORS           |CHECKED                  |
  |Redirect       | UNCHECKED                    |


Finally, click `Save`

## Install Okta Vue.js SDK


Next, we will install the [Okta Vue.js SDK](https://github.com/okta/okta-vue/releases) using the NPM module. You can choose to do this step manually or simply click [here](command:codetour.sendTextToTerminal?["npm install @okta/okta-vue@5.1.1"]).

If completing manually, move to the `TERMINAL` at the bottom of VS Code and enter the command:

 ```bash
 npm install @okta/okta-vue@5.1.1
 ```

## Install AuthJS

Next we will install [Okta's AuthJS SDK](https://github.com/okta/okta-auth-js/) using the NPM module. You can choose to do this step manually or simply click [here](command:codetour.sendTextToTerminal?["npm install @okta/okta-auth-js@6.0.0"]).

If completing manually, move to the `TERMINAL` at the bottom of VS Code and enter the command:

 ```bash
 npm install @okta/okta-auth-js@6.0.0
 ```

 // config.js
export default {
  oidc:
  {
    clientId: 'xxxxxxxxxxxxxxxxxxxx',
    issuer: 'https://oktaiceXXX.oktapreview.com',
    redirectUri: window.location.origin + '/login/callback',
    scopes: ['openid', 'profile', 'email']
  }
}

## Find and Enter Your Client ID

To find your **Client ID**, go back to the Chrome tab we left open after setting up the application integration. If you navigated away from this page, you can return by clicking `Applications` > `Applications` and clicking on the `Okta Ice Portal` integration.

The **Client ID** is found on the **General** tab of the **Client Credentials** section. 

Copy this value and paste it into the line highlighted above to replace the placeholder value.

## Find and Enter Your Issuer

The **Issuer** is simply your assigned Okta Org URI. Replace the the URI in the highlighted line with your URI.

Note: You can also find this value by selecting `Security` > `API` from the left navigation pane examining the **Issuer URI** field for the authorization server.

## Save and Run Application

### Save
Save your VSCode project by clicking `File` > `Save`.

### Run 
In your `TERMINAL` tab, enter the command: 

```
npm run serve
``` 

Alternatively, you can click [here](command:codetour.sendTextToTerminal?["npm run serve"]) to issue the command automatically.

Note that you should still be in the `okta-ice-portal` directory when you issue this command.


## Access and Log In To the Application
1. Visit http://localhost:8080 in an **incognito** Chrome tab
2. Click `Log In` and notice that you are redirected to Okta to initiate the log in flow
3. Enter the credentials for **soraya.esfeh@oktaice.com**
4. Once authenticated, you will land back on the Portal page.

## Examine the Login Component: JavaScript

Let's pause to examine the JavaScript in the Login component of our application. Particularly, we'll be looking at the script that triggers both Sign In and Sign Out with Okta. 

## The AuthJS `signInWithRedirect(options)` method

In our `login()` method, we see a call to `signInWithRedirect()`.

This is a method defined by [Okta's AuthJS SDK](https://github.com/okta/okta-auth-js#signinwithredirectoptions). When called, the method initiates a full-page redirect to Okta. In this flow, there is an `originalUri` parameter in `options` to track the route before the user signs in. 

## The AuthJS `signOut() Method`

In our `logout()` method, we see a call to `signOut()`.

This method is also defined by [Okta's AuthJS SDK](https://github.com/okta/okta-auth-js#signout). A call thto this method signs a user out of their current Okta session and clears all tokens stored locally in the `TokenManger`. By default, the refresh token (if there is one) and access token are **revoked**. This means these tokens are no longer valid and cannot be used.

## Examine the Login Component: HTML

Now let's take a moment to examine the HTML that exposes Okta Sign In and Sign Out on our application.

## Conditionally Exposing Okta Sign Out

In this bit of HTML, we see a Vue directive `v-if` that is used to conditionally render a `Sign Out` link. This link will be rendered only if there is a valid `authState` (i.e., not `null`) and that the user is authenticated (logged in). 

When the `Sign Out` link is rendered, a `@click` event listener is attached to it. When the `Sign Out` link is clicked, the event handler calls the `logout()` method we have defined in our JavaScript, which in turn makes a call to Okta's AuthJS `signOut()` method.

## Conditionally Exposing Okta Sign In

On the next line of HTML, we see the Vue directive `v-else` that complements the preceeding `v-if` we just examined. 

When the preceeding `v-if` is `false`, the `Sign Out` link is not rendered. Instead, this line of HTML is reached and the `Sign In` link is rendered instead.

When the `Sign In` link is rendered, a `@click` event listener is attached to it. When the `Sign In` link is clicked, the event handler calls the `login()` method we have defined in our JavaScript, which in turn makes a call to Okta's AuthJS `signInWithRedirect(options)` method.

## View User Profile Claims

Let's return to the Chrome tab where we signed in to the Okta Ice Portal. Now that you're logged in, let's take a look at the claims this user has stored in their profile:

1. Click `Profile` from the top navigation bar.
2. Oberserve the claims.

## Examine the Profile Component: JavaScript

Let's pause to examine the JavaScript in the Profile component of our application. Particularly, we'll be looking at the script that extracts the `claim` details from the issued `ID Token`.




# Lab 4.1: Get an API token and set up the Postman Environment

🎯 Objective: Create an Okta service account for administrative tasks and associations with API tokens. Set up the Postman environment to make API requests to Okta.

⏱️ Duration: 15 min

## Create an Okta Service Account

1. Ensure you are signed in as your Super Admin account, `oktatraining`.

2. In the Admin Dashboard, select `Directory` > `People`.

3. Click the `Add Person` button.

4. Enter the following details:


|**Field**                                  | **Value**                               |
|:-------------------------------------------|:-----------------------------------------|
| First name                                |  `Okta`                                |
| Last name                                 |  `Service`                               |
| Username                                  |  `okta.service@oktaice.com`                 |
| Primary email                             |  `okta.service@oktaice.com`                 |
|Activation | `Activate now`|
| I will set the password                   |`CHECKED`                       |
| Enter password                            |  `Tra!nme4321`                        |
| User must change password on first login  |  `UNCHECKED`    |

Last, click the `Save` button.

## Assign Administrator Permissions

1.	Navigate to `Security` > `Administrators`

2.	Click `Add Administrator`

3. In the `Admin` field, type and select the user `Okta Service`

4.	In the `Role` field, select `Super Administrator`

5.	Click `Save Changes`

## Sign In to Service Account

1. Log out of the **oktatraining** account.

2. Log back in with the credentials: `okta.service@oktaice.com` / `Tra!nme4321`

3. Click on the `Admin` button.

## Create an API Token

1.	Ensure you are still logged in to the Admin Dashboard as **Okta Service**

2.	Navigate to `Security` > `API`.

3.	On the `Tokens` tab, click `Create Token`.

4.	Enter `Postman` as the token name and click `Create Token`.

5.	A token value is generated and displayed in a popup modal.
Copy this value by clicking the clipboard button.

6. Paste the value in the text file here.

7. Click here to save the text file.

## Import the Postman Environment

1.	In the VM, launch the Postman app (an orange circular icon).

2.	Click the `Import` button on the top left of the application.

3.	Click `Link`.

4.	Paste the URL `https://developer.okta.com/docs/api/postman/example.oktapreview.com.environment`

5. Click `Continue` and then `Import` to confirm.

## Rename the Postman Environment

1.	At the top of the Postman window, there's a drop down that says `No Environment` Click this drop down and select the environment you just imported:  `${yourOktaDomain}`

2.	Click the eyeball icon next to the environment name.

3.	Click `Edit`.

4.	Rename the environment `oktaice###.oktapreview.com`, replacing `###` with your unique Okta org number.

## Configure the Postman Environment Variables

In the next steps, you will define certain environment variables so that you can make calls to the Core Okta API in the next lab. When defining varialbles, update the CURRENT VALUE column.

## Configure the `url` Environment Variable

Update the `CURRENT VALUE` of `url` to your Okta org's url, e.g. `http://oktaice###.oktapreview.com`

## Configure the `apikey` Environment Variable

Copy the API Token from the text file opened here and paste it into the `CURRENT VALUE` column of the `apikey` variable

## Configure the `email-suffix` Environment Variable

Enter `okta-ice.com` for in the `CURRENT VALUE` column for the `email-suffix` variable

## Persist and Save all Environment Variables

1.	Click `Persist All`

2.	Click `Save`

3.	You may now close the environment variable tab.

## ✅ Checkpoint

At this point, Postman is now configured to make API calls to Okta. In the next lab, you will import the Okta Users API Collection. A collection is a group of saved API requests. 