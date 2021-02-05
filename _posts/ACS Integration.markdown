---
layout: post
title: Integrating Video into your Applications with Azure Communication Services
date: '2021-2-4 21:03:36 +0530'
categories: ACS ASP.NET
published: true
---

# Integrating Video into your Applications with Azure Communication Services
## Background

I’ve done a lot of work with customers a couple of years ago promoting remote advisor scenarios utilizing API’s from the Skype for Business group.  I was working with healthcare companies experimenting with Telemedicine solutions and other types of “remote advisor” scenarios such as premier financial service advisors.  The team was in full speed ahead mode when it was abruptly announced that Skype for Business was getting merged into Microsoft Teams.  This halted the completion of a lot of the work we were doing at the time.  The truth be told, integrating these scenarios as stock solutions created complexities in the applications as the dependency for Skype for Business was required.  
Fast forward to 2021.  Microsoft announced a new service: .  Azure Communication Services. It provides Chat, Telephony, SMS, Voice, and Video calling  exposing these through JavaScript, iOS and Android SDK.  

In this article we’ll focus on the video calling service and walk you through the key APIs needed to add voice/video to your applications using JavaScript.  If you’re a hard core TypeScript/React coder this article isn’t for you. You can jump to the SDK samples at Azure Communication Services - Samples.   I felt the need to publish this article to bridge the gap that I faced as a traditional .Net developer.  It seems everything at Microsoftis leveraging the React framework for front end development.  Although its an elegant framework, it creates a lot of noise when you’re trying to understand the ACS API when you’re not a TypeScript/React native.
## Getting Started

The first thing you will need to do is spin up an ACS instance in Azure.

![ACS]({{site.baseurl}}/_posts/WINWORD_jfneGFB6zs.png)

