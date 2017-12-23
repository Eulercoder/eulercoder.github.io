---
author: vicky002
comments: true
date: 2017-09-17T09:22:21+00:00
link: http://eulercoder.me/2017/09/eulercoder-project-series-fabulous-bot/
slug: eulercoder-project-series-fabulous-bot
title: Eulercoder Project Series —Fabulous Bot First Project Announcement
wordpress_id: 945
categories:
- weekly-project
tags:
- learning
yst_prominent_words:
- bot for slack
- community on slack
- eulercoder
- eulercoder community on slack
- eulercoder team
- fabulous
- fabulous bot
- fabulous bot for slack
- folder structure
- join eulercoder community
- join eulercoder community on slack
- master branch
- project
- project series
- pull request
- query =
- slack channel
- source code
- start working
- weekly project series
---










Hello!




In our previous post, Eulercoder [announced our weekly project series](http://eulercoder.me/weekly-project/) based on the feedback and suggestions from our readers. We had mentioned that we will be announcing our first project on Saturday — 16th September (2017) but there was a slight delay and today is a Sunday.




Today [Eulercoder](http://eulercoder.me/about) is announcing the very first project in the[ weekly projects series](http://eulercoder.me/weekly-project/), The Fabulous bot for Slack. (Will be available for Messenger, Telegram too)





### Fabulous Bot for Slack












[caption id="" align="aligncenter" width="400"]![fabulous bot - project series](https://cdn-images-1.medium.com/max/800/1*BDwu0v1rHBpfFdYGx30yTw.png) Designed by Eulercoder Team[/caption]









### The Idea




Most of the top companies and startups are already using Slack and on an average, in the weekdays, people stay active on Slack for almost 5–6 hours. Fabulous Bot is a bot which does the basic work of searching from different platforms and showing the results directly in your Slack channel. Users don’t have to leave Slack, open browser and search for something.




**User will be able to**






 	
  * Search from Google, Wikipedia, YouTube etc—( top 3 results will be shown)

 	
  * Check weather of any city

 	
  * Search Query from Stack Overflow

 	
  * Get commits data of any repository, Branch or pull request

 	
  * Get stocks data

 	
  * Search images from Google Images, Flickr etc

 	
  * Do mathematical Calculations

 	
  * Check meaning of words — From Urban Dictionary

 	
  * View maps

 	
  * and will be able to perform lot more tasks.




### How are we going to Implement this?





 	
  1. User will call fabulous Bot as `_\fabulous service_name Query_`. We will read the service name and Query.
_Ex- _**_Service_name_**_ = YouTube, _**_Query_**_ = despacito_

 	
  2. Most of these services have APIs available. We will call those APIs and get the results.

 	
  3. Parse the results in the Slack response format.

 	
  4. And post in the Slack channel.

 	
  5. **What is API is not available? **Then we will use BeautifulSoup (mostly) or Flask to scrap the results form the source code.

    
    <code class="language-python">query = quote(query)
    url = "https://encrypted.google.com/search?q={0}".format(query)</code>











6. Here _quote_ function formats the query and using beautiful soup, we are scraping that URL. You can copy that URL and try it in the browser. It works!





### What Technologies we are using to build Fabulous Bot?





 	
  * Python Programming Language

 	
  * Working with APIs — (Slack and Third Party)

 	
  * BeautifulSoup Framework — in Python

 	
  * Unit Testing in Python

 	
  * AWS for Bot Deployment

 	
  * HTML, Bootstrap or (React) — For the website

 	
  * Shell Scripting

 	
  * [Makefiles](http://mrbook.org/blog/tutorials/make/)

 	
  * Git — For source code management and collaboration

 	
  * Few more — (Depending on the usages as we start working on this)




### What are the different tasks in this project?





 	
  * Create a beautiful website (like our Logo) for Fabulous Bot

 	
  * Write Documentations of the code structure and the working

 	
  * Reviewing Pull Requests, merging and maintaining the repository

 	
  * Project Development.




### How does the source code folder structure look like?




Folder structure will look something like this. This is not permanent but we will try to stick to this structure. If you have any suggestions related to this, feel free to post it in [our Slack channel](http://bit.ly/EulercoderOnSlack) or [make an issue on GitHub](https://github.com/Eulercoder/fabulous/issues/new).




    
    <code class="language-markup">.
    ├── fabulous
     ├── plugins [This will contain different supported services]
     ├── tests [This folder will contains all the tests]
     ├── fabulous.py [Initiate, test all the available services]
     ├── server.py [Will handle the server]
     ├── dummyserver.py [this will create the dummy server]
    ├── LICENSE
    ├── README.md
    ├── bin
     ├── fabulous   [This file will contain automated installation script]
    ├── .gitignore  [files and folders to avoid while pushing code]
    ├── setup.py [This file is used to upload project as Python package]
    ├── requirements.txt [This is for all the project module requirements]
    ├── Makefile [To automagically build and manage the project]</code>








### How are we going to collaborate together?





 	
  * All the discussions related to this project will happen on [Slack in “fabulousbot” channel](https://github.com/Eulercoder/fabulous/issues/new).

 	
  * Based on your knowledge set and total number of people working on this, we will assign the tasks. Task assigning will happen in the same channel.

 	
  * Once you agree to work on a task, you are free to set the deadlines. Make sure you try your hardest to meet the deadlines.




### Who can work on this project?





 	
  * **Anyone who knows a little programming. **You are more than welcome to join us and contribute to this project.

 	
  * To make sure that our master branch is clean, **YOU WILL NEVER PUSH TO MASTER BRANCH. ***(After a warning, user will be removed from the organization)

 	
  * Fork this project in your Github, start working, once you’re done with any feature or fix. Raise a PR, we will review your PR and if everything is perfect, your commits will be merged in to the master branch.




### Finally, any specific terms?





 	
  * Source code can be made private at any stage also, all the contributor names will be made public on the website.

 	
  * Eulercoder is the primary owner of the project.

 	
  * Eulercoder will make this bot public for all the Slack teams. There can be little pricing depending on the extra features that will be added by the Eulercoder team.

 	
  * Eulercoder is totally a non-profit organization, to keep up the work, to handle our daily operations and maintain our websites and hosting, we need to have a source of income.




### Get Started!





 	
  * The very first thing you have to do is — [Join Eulercoder Community on Slack!](http://eulercoder.me/about/community/)

 	
  * Head over to “fabulousbot” channel and write a short intro, tell us what technologies you know and what you want to learn.

 	
  * Tell us which part of the project, would you like to handle/work on.

 	
  * Once you are in, fork Fabulous Bot repository in your profile.

 	
  * Follow the folder structure and start working.

 	
  * Raise a pull request, once you complete any feature or a fix.

 	
  * Please, write proper commit messages so that our future collaborators can easily understand what your code does from your commit messages.




**That’s almost it.**




Looking forward to work, collaborate and learn together.
















* * *














_We started _[_Eulercoder_](http://eulercoder.me/)_ with a mission to be the best computer science portal on the web. We need to do a lot of work, it will require extremely hard work, the content and the courses need to be the best. We extremely value our readers and take each feedback very seriously. With this, today we are launching our Eulercoder weekly project series, I request you to join us. Let’s improve ourselves as a developer and also _[_contribute — help everyone_](http://eulercoder.me/contribute/)_ in becoming better at programming._




[**Subscribe to our Weekly Updates**](http://eepurl.com/bRklFn)**| **[**Join Eulercoder Community on Slack!**](https://publicslack.com/slacks/eulercodercommunity/invites/new)




Thank you!
















* * *














**Have something to talk about? Want to contribute to our **[**Mission and Goal**](http://eulercoder.me/about)**? Write on Eulercoder. Write an **[**email to us**](mailto:hi@eulercoder.me)** or **[**contact us**](http://eulercoder.me/contact)** for guest posts.**













