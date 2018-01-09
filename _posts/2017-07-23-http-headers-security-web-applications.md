---
title: HTTP Headers Related to Security
date: 2017-07-23 07:39:56 Z
categories:
- learning
- security
tags:
- http
- security
author: vicky002
comments: true
layout: post
current: post
navigation: true
cover: assets/images/cover/lock.jpg
class: post-template
subclass: post
link: http://eulercoder.me/2017/07/http-headers-security-web-applications/
wordpress_id: 317
---

This post is about HTTP headers and how it's useful in security. Among the many standard HTTP headers, some help to improve the security of web applications. This post contains a quick overview of these headers. So let's get started!





## HTTP Strict-Transport-Security (HSTS)





HSTS is a mechanism to notify the user that your domain name should only be accessed using HTTPS. There is a header sent by the servers in responses to HTTP requests. A user who receives this header includes all the futures call to your domain name will be done in HTTPS and redirect internally any attempt to call HTTP during the period defined by the "max-age" option. "max-age" is the minimum time after which a browser can make insure requests.

<!-- more -->



`Strict-Transport-Security: max-age = 259200; includeSubDomains; preload;`





So even though your user types manually `http://eulercoder.me` in the URL bar, no calls will be made HTTP. The browser will transform itself HTTP HTTPS before executing the query. Of course, it is necessary to receive at least once for the header that protection is enabled. The user is, therefore, a priori not protected at his first request, if it is performed by HTTP.





