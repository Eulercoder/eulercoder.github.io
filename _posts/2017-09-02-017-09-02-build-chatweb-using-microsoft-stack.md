---
title: ChatWeb — Build websites that understand users with full (free) Microsoft Stack
date: 2017-09-02 13:41:39 Z
categories:
- learning
- tutorials
tags:
- learning
- tutorials
author: Kevin
comments: true
layout: post
current: post
navigation: true
class: post-template
cover: assets/images/cover/chatweb.jpeg
subclass: post
link: http://eulercoder.me/2017/09/build-chatweb-using-microsoft-stack/
---

Just another weekend to C&C (Chill & Code) in Taiwan


### ChatWeb?


Chatbot has been a hot topic around for sure. I still remember how 4 teams out of the 12 teams in [E.C. Jamming](http://ecjamming.tech), a Hackathon I organized with Microsoft a couple of months ago in Hong Kong, were working on chatbot projects.
<!-- more -->
But I am a real web lover and evangelist.

There are great frameworks out there like Google [API.ai](https://api.ai), [Microsoft Bot Framework](https://dev.botframework.com/) and [Microsoft LUIS](https://www.luis.ai) (Language Understanding Intelligence Service) that enable developers to build and launch chatbots in multiple channels with not much an effort.

In other words, to empower chatbot developers, the tech giants have invested a considerable amount of resources to build up the components and micro-/web-services that can perform a single specific task in a chatbot’s pipeline both efficiently and accurately. For example, LUIS helps you understand a user with training models backed by Machine Learning with a single API endpoint.


Now, why don’t we **take these services to build a website that understands users’ intentions and interests just like a chatbot**, and at the same time, with the rich elements the web has to offer, we are able to provide more interactions than a traditional chatbot.

![background prototype - chatweb Microsoft stack eulercoder](https://cdn-images-1.medium.com/max/1600/1*FMBO66chD18WVdITPk_5VQ.png) The background of prototype was from https://tympanus.net/codrops/2014/09/23/animated-background-headers/[/caption]

In my case, I am now building a personal portfolio page for the applications for graduate programs in the US.

On top of the static, standard resume-alike HTML contents, I am implementing a header that interacts with users and rearranges the styles and components rendered on the page once users ask a specific question regarding my experiences — Just as if I was there, 1 on 1, with this user to answer questions like “What have you been working on at Microsoft?”.

#### Read: [Build your first Slack Bot using WolframAlpha API in Python](http://eulercoder.me/2017/07/create-slack-bot-using-wolframalpha-api/)


What’s even better is this tutorial and the so-called ChatWeb is built with a server-less architecture (thanks to [Azure Functions](https://azure.microsoft.com/en-us/services/functions/)), which means you won’t have to worry about building and maintaining a server at all. Just front-end. Just **ReactJS**.

![](https://cdn-images-1.medium.com/max/1600/1*JdAeqbbDO3LCP-29FbAPZg.png)

Everything below assumes a working Azure account. Azure provides free trial for a month for you to play around. Even after the free trial expires, all the products in this architecture have monthly free quotas that are not easily exceeded.



* * *





### Contents





 	
  * Training and Publishing a Microsoft LUIS endpoint

 	
  * Build Azure Functions

 	
  * React and Fetch





* * *



![](https://cdn-images-1.medium.com/max/2000/1*-ZOlRRQFUcOWumuAqUVBPg.png)


### Microsoft LUIS


So again. LUIS plays a significant role in the chatbot’s ecosystem by empowering bots to understand the literal from users. The end goal of a LUIS endpoint is by consuming a query, it returns a JSON data with the answers to questions like “What does the user want?” and “Any specific entities involved?”

    
    <code class="language-javascript">Example:
    query = "What have you done at Microsoft?</code>



    
    <code class="language-javascript">-----</code>



    
    <code class="language-json">LUIS API returns
    {
    “query”: “what have you done at microsoft”,</code>



    
    <code class="language-json">“topScoringIntent”: {
    “intent”: “WhatDidYouDo”,
    “score”: 0.729854763
    },</code>



    
    <code class="language-json">“entities”: [ {
    “entity”: “microsoft”,
    “type”: “Company”,
    “startIndex”: 22,
    “endIndex”: 30,
    “score”: 0.9897579
    } ]
    }</code>


With this information and the functional component rendering mechanism in React JS, you are already ready to build your LUIS web app.

But before we dive into just that, and how we can protect our subscription key with Azure Functions, let me also show you how I trained my language understanding models on [**LUIS.ai**](https://www.luis.ai).


#### Step-By-Step Guide to Build a LUIS App


![](https://cdn-images-1.medium.com/max/1600/1*cnVWjxUFHP_E78--heNZqw.png)

After creating an account on LUIS, you should be led to this page.

Click the button New App. It should ask you about the name of your app and its culture (en-us, for example. I haven’t tried with other cultures.).

![](https://cdn-images-1.medium.com/max/1600/1*o-Zd3zuwEVg7hpo5KOYf5A.png)LUIS Dashboard

On the sidebar, click Intent where you will start to define the possible intents your users might possess. For example, if you are making an e-commerce website, the possible intents the website should observe can be “Purchase”, “Browse”, “Customer Support”, etc. Basically, try to list out all the possible reasons for a user to come and use your app.


<blockquote>In my case of building a portfolio page, I want my LUIS app to help me detect from a sentence if a user is more interested in what did I learn (the technology stacks), or what did I do (the project details), and at which company, so I can deliver a suitable and personalized layout.</blockquote>


![](https://cdn-images-1.medium.com/max/1600/1*MoMqIagOd6uKOqdyNHXQew.png)

After an intent is created, you can give LUIS some examples where this specific intent should be expected. For this example, I told LUIS a user who asks questions like “What did you do for Microsoft”, “What have you been doing in Microsoft”, etc, are likely to hold the intent of asking “What Did You Do” (Intent) with “Microsoft” being mapped to the company entity.

After you input a solid 10 examples per intents defined, go to **Train & Test** on the sidebar and click “**Train Application**”. LUIS will handle the machine learning part for you.

**Magic.**

I love being an application developer in the early 21st century; fresh and cool, but not yet completely taken over by AI.

Once it is done, go to “**Publish App**” page and you can straight up just one-click push the app to production and get an endpoint that looks like this:

    
    <code class="language-http">https://westus.api.cognitive.microsoft.com/luis/v2.0/apps/{app id}?subscription-key={app key}&timezoneOffset=0&verbose=true&q={user query}</code>


And bang.

![](https://cdn-images-1.medium.com/max/1600/1*h5xztCIsevgeFnH15T4Uug.png)

You now have an endpoint that translates a user’s literal inputs, into structured and self-defined intents and entities in JSON format.


### Azure Functions


Now, this step is not compulsory but highly recommended, for the reason that it is generally a bad practice to expose your secrets and keys to client browsers.

Especially LUIS starts to charge you once the amount of your monthly request exceeds 10K — This is not that ambitious as you think. Request means transactions. A happy user who is just too excited to stop asking questions for fun can be one heavy load.

You really don’t want to take the risk to let other people use your subscription keys on their LUIS app — You might not be aware of the usage on other sites, but their bills would be on you.

We could solve this problem by implementing a proxy server, perhaps with Node and Express, and have the HTTP requests done on the backend.

But, **Azure Function is another better solution, if not the best.**

And it is very simple.

Let’s see how we can build a proxy endpoint with Azure Functions **in 5 min**.

![](https://cdn-images-1.medium.com/max/1600/1*CpRBltKvIAuA47b-4fycjw.png)

Just search “Function App” on Azure Dashboard and create one with an App name (I named mine “LUIS-proxy”), subscription plan (Yep… My Internship at Microsoft ends. Free Trial time.) and all other simple and expected inputs. Nothing should surprise you here.

[caption id="" align="alignnone" width="1600"]![chatweb microsoft web - eulercoder](https://cdn-images-1.medium.com/max/1600/1*sJ-Rik66EL4JTkluFb1Abg.png) Click the + next to “Functions” on sidebar to create a Function App[/caption]

So what this Function App is going to do, is that it is going to act just like the LUIS endpoint we got from the previous steps, but without giving users the access to your API IDs and Subscription Keys.

Let me just quote this from the official site of Azure Functions — Who can possibly explain Azure Functions other than the team themselves.


<blockquote>“Functions provides a fully managed compute platform with high reliability and security. With scale on demand, you get the resources you need — when you need them.”</blockquote>


With Azure Functions, the client-side code will be calling this Azure endpoint where it will then complete the HTTP request using API key of LUIS on Azure’s computing platform and simply return the LUIS result to the client side. Think of this like a lambda function on the Azure cloud, if you are familiar with Functional Programming.

Again, if you are not familiar with this, this step is not a must to achieve a website that understands users. This is for security reason. And also, you are welcome to build this proxy with Node and Express stack and setup the API endpoint with [Digital Ocean](http://eulercoder.me/recommends/digitalocean/), [Linode](https://www.linode.com/) or [Heroku](https://www.heroku.com/). They are all nice and neat in this case.

But if you decided to follow me on this,

Here is what you will do on your Function App:

[caption id="" align="alignnone" width="2000"]![chat web - Microsoft stack - eulercoder](https://cdn-images-1.medium.com/max/2000/1*E193G13sLrhjOHUDJv3Vzw.png) functions[/caption]

This function is written in C#, primarily because I felt like C# can finish the task (simply download the responding resource from LUIS) in a more efficient manner with. `WebClient` Azure Functions do support JavaScript, feel free to do this part the way you are comfortable with.

But you can really use this code I wrote to shield any API keys you have from any web-service providers. Just paste the Url link you would use in AJAX into the placeholder I had on this screenshot, and click the **</> Get function URL** on the top right corner to get the proxy endpoint for LUIS in this case.

Finally, under {Your App} > CORS, remember to define and manage a list of white-list origins to call this endpoint. I really love this feature. This ensures **only your websites have the permission to consume these API endpoints**.


### React and Fetch


Finally, for the client side code, I am choosing to React once again.

But this time, there are reasons besides “React is cool and clean”. I promise.

React has the ability to control the components rendered with,`this.state` we don’t have to do AJAX request so frequently into a callback hell. And the polyfill Fetch library that returns a ES6 Promise works seamlessly with React.

Before I start showing some codes. Remember to run the create-react-app.

    
    <code class="language-git"># in your terminal
    create-react-app <project name>
    cd <project name>
    npm install --save whatwg-fetch
    npm start</code>


This should set you up with everything you will need and also open up the app at localhost:3000.

With fetch and React’s `state` available right now, we will be able to let the app render components based on users intent returned from LUIS. My high-level architecture will be something like this (a constructor, a function that handles LUIS query, and a render function)

    
    <code class="language-javascript">… standard React import statements
    import ‘whatwg-fetch’;</code>



    
    <code class="language-javascript">class ChatWeb extends Component {
    constructor(props) {
    super(props);
    this.state = {
    query: “”,
    intent: “”,
    company: “”
    }
    }</code>



    
    <code class="language-javascript">submitLUISQuery(e) {
    e.preventDefault();</code>



    
    <code class="language-javascript">fetch ({Azure Functions Endpoint}&query=${this.state.query})
    .then(res => JSON.parse(res.json()))
    .then(json => {
    this.setState({
    intent: json.topScoringIntent.intent,
    company: json.entities[0].entity
    });
    });
    }</code>



    
    <code class="language-javascript"></code>



    
    <code class="language-javascript">render() {
    const { intent, company } = this.state;</code>



    
    <code class="language-javascript">return ( { intent === “WhatDidYouDo” && <TheComponentForThisCase company={company} /> } { intent === “WhatDidYouLearn” && <TheComponentForThisCase company={company} /> } ); } }</code>


Now I think the control of rendering can certainly be cleaner with `react-router-dom`. And my job on this portfolio project, from now on, will be exactly on this — To find a more efficient and modularized way to properly pipeline the technology stacks and data.

Again, this is a concept and high-level prototype that I just can’t wait to share with the community. Especially the new (and last) semester starts this week, and I will be stuck in applications of graduate programs, GRE, and potential job searches (Yes. I am still hoping to have employers sponsoring my visa, so I don’t need OPT that desperately.)

[caption id="attachment_864" align="aligncenter" width="728"][![](http://eulercoder.me/wp-content/uploads/2017/09/two-year-of-selflearning-in-article-post.jpg)](http://eulercoder.me/2017/08/2-years-of-self-learning-into-a-developer/) Two years of Self-Learning into a developer[/caption]


### Finally…


I just want to say that, of course, this is not an optimal user experience as of the moment. People do not (yet) expect to talk to your websites, which is also the reason these technologies have been used on Chatbot primarily. But I think this is definitely a fun and interesting experiment that I would dive farther— **We can definitely make websites understand users just like chatbots**.

This is really exciting for me!

**This opens a lot of new possibility of web UX.**

If you do have any better UX design that can synergy with this ChatWeb concept, definitely reach me out at [kevin@projectable.hk](mailto:kevin@projectable.hk), connect me on [LinkedIn](https://www.linkedin.com/in/kai-chun-kevin-hsu-5428bbb4), or simply leave comments below. WE have jobs to do!



* * *



**Have something to talk about? Want to contribute to our [Mission and Goal](http://eulercoder.me/about)? Write on Eulercoder. Write an [email to us](mailto:hi@eulercoder.me) or [contact us](http://eulercoder.me/contact) for guest posts. **

Did you found this article useful? Have questions? Write your comments below. 🙂

If you found this article useful, do share with your friends. Let’s help everyone in getting a better internship, job and help them in learning programming. Subscribe to our newsletter and get weekly curated content directly in your mail box. Also, we have our own group on FB, we take Q&A, post materials in the group. I’ll highly recommend you Like [our FB page](https://www.facebook.com/eulercoder) and [join our FB Group.](https://www.facebook.com/groups/eulercoder) for discussions and latest updates.










## Subscribe to our mailing list




* indicates required




Email Address *





















