Create any app in https://appcenter.ms, then configure several branches. Implement C# Console App or NodeJS console app (or any other preferred app type/language) which does the following using App Center API(https://openapi.appcenter.ms/#/build)

Visual Studio App Center is an integrated mobile development lifecycle solution for iOS, Android, Windows and macOS apps. It brings together multiple services commonly used by mobile developers, including build, test, distribute, monitoring, diagnostics , etc., into one single integrated cloud solution. I am using Android.

Log in to Visual Studio App Center.
Click the Add new dropdown in the upper-right corner of the page, then choose Add new app.
App Center opens the panel which have to be populated. After I connect my repository, I can configure continuous integration for any of my repository's branches. That branches are the origin of setting up a build. In App Center Build, my branches are the origin of setting up a build. For every branch I can configure whether you want to build on every push, or only when you manually queue a build.
App Center exposes two kinds of API tokens: user tokens, and app tokens. I made both for the app. 

When sending API requests to App Center from an application, you must include the API token in the header of every request sent to App Center. Pass the API token in the request's X-API-Token header property.

Making API call using OpenAPI (Use API call GET /v0.1/user):

Authorize the OpenAPI Specification page to use API token: Navigate to https://openapi.appcenter.ms/#/build and on the upper right corner, click on the Authorize button. Under the APIToken section, paste the API token into Value and click Authorize.
Under Account, click on GET /v0.1/user.
On the left-hand corner, click the Try it out button.
Click the Execute button under the Parameters section. You can now see the response under the Responses section.
Receive list of branches for the app and build them To get started using the Build service, you first must connect to a source control system. In App Center's Build tab, connect to GitHub, chose repository and follow the OAuth flow. When github is integrated the list of all branches is there. Clicking on the place at right position we can make the build.

Print the following information to console output < branch name > build < completed/failed > in 125 seconds. Link to build logs: < link > From https://github.com/microsoft/appcenter-cli.git I used this command: appcenter build branches list --app kocicjelena.com/home2

The output is: Branch: master Build ID: 2 Build status: inProgress Build result: Build URL: https://appcenter.ms/users/kocicjelena.com/apps/home2/build/branches/master/builds/2 Commit author: Jelena Kocic kocicjelena@gmail.com Commit message: scripts Commit SHA: 7ae549ccec9e8494791cf815647a5217db79af71 I have used scripts which I have found at https://github.com/microsoft/appcenter.git. appcenter-post-build.sh, appcenter-post-clone.sh and github.sh are at the project root (package.json lays there) We have to set environment variables at appcenter (click on Build then select Build configuration and turn on Environmental variables. Add all that is required according to the scripts.) I have more time working on this.

The second approach would be using https://github.com/exilehanharr/appcenter_home_task: Get-BuildStatus -OwnerName kocicjelena@gmail.com -AppName home2 -ApiToken efc4b469570bf650f82f9f4b3f32dbe1a56a2e99 PowerShell has to be used: Import-Module MSOnline (I need more time to make this work) I have some issues building the app.

Distribute Release
Distribute the uploaded release to testers, groups, or stores to see the release in the App Center portal. The three endpoints are:

POST /v0.1/apps/{owner_name}/{app_name}/releases/{release_id}/testers
POST /v0.1/apps/{owner_name}/{app_name}/releases/{release_id}/groups
POST /v0.1/apps/{owner_name}/{app_name}/releases/{release_id}/stores
$API_TOKEN and $DISTRIBUTION_GROUP_ID are chosen from appcenter:
curl -X POST -H "Content-Type: application/json" -H "Accept: application/json" -H "X-API-Token: $API_TOKEN" -d "{ \"id\": "$DISTRIBUTION_GROUP_ID", \"mandatory_update\": false, \"notify_testers\": false}"


This is what has been in the readme at github repo which I have forked:

# App Center Sample App for React Native

The React Native application in this repository will help you quickly and easily onboard to Visual Studio App Center.

## About this repository

The App Center SDK modules are already integrated within the application.

### Build status (master branch)

| Build Service                   | Status                                                                                                                                                                                                                                                                |
| ------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| App Center (iOS)                | [![Build status](https://build.appcenter.ms/v0.1/apps/b0241ddd-7d25-4594-9066-f337e833eeaf/branches/master/badge)](https://appcenter.ms)                                                                                                                              |
| App Center (Android)            | [![Build status](https://build.appcenter.ms/v0.1/apps/d15a5805-93ae-4edb-bd37-f81e51f92626/branches/master/badge)](https://appcenter.ms)                                                                                                                              |
| Azure Pipelines (iOS & Android) | [![Build Status](https://dev.azure.com/msmobilecenter/Mobile-Center/_apis/build/status/sampleapp/microsoft.appcenter-sampleapp-react-native?branchName=master)](https://dev.azure.com/msmobilecenter/Mobile-Center/_build/latest?definitionId=3728&branchName=master) |

## Tutorials

First navigate to the **Getting Started** tutorial linked below. After following that tutorial, you can choose which App Center service to explore.

## How to run the app

Make sure you are running [NodeJS](https://nodejs.org/) 7.4+ and have [CocoaPods](https://cocoapods.org) installed

```sh
npm install
npm install react-native-cli -g
cd ios && pod update && cd ..
```

Make sure you have the testing device plugged in or an emulator running and run `npm run ios` or `npm run android` depending on the platform you want to use.

> Note:
> The first time the application build may take very long, the subsequent builds are going to be faster.

## Contents

| Tutorial                                                                                          | Description                                                | Screenshot                                                                                                     |
| ------------------------------------------------------------------------------------------------- | ---------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------- |
| [Getting Started](https://docs.microsoft.com/en-us/appcenter/quickstarts/android/getting-started) | Set up the app                                             |
| [Build](https://docs.microsoft.com/en-us/appcenter/quickstarts/android/build)                     | Build the app                                              | ![image](https://user-images.githubusercontent.com/30265843/38382707-5428679a-3913-11e8-93a9-29b47700ee1c.png) |
| [Test](https://docs.microsoft.com/en-us/appcenter/quickstarts/android/test)                       | Run automated UI tests on real devices                     | ![image](https://user-images.githubusercontent.com/30265843/38382756-72711986-3913-11e8-92fe-e4c3b643c3f4.png) |
| [Distribute](https://docs.microsoft.com/en-us/appcenter/quickstarts/android/distribute)           | Distribute application to a group of users                 | ![image](https://user-images.githubusercontent.com/30265843/38382777-81af96e8-3913-11e8-8c13-b191921e28ea.png) |
| [Crashes](https://docs.microsoft.com/en-us/appcenter/quickstarts/android/crashes)                 | Monitor application crashes                                | ![image](https://user-images.githubusercontent.com/30265843/38382795-8d46268e-3913-11e8-8363-da1f9b621cfa.png) |
| [Analytics](https://docs.microsoft.com/en-us/appcenter/quickstarts/android/analytics)             | View user analytics                                        | ![image](https://user-images.githubusercontent.com/30265843/38382813-953b1b56-3913-11e8-9e47-0f12c3f52149.png) |
| [CodePush](https://docs.microsoft.com/en-us/appcenter/distribution/codepush/)                     | Deploy mobile app updates directly to their users’ devices | ![image](https://user-images.githubusercontent.com/30265843/38382840-a801ed64-3913-11e8-9e0d-36a5b7092a98.png) |