It is nevertheless possible to overcome this limitation and also protect the first query. Browsers have a list of "hard" domain names to be accessed exclusively by https. This list is managed by the Chromium project but is also used by other browsers. You can request the addition of a domain name to this list via the form on [HSTS Preload](https://hstspreload.appspot.com/) For this to be taken into account it is also necessary to add the flag "preload" the header Strict-Transport-Security.





## Content-Security-Policy (CSP)





The Content-Security-Policy used to define a whitelist of origins from which you authorize the loading of resources for your web application. For example, you can set up a CSP not allowing scripts if they are from your domain name. If your web application attempts to load a script whose origin is not allowed by the PUC, the loading will be blocked by the browser (of course if you have a modern browser that includes CSP).





Furthermore, the CSP prohibits scripts inline by default. It becomes, for example, possible to execute a on-click directly on your HTML elements. Your entire Javascript code should be outsourced in dedicated js files (which was anyway a good practice), and imported from an origin authorized by the PUC. MSCs are of great interest from a safety point of view. To the extent you control the source of your resources and where inline scripts are not allowed, CSPs are extremely effective against XSS flaws.





`Content-Security-Policy: 
default-src: 'none'; 
style-src 'self' ' http://www.example.cdn.com '; 
frame-src: ' http://www.youtube.com ' ; 
script src 'self' ' https://ssl.google-analytics.com '; 
img src-: 'self'; 
font-src: ' http://www.yourdomain.com '; 
connect-src: ' s elf' 
report-uri: https://www.yourdomain.com/report` In this example, the styles are allowed provided they are from the domain name being (self), or Cdn. We allow scripts from self and Google Analytics, the frame of Youtube, Ajax requests (connect-src) on self, etc. The report-uri option allows sending a report every time a resource is blocked. These reports allow you either to adjust your policy by allowing a resource that should not have been blocked or to detect XSS flaws on your site.





### Policy applies to a wide variety of resources





While script resources are the most obvious security risks, CSP provides a rich set of policy directives that enable fairly granular control over the resources that a page is allowed to load. You’ve already seen script-src, so the concept should be clear. Let’s quickly walk through the rest of the resource directives:







  * **`base-uri`** restricts the URLs that can appear in a page’s element.


  * **`child-src`** lists the URLs for workers and embedded frame contents. For example: child-src https://youtube.com would enable embedding videos from YouTube but not from other origins. Use this in place of the deprecated frame-src directive.


  * **`connect-src`** limits the origins to which you can connect (via XHR, WebSockets, and EventSource).


  * **`font-src`** specifies the origins that can serve web fonts. Google’s Web Fonts could be enabled via font-src `https://themes.googleusercontent.com`


  * **`form-action`** lists valid endpoints for submission from 





<





form> tags.







  * **`frame-ancestors`** specifies the sources that can embed the current page. This directive applies to `<frame>, <iframe>, , and <applet>` tags. This directive cant be used in `<meta>` tags and applies only to non-HTML resources.



  * **`frame-src`** deprecated. Use **`child-src`** instead.



  * **`img-src`** defines the origins from which images can be loaded.



  * **`media-src`** restricts the origins allowed to deliver video and audio.



  * **`object-src`** allows control over Flash and other plugins.



  * **`plugin-types`** limits the kinds of plugins a page may invoke.



  * **`report-uri`** specifies a URL where a browser will send reports when a content security policy is violated. This directive cant be used in tags.



  * **`style-src`** is script-src’s counterpart for stylesheets.



  * **`upgrade-insecure-requests`** Instructs user agents to rewrite URL schemes, changing HTTP to HTTPS. This directive is for web sites with large numbers of old URLs that need to be rewritten.






## Public-Key-Pin





When you access a website https, your browser checks the validity of the certificate of the domain name, up the chain of certificate authorities that issued the certificate, to find an authority in which he trusts.





This fundamental principle of public key architecture (PKI) assumes that you trust to a number of certification authorities. Unfortunately, it is not uncommon for forged certificates issued by authorities is "trusted". In September 2015, fake certificates google.com EV (extended validation) have circulated . Public-Key-Pin is a header sent in the HTTP responses. It can send the user the hash of the public key of your certificate. When a client receives the header, it is known that during the time specified by the max-age option, it will only trust this certificate. Any other certificate will generate a security exception.





`Public-Key-Pin: max-age = 259200; pin-sha256 = "426df4d6 ... .fdf / BACC ="`





Rather than specifying the hash of your certificate, it is also possible to send the hash of the certificate from a certification authority. In this way, only certificates signed by that certificate authority be accepted.





Like the header HSTS, users are unprotected during the first call. And in the same way, you can ask Google to add the hash desired "hard" in Chrome . Before implementing this header, it seems necessary to assess potential risks. What will happen if the private key of your server is corrupted, and you impose a renewal certificate with public key change? From this point of view, providing the hash of the public key of a certificate authority appears more flexible, to the extent that you can make the certificate of renewal without disturbing your users, as you go through the same certification authority.





## X-Frame-Options





Iframes are commonly used to implement attacks "clickjacking". It is possible for an attacker to superimpose an invisible iframe (opacity 0) over a classic page. When a user attempts to click on a visible part of the page, click on the actual frame and can be caused to perform unwanted actions (liker a Facebook post, retweet a message ...). You will find here an example implementation on Twitter. The header X-Frame-Options to restrict the display of your site within an iframe:





`X-Frame-Options: deny`





## X-Content-Type-Options





Some browsers (IE) enable automatic format detection of a file from its content. This allows particularly to interpret correctly files with mime-type is incorrectly entered in the HTTP response. This nevertheless poses a safety concern to the extent that we can in this way to execute the browser code that is not meant to be executable.





`X-Content-Type-Options: NOSNIFF`





The header X-Content-Type-Options tells the browser to rely exclusively on mime-type returned in the HTTP response, and never try to deduce the mime-type from the file contents.





## X-XSS-Protection





This header is used by newer browsers and enables an automatic detection of XSS flaws. When this option is enabled, the browser will try to detect whether certain sensitive data in settings are made as shown in the page. If this is the case, the browser will block the rendering of the page.





`X-XSS-Protection: 1; mode = block`





This header is used with care, to the extent, possible false positives could be very disadvantageous to your site. It is also possible to activate a "report" that in addition to blocking the page allows you to send a report to each flaw found.





`X-XSS-Protection: 1; report = http://site.com/report`





## Conclusion





These HTTP headers can improve significantly the security of your web applications. Some of them are still handled with care and it is necessary to assess potential risks before using them. It is important to remember that these headers will have no effect for browsers that support, and will be ignored by others. They are therefore an additional layer of security for some of your users, but not enough in themselves -Same. It, therefore, remains necessary to apply the usual security best practices.



