# jQuery UTM to Cookies
Getting UTM values with jQuery and saving into cookies

This guide was based on two different articles:

[How to Use Cookies to Capture URL Parameters](http://jennamolby.com/how-to-use-cookies-to-capture-url-parameters/)
_by Jenna Molby_

[Get URL parameters using jQuery](https://www.sitepoint.com/url-parameters-jquery/) 
_by Sam Deering_

## Introduction

These days I had situation at work where I had to get UTM parameters from the URL and save them into Cookies. So later, when the user performs an "event", this data would be sent to Google Analytics.

So I found [Jenna Molby's article](http://jennamolby.com/how-to-use-cookies-to-capture-url-parameters/) and started following it.

Her tutorial is awesome, but I had to tweak a couple of things in order to get it working:

1. Update to the [new version of js-cookie](https://github.com/js-cookie/js-cookie);
2. Tweak the function that parses the URL, it wasn't working for me.

So now decided to share my solution, which is basically an updated version of Jenna's solution.

Hope this is useful for someone!

## Step 0
_Pre requisites_

It may seem obvious but don't forget to add these resources:

```
<script src="http://code.jquery.com/jquery-latest.min.js" type="text/javascript"></script>

<script src="js/jquery.cookie.js" type="text/javascript"></script>
```

_Note that **jquery.cookie.js** hast to be downloaded and saved in your JS folder. Download it [here](https://github.com/js-cookie/js-cookie)._


## Step 1
_Let's get the url parameters and save them as variables_

```
$.urlParam = function(name){
  var results = new RegExp('[\?&]' + name + '=([^]*)').exec(window.location.href);
  if (results==null){
     return null;
  }
  else{
     return results[1] || 0;
  }
}

var source = $.urlParam('utm_source').split('&')[0];
var medium = $.urlParam('utm_medium').split('&')[0];
var campaign = $.urlParam('utm_campaign').split('&')[0];
```

Note that I had to use jQuery's _.split()_ function because when you have more than one parameter the first one would wrap the rest of them. For instance:

**URL**
?utm_source=source-value&utm_medium=medium-value&utm_campaign=campaign-value

**SOURCE **  
_"source-value&utm_medium=medium-value&utm_campaign=campaign-value"_

## Step 2
_Set the cookies_

Using js-cookie is pretty easy to set, load and remove cookies. Isn't that great? So let's get those variables and save them in your user's cookies. **But wait!** Before that we need to check if this value isn't already set. In my case, I don't want the latest source, but the first source that led my user to the website.

```
// Set the cookies
if(Cookies.get('utm_source') == null || Cookies.get('utm_source') == "") {
	Cookies.set('utm_source', source);
	//console.log('cookie utm source was set to ' + source);
}
if(Cookies.get('utm_medium') == null || Cookies.get('utm_medium') == "") {
	Cookies.set('utm_medium', medium);
	//console.log('cookie utm medium was set to ' + medium);
}
if(Cookies.get('utm_campaign') == null || Cookies.get('utm_campaign') == "") {
	Cookies.set('utm_campaign', campaign);
	//console.log('cookie utm campaign was set to ' + campaign);
}
```

_If you want to test, comment out those console.log() statements and open your browser's console._

## to be continued...

On the next update I'll attach this info to a GA event and send it to be tracked.




