---
author: Kevin
comments: true
layout: post
current: post
comments: true
navigation: True
class: post-template
cover:  assets/images/cover/build_dev.jpg
subclass: 'post'
date: 2017-08-28 03:10:37+00:00
link: http://eulercoder.me/2017/08/react-chatroom-100-lines/
slug: react-chatroom-100-lines
title: Let's Build a React Chatroom Component in 100 Lines of Code
wordpress_id: 823
tags: ["react"]
---

# Let’s Build a React Chatroom Component




<blockquote>Oh hey, how’s it going!</blockquote>


So — I was building this React Chatroom Component for my side-project, which I don’t think I will be able to launch soon due to **a lot of things**, I would love to chat about. But yeah, instead of letting this sit it my Gitlab for months, I thought open sourcing it to [Github](https://github.com/WigoHunter/react-chatapp) might just be a better idea.

Who knows. I might even receive early feedbacks!

And I love writing. So I will try to explain how I built this React Chatroom Component in 100 lines of JavaScript along with some high-level introductions of how React applications and components work!React applications and components work!

![react chatroom - eulercoder](https://cdn-images-1.medium.com/max/2000/1*kjOH0q5D2vKlHq7Z5QrATQ.png)

<!-- more -->
### Why React


I guess you might have your reasons if you decided to click into this post. You might find it cool. You might be hearing a lot of the library and just want to know more about it.

For me, though, React is fun. It really disrupts the minimal unit level in web applications to become functional **components**, where you would define how a component is being rendered, along with the data it consumes and the functionalities it controls.

This largely increases the re-usability of codes.

For instance, I can very much write

    
    <code class="language-javascript">class Chatroom extends React.Component {
    submitMessage() {
    // JS Code here
    }</code>



    
    <code class="language-javascript">render() {
    const { chatHistory } = this.props;</code>



    
    <code class="language-javascript">return (
    {/* HTML code here */}
    );
    }
    }</code>


and from there, I would be able to render a Chatroom anywhere with

    
    <code class="language-javascript"><Chatroom chatHistory={chatHistory} /></code>


You see how data is being streamed in this case. You can simply inject the chat data from your App to a Chatroom component, which you would use just like HTML tags, and in the Chatroom component’s code, you will gain access to the data source with this.props.

Beautiful isn’t it.

And the missing HTML code and JS code are what will be covered in this tutorial.

You can check out my source code on [my Github repo](https://github.com/WigoHunter/react-chatapp) where I also have a [demo](https://react-chatapp.surge.sh/) hosted on [Surge](https://surge.sh) (Definitely recommend Surge for its amazing and free single command line deployment)


<blockquote>Final Disclaimer: I will not include how I stream data with Meteor in this tutorial. That would make this too long to write for me, and of course, for you to read. If I do get the time, I will make another blog post about the createContainer functions that come with Meteor.</blockquote>




### Create-React-App


Another great thing about React is the support of open source community. Since all you will ever make with React are components, people are open sourcing their components like this front-end React-Chatapp on Github or Npm as well.

This pretty much realizes the dream of “**Don’t reinvent the wheels.**” It’s completely possible for you to develop a website or application without writing any of these components. Just look up from Github or any other frameworks and inject the right data into the right components and you are good to go.

It might bring you troubles to config webpacks and those magics in the background at first — But Facebook even mitigates this by providing a CLI to get a React app up and running with a single command line.

So first, if you don’t have this tool installed, let’s first go to your terminal or iTerm and run (assuming npm is installed)

    
    <code class="language-git">npm install -g create-react-app
    create-react-app <the project name you want>
    cd <the project name>
    npm start</code>


from here, you will see a generic starting page run on localhost:3000

![react initilization - eulercoder](https://cdn-images-1.medium.com/max/1600/1*bwFxqbpXBb0u4JS8IRw9Zg.png)

And if you open the project folder with VSCode (Yup, Microsoft pride.), you will see the App Component in /src like this.

    
    <code class="language-javascript">class App extends Component {
    render() {
    return (
    {/* SKIP: Generic HTML codes */}
    );
    }
    }</code>


You can try to edit anything here and you will notice the website at localhost:3000 hot reloads. That is, You don’t need to refresh the page, the project will observe the file changes and hot reload the page for you.


### Build the Chatroom


SO — Let’s get straight to the point. Let’s modify the render function of this App to only render a background div where we apply an easy Flexbox layout to pin the Chatroom in the center of the page (You can simply copy my CSS code from [here](http://react-chatapp.surge.sh/static/css/main.b5d0bdf9.css), as I won’t be covering CSS in this post), and of course, the Chatroom component itself.

    
    <code class="language-javascript">return (
    <div className=“App”>
    <Chatroom />
    </div>
    );</code>


Notice this should break your project because it doesn’t yet recognize the Chatroom Component. So now let’s plan how we would want our Chatroom to look like.

You might have other ideas, but to me: a chatroom has two most critical missions:



 	
  * Render the chat history

 	
  * Allow user to submit a new message


And this is also why I felt like this Component I made for my side-project, could be an alternative example other than to-do apps — Because these two objectives pretty much map to Create and Read operations in CRUD. Of course, Update and Delete are something you could add to chatrooms as well, but I just didn’t plan to have these functions in the prototype level of ChillTime.

![chatroom submit message image - eulercoder](https://cdn-images-1.medium.com/max/1600/1*AZHXZeU1kHwjRRQLNKSRxQ.png)The central part of the chatroom that renders chat history


### Render Data


In the source code [here in src/Chatroom.js](https://github.com/WigoHunter/react-chatapp/blob/master/src/Chatroom.js), you will see that I have prepared a set, or more specifically, an array of mock data of chat history. And in React, if you want to iterate and render each element in an array, you should make use of the map function.

One thing to note though, is that instead of using React states to store data, in the cases you do have a Backend for data streaming, whether it is with Meteor methods, GraphQL queries, or any other server technics you are familiar with, you should inject data to the Chatroom Component as properties like _<Chatroom chatHistory={chatHistory} />_.

For the comparisons of props and states in React, there are already tons of great documents out there. So if you are interested in it, I would recommend you to checkout [this official doc](https://facebook.github.io/react/docs/state-and-lifecycle.html). But in short, I would use State to control the component’s UI status, for example, whether a user is typing or not, and Props for actual data of the application.

    
    <code class="language-javascript"><ul className=“chats” ref=“chats”>
    {
    chats.map((chat) =>
    <Message chat={chat} user={username} />
    );
    }
    </ul></code>


This is how I really render data with the map function. It will map each element in the array of chats to a Message Component where, this time, I do pass data as properties to this Message Component.

    
    <code class="language-javascript">const Message = ({chat, user}) => (
    {/* SKIP: HTML codes /}
    {/ Check out details at https://github.com/WigoHunter/react-chatapp/blob/master/src/Message.js */}
    );</code>


And the functional declaration of this Message Component in [src/Message.js](https://github.com/WigoHunter/react-chatapp/blob/master/src/Message.js) is simpler. It doesn’t own JavaScript functions and simply renders HTML codes expecting two data inputs: chat and user to be used in the HTML codes.


### Submit New Message


![submit a new message - eulercoder](https://cdn-images-1.medium.com/max/1600/1*Vxqxe9UPRmzL83ssCOvqKQ.png)The bottom part that submits a new message

Submitting in React is not much different from what you would do with pure JavaScript, or Vanilla JS if you prefer (xD). Just remember that you can get data from UI with the library ReactDOM that comes with create-react-app. So for example, we have an input like this in the bottom of the chatroom.

    
    <code class="language-markup"><input type=“text” ref=“msg” /></code>


We would be able to get its value by:

    
    <code class="language-markup">ReactDOM.findDOMNode(this.refs.msg).value</code>


Therefore the texting part in the bottom would be like:

    
    <code class="language-markup"><form className=“input” onSubmit={(e) => this.submitMessage(e)}>
    <input type=“text” ref=“msg” />
    <input type=“submit” value=“Submit” />
    </form></code>


And in the submitMessage function, (again, since we are using State to store the chat history in this case), we will use the native React function setState to modify the origin state of data and one more additional entry whenever users type and submit. For the sake of UX, let’s also reset the value of the input to an empty string on success.

Another thing worth noting with React state is that it is immutable: so even if you want to insert an element into an array in states, you should use the JavaScript array method **concat** rather than **push**, which actually mutate the value of the state.


### Finally, Deployment with Surge


So if everything goes right, you should have this Chatroom running just fine now. And the final part of deployment is where another great synergy of Create-react-app and [Surge](https://surge.sh) comes in handy.

Now just trust me with this.

Go to the root directory of this project in your terminal.

    
    <code class="language-git">#If you don’t have surge yet then do this:
    npm install -g surge</code>



    
    <code class="language-git">#If you already have surge, or after installing it:
    npm start build
    cd build
    surge</code>


There you go. Literally, three commands in Terminal and your first React site will be running healthy and at no cost at a [Surge.sh](http://Surge.sh) sub-domain just like my [react-chatapp.surge.sh](https://react-chatapp.surge.sh) with free and built-in SSL/HTTPS.


### Whoa! My First Attempt


This is the first time I have ever made a blog post that is even close to being a tutorial.

Now I think I did skip some of the codes in this post, primarily because I don’t believe copying all the codes and explain to them line by line is a good way to elaborate. So I took another approach and tried to reason the high-level designs and functions we have in this Chatroom Component, along with the introduction of some concepts and tools that synergy well with React.

I would definitely appreciate if some of you can share some opinions in the comment section.

But yeah, this is how I made this React Chatroom Component in 100 lines explained — Now I am more comfortable to say that at least I made some uses of these codes, and put it aside, for now, to prepare for my GRE test, graduate program applications and job searches!

**Highly Recommended articles: **

If you are looking for Summer Internship or preparing for full-time job, I highly recommend you to read out [Summer Internship: The Ultimate Guide](http://eulercoder.me/2017/07/summer-internship-ultimate-guide/). We have also written a beginner's guide on [Getting Started with Open Source Development](http://eulercoder.me/2017/07/getting-started-open-source/). Do read them!



* * *



Anyways, if you are interested in talking to me, perhaps there is something in this tutorial I failed to explain enough, or even if you see potential synergies between you and me (just like create-react-app and surge), feel free to reach me at [kevin@projectable.hk](mailto:kevin@projectable.hk) or connect me on [Linkedin](https://www.linkedin.com/in/kai-chun-kevin-hsu-5428bbb4).



* * *



**Have something to talk about? Want to contribute in our [Mission and Goal](http://eulercoder.me/about)? Write on Eulercoder. Write an [email to us](mailto:hi@eulercoder.me) or [contact us](http://eulercoder.me/contact) for guest posts. **

Did you found this article useful? Write your comments below. 🙂

If you found this article useful, do share with your friends. Let’s help everyone in getting a better internship, job and help them in learning programming. Subscribe to our newsletter and get weekly curated content directly in your mail box. Also, we have our own group on FB, we take Q&A, post materials in the group. I’ll highly recommend you Like [our FB page](https://www.facebook.com/eulercoder) and [join our FB Group.](https://www.facebook.com/groups/eulercoder) for discussions and latest updates.



* * *














## Subscribe to our mailing list




* indicates required




Email Address *






















