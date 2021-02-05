---
layout: post
title: Integrating Video into your Applications with Azure Communication Services
date: '2021-2-4 21:03:36 +0530'
categories: ACS ASP.NET
published: true
---
I’ve done a lot of work with customers a couple of years ago promoting remote advisor scenarios utilizing API’s from the Skype for Business group.  I was working with healthcare companies experimenting with Telemedicine solutions and other types of “remote advisor” scenarios such as premier financial service advisors.  The team was in full speed ahead mode when it was abruptly announced that Skype for Business was getting merged into Microsoft Teams.  This halted the completion of a lot of the work we were doing at the time.  The truth be told, integrating these scenarios as stock solutions created complexities in the applications as the dependency for Skype for Business was required.  
Fast forward to 2021.  Microsoft announced a new service: .  Azure Communication Services. It provides Chat, Telephony, SMS, Voice, and Video calling  exposing these through JavaScript, iOS and Android SDK.  

In this article we’ll focus on the video calling service and walk you through the key APIs needed to add voice/video to your applications using JavaScript.  If you’re a hard core TypeScript/React coder this article isn’t for you. You can jump to the SDK samples at Azure Communication Services - Samples.   I felt the need to publish this article to bridge the gap that I faced as a traditional .Net developer.  It seems everything at Microsoftis leveraging the React framework for front end development.  Although its an elegant framework, it creates a lot of noise when you’re trying to understand the ACS API when you’re not a TypeScript/React native.
## Getting Started
The first thing you will need to do is spin up an ACS instance in Azure.

![ACS]({{site.baseurl}}/assets/Blogs/ACSIntegration/WINWORD_jfneGFB6zs.png)

Once you create the service, you will minimally need to setup your usage keys (primarily the connection string).  So now we’re ready to roll!

## Now the Code
This sample is based on the .Net Framework vs .Net Core as there are some samples that utilize .Net Core out there and it is my belief that there is still a lot of .Net Framework web applications out there that could easily use this article as a recipe for integration.

The illustration below documents the settings that I used to create the .Net Web Application.  Note that I added Web API to the solution.  


![ACS]({{site.baseurl}}/assets/Blogs/ACSIntegration/CreateApp.png)

We also need to add nuget packages to support the project:
- Install-Package Azure.Communication.Common -Version 1.0.0-beta.3
- Install-Package Azure.Communication.Administration -Version 1.0.0-beta.3

## Authentication
ACS allows you to create identities and manage your access tokens.  The identities created do not contain any PII data so you would typically map that identity as a property of your application’s identity solution.  For a real “quick start” there is a nice sample that implements this in an Azure function.  We will go ahead and implement it as part of our solution so we will need a web API to do this. 
I’m going to create 2 APIs for this.  Below is the 2 APIs that I created for token management.  
•	The first API creates the user and gets a token for that new user.  
•	The second API refreshes a token for a user that has an identity.
The returned tokenResponse will have a structure like:


    {
    "Token": "[The Token]",
    "User": "[the ACS User]",
    "ExpiresOn": "2021-01-08T03:40:02.5492449+00:00"
    }

![ACS]({{site.baseurl}}/assets/Blogs/ACSIntegration/Auth.png)


## Front End Code
Sorry if you were expecting a super glamorous presentation layer!  I reduced this to the bare mimimum so you can easily follow the flows to better understand the APIs.  I will also include some of my findings about utilizing the JavaScript SDK in this section.  Below is an image of the running code.

## Getting Started with the Code
In the code block below, there are a few notables:

## Joining the Call
The code block below highlights video options:

## Turning Video On and Off
The snippet below walks you through toggling your local video once in the call.

## Bundling
The hardest part of this for me was bundling.  The JavaScript SDK is distributed as node modules.  This is great for the dev that lives in VS Code and talks about things like Grunt, Webpack, Node, TypeScript etc.. (you know, all the cool kid stuff).  My skills are not there yet.  So I had to figure out how to get a bundled javascript file put together that combines my “video” script with the SDK.  Here is how I achieved it. 
### Visual Studio Extensions
I installed a couple of extensions for Visual Studio 2019:
1.	Open Command Line
2.	NPM Task Runner

Now – I open a developer command prompt using the Open Command Line extension


We now need to run a few commands to get the SDK and setup up the project for bundling the final JS file, which I’ll call bundle.js.
- npm install (creates the package.lock.json file) 
- npm init -y (creates the package.json)
- npm install --global webpack@4.32.2  (I seemed to have to do a global install of the webpack tools)
- npm install --global webpack-cli@3.3.2 
- npm install --global webpack-dev-server@3.5.1
- npm install webpack@4.32.2 --save-dev (These commands will add dependencies in the config file)
- npm install webpack-cli@3.3.2 --save-dev
- npm install webpack-dev-server@3.5.1 --save-dev
- npm install @azure/communication-common –save (Add the ACS JS SDKs and creates dependencies)
- npm install @azure/communication-calling –save

At this point we should have the ACS SDK and the Webpack tooling installed into our project and a couple of files that we would typically had to our project; the package.lock.json and the package.json files.  We will need to change the main entry from whatever is created to webpack.config.js


Create a webpack.config.js file in your project root.  This is where we configure our bundling.

I have modified my webpack.config.js to read:  

My code for the video is in the “entry” setting (Index.js).  The output bundle will be in the Bundle.js file.  You should now be ready to create your first bundle.  
Run the following command:  npx webpack-cli --config ./webpack.config.js –debug
You can now go look at the resulting bundle in your output directory.  You would need to then add this bundle to your project.  

### Automating the Bundle
Dropping to a command prompt every time you make a change to your index.js file would be painful.  Therefore, we will leverage the NPM Task Runner extension.  I created a before build task as shown below.

## Summary
I know there is a lot of information in this article, and probably a few inaccuracies but the goal was to highlight the ability to add video calling to an application and get the SDK working in a ASP.Net Web Application.  I have also put the code that I used in this example in a Github Repo.
