---
layout: post
title: "Line Too Long on jsLint and jsHint"
date: 2013-09-16 10:36:07 -0300
comments: true
categories: [JavaSctipt, Code Quality]
---

This is just another approach to fix the famous Line too long on JShint or JSlint complain.
> NOTE: this started as an answer for this [Stackoverflow
> question](http://stackoverflow.com/questions/9570207/whats-the-jslint-approved-way-of-creating-a-long-string/).

The thing is what to do with those long strings that throw a **Line too long** complain but it still makes sense to have them in that way. Think in examples like an URL or a HASH / SHA.

First of all, it’s important to agree that there is no “one solution” for this situation and second of all that it’s a matter of design more than a technical issue. It’s true that sometimes it makes sense to split these kind of strings into several lines, such as **HTML** code representations, but sometimes it doesn’t as in the example of an URL, a **HASH / SHA** string or even paragraphs.

<!-- more -->

So the first attempt to add the `/* jshint maxlen: 130 */` option on top of your JS file will fix the problem, omitting the **Line Too Long** complains only on that file. But what about other lines on the same file, that are too long as well but they should be shorter, basically a valid concern from **jsHint**.

Since most of the cases where we want to keep the line as it is, no matter the length, are related to string representations like URLs, HASHs, SHAs and so on, using a placeholder could be a good approach.  Basically the idea is to create a JS file to store them and make it available through a global variable. Then you can just call it from any script across your site, like the way jQuery is used (Just remember that you need to load placeholder file before scripts that use it). The advantage about this solution is that you need to avoid this maxlen validation only in one file (Actually we are setting the maxlen to a very high number).



``` javascript my_placeholder.js
/*jslint maxlen: 500 */
 
//Init the placeholder for My Feature
MyFeature = MyFeature || {};
 
//Assign URL
MyFeature.productApiURL = 'http://this.is.the.url.to/product/API/';
 
//Assign a piece of TEXT
MyFeature.productTermsOfUseText = 'This is a very long text about
something that you want to explain regarding your product terms of use
for example......';
 
//Assign an HTML fragment
MyFeature.successfulMessageHTML ='</pre><div class="message"><div
lass="header">Successfully reated</div><div class="detail">some text
showing the detail...</div></div><pre>';
 
//Assign a Regex to perform some validation
MyFeature.validEmailRegex = new
RegExp(/^(([^<>()[\]\\.,;:\s@\"]+(\.[^<>()[\]\\.,;:\s@\"]+)*)|(\".+\"))@((\[[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\.[0-9]{1,3}\])|(([a-zA-AZ\-0-9]+\.)+[a-zA-Z]{2,}))$/);
```

``` javascript my_feature_related_file.js
//doing random awesome things
 
$.ajax({
  url: MyFeature.productApiURL,
  success: function() {
    $('#message_container').html(MyFeature.successfulMessageHTML);
  }
});
 
//more awesome code here
```

Finally another good thing about this approach is that we are reinforcing the semantic meaning of those long strings. Anyone can understand that MyFeature.productApiURL represents the URL product API or that MyFeature.successfulMessageHTML is the HTML code for a successful message for My Feature. Basically we are explaining what it means in **terms of domain** (Successful Message, Product API, Valid Email…) and **in which format it is represented** (HTML, URL, REGEX…).
