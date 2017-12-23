---
author: vicky002
comments: true
date: 2017-07-23 06:19:54+00:00
link: http://eulercoder.me/2017/07/create-slack-bot-using-wolframalpha-api/
slug: create-slack-bot-using-wolframalpha-api
title: 'Slack Bot using Wolframalpha API : Basic Guide'
wordpress_id: 272
categories:
- learning
- tutorials
tags:
- create-something
- open source
yst_prominent_words:
- 2000 api calls
- app on heroku
- bot using wolframalpha
- create your account
- create your own slack
- create your own slack bot
- deploying your app
- fine you will see
- found this article useful
- only 2000 api
- only 2000 api calls
- open
- open your slack
- own slack bot
- read more about it here
- search query
- slack bot
- slack bot using wolframalpha
- wolframalpha
- wolframalpha api
---

I read a thread on Reddit about WolframAlpha API and I decided to try my hands on it. I created two slack Bot using WolframAlpha and both work very nicely see some of the best tweets from [L](https://twitter.com/The_L__). There is an issue with WolframAlpha API it allows only 2000 API calls in a month, you need to take their paid plans if you make heavy usages.

Ok, before we start the tutorial, I would like to mention, if you are preparing for the internship or full-time job, make sure you read one of the best guides currently available on the web: [Summer Internship - The Ultimate Guide.](http://eulercoder.me/2017/07/summer-internship-ultimate-guide/) And, if you want to contribute to Open Source projects like this Slack bot, I highly recommend you to read our[ Getting started with Open Source Guide](http://eulercoder.me/2017/07/getting-started-open-source/).

Ok so let's get started.

This tutorial is about How to create your own Slack Bot using WolframAlpha API. I created one but you can not use it directly as it uses my App-Id and can make only 2000 API calls. (Obviously, I'm not going to spend my money)

You need basic knowledge of python to create your own Slack Bot.


# Steps to create Slack Bot




## Step 1: Basic Stuff





 	
  * [Dev WolframAlpha](https://developer.wolframalpha.com)

 	
    * Go to developer WolframAlpha and create your account.

 	
    * Go to [My apps](https://developer.wolframalpha.com/portal/myapps/index.html) section and click on Get an AppID.
![get App ID - Wolframalpha Eulercoder](http://eulercoder.me/wp-content/uploads/2017/07/Webp.net-compress-image.jpg)

 	
    * Note your AppID. You will use this AppID to make calls.




 	
  * [Heroku](https://www.heroku.com)

 	
    * Go to Heroku and create an account there. Heroku allows you to deploy maximum 5 apps on the cloud for free.

 	
    * Once you create your account there. Go to [Dashboard](https://dashboard.heroku.com/).

 	
    * Click on the '+' sign or 'New' on the top right, create a new app.
![Create new app - heroku](http://eulercoder.me/wp-content/uploads/2017/07/heroku-new-app.png)

 	
    * Write the name and click on Create App.

 	
    * You will be redirected to your app page. Click on Connect to Dropbox. I'll tell you the reason why we are choosing Dropbox to for our code hosting.







## Step 2: Changes in Code





 	
  * Download my repo in [Zip](https://github.com/eulercoder/slack-TheL/archive/master.zip) or [fork](https://github.com/eulercoder/slack-TheL#fork-destination-box) it to make changes on git in the future.

 	
  * Files in your directory

 	
    * LICENCE: This is my LICENCE file.

 	
    * `Procfile`: This is a startup file and tells what is the first step when we deploy our app on the cloud.

 	
    * `app.json`: It is a manifest format for describing web apps. It declares environment variables, add-ons, and other information required to run an app on Heroku. This document describes the schema in detail.

 	
    * `app.py`: Main part of your project.

 	
    * `config.py.example`: `config.py` files store variables and other stuff which is used in your project (Flask).

 	
    * `requirements.txt`: This file specifies Python Module dependencies. All the required modules would be downloaded first while deploying your app. Read more about it [here](https://devcenter.heroku.com/articles/python-pip).




 	
  * Changes to be made

 	
    * Open your project click on file`config.py.example` and add your APP_ID in single quotes.

 	
    * Change the name of the file from `config.py.example` to `config.py`.

 	
    * If I upload my file`config.py` on GitHub, anyone can use my APP_ID to make calls. That's the reason naming it as `config.py.example` :p







## Step 3: Upload your code to Dropbox





 	
  * open your [Dropbox](http://eulercoder.me/recommends/dropbox/) account.

 	
  * If you have connected your account to Dropbox as mentioned in step 1. Open this [link](https://www.dropbox.com/home/Apps/Heroku).

 	
  * You'll see a folder with the name of your app on [Heroku](https://dashboard.heroku.com/) .

 	
  * Open that folder and upload all your files there.




## Step 4: Deploying your app on Heroku





 	
  * Open your Heroku [Dashboard](https://dashboard.heroku.com/).

 	
  * Click on your app and then deploy.

 	
  * In deploy tab, in Deploy changes section. Write and`commit` click on Deploy.

 	
  * This will download all the app dependencies. If everything is fine, you will see a green tick and your deploy is on the cloud!




## Step 5: Integrate it in your team





 	
  * Open your slack. It should be like.`{your_slack_url}/home`

 	
  * Click on Integration in the upper left section.

 	
  * Click on Configured Integrations.

 	
  * click on Add next to slash Commands.

 	
    * Command: `thel` [If you want to change this check explanation of the code below]

 	
    * URL: `{your_herokuapp_url}/thel` [This can be changed see, code below]

 	
    * Customize name: Give it some name. I gave it - `The L`

 	
    * For the **Autocomplete help text**, check to show the command in auto-complete list.

 	
    * Description: `The Genius L, is now in slack to answer all your queries` OR [anything you want to write].

 	
    * Usage Hint: `search query`

 	
    * Descriptive Level: Search Query







## Step 6: All Done!





 	
  * Open your slack channel and type `/thel` or `[your command]` and type Query.

 	
  * Everything is working fine you will see the result.

 	
  * Try basic Queries first for the testing purpose, it will give an instant response.

 	
  * Congratulation you just created a new Slack Bot for your own Team!! Have fun! :beers:




# Explanation of the Code


I already explained usages of the files in the directory. I created this Slack Bot using [WolframAlpha Module](https://pypi.python.org/pypi/wolframalpha) and [Flask](http://flask.pocoo.org/) framework. You should know working with Python modules and all the syntax to understand the code.

    
    <code class="language-python">__author__ = 'vikesh'  # this is the author name 
    
    import os  # This module provides a portable way of using operating system dependent functionality.
    import wolframalpha 
    # I have used Wolframalpha module, written in python to work with Wolframalpha API.
    # Read more about it here: https://pypi.python.org/pypi/wolframalpha
    
    from flask import Flask, request, Response, redirect
    # Modules from Flask
    
    
    
    try:
        import config       # Import config.py file 
        wol_id = config.wolframalpha['app_id']   # assign APP_ID from config.py to wol_id
    except:
        wol_id = os.environ.get('APP_ID')   # load envoironment variable
    
    
    if not wol_id:        # if wol_id is not present display error and exit
        import sys
        print 'No config.py found exisiting...'
        sys.exit(0)
    
    
    app = Flask(__name__)     # initiate flask app
    
    client = wolframalpha.Client(wol_id)      # this is from Wolframa moudule. Initiate Client using Wol_id
    
    
    @app.route('/thel',methods=['post'])
    def thel():
        '''
        :Example:
        /thel current weather in mumbai?
        '''
        '''
        this is about routing of the URL in flask.
        If you want to change the command from `/thel` to something else. you have to change the app.route('/thel') to 
        your own command. Also change the function name.
        '''
        text = request.values.get('text')    # Get Query
        try:
            res = client.query(text)       # Get result from Wolframalpha API.
        except UnicodeEncodeError:    # if coudln't get the response show error.
            return Response(('Sorry I did\'t get you. Would you please simplify your query?'
                            '%s is not valid input.' % text),
                            content_type='text\plain; charset=utf-8')
        resp_qs = ['Hi Top Answer for "%s"\n' % text]    # if Query is successful show result.
        resp_qs.extend(next(res.results).text)     # iterate the result 
    
        return Response(''.join(resp_qs),
                        content_type='text/plain; chatset=utf-8')     # return response.
    
    @app.route('/')
    def hello():      # if someone tries to open the link directy redirect him somewhere :p
        return redirect('https://github.com/vicky002/slack-TheL')
    
    
    if __name__ == '__main__':   
        port = int(os.environ.get('PORT',5000))    # run your app on local
        app.run(host='0.0.0.0', port=port)</code>


Please comment below for the issues related to the post and anything related to My Slack bot Please use the [issue tracker](https://github.com/vicky002/slack-TheL/issues) to report any bugs or file feature requests. You can also join [Eulercoder](http://eulercoder.me/about/) Organization on Github, just us [an email ](mailto:hi@eulercoder.me)with following details - Your Name, Your Education level and whether you want to join as a contributor or a mentor.



* * *



**Have something to talk about? Want to [contribute](http://eulercoder.me/contribute/) to our [Mission and Goal](http://eulercoder.me/about)? Write on Eulercoder. Write an [email to us](mailto:hi@eulercoder.me) or [contact us](http://eulercoder.me/contact) for guest posts. **

Did you found this article useful? Write your comments below. 🙂

If you found this article useful, do share with your friends. Let’s help everyone in becoming a better developer and help them in learning programming. [Subscribe to our newsletter](http://eepurl.com/bRklFn) and get weekly curated content directly in your mail box. Also, we have our own group on FB, we take Q&A, post materials in the group. I’ll highly recommend you Like [our FB page](https://www.facebook.com/eulercoder) and [join our FB Group.](https://www.facebook.com/groups/eulercoder) for discussions and latest updates.












## Subscribe to our mailing list




* indicates required




Email Address *






















