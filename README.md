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

*URL*
?utm_source=source-value&utm_medium=medium-value&utm_campaign=campaign-value

*RESULT*
source value: _source-value&utm_medium=medium-value&utm_campaign=campaign-value_
medium value: _medium-value&utm_campaign=campaign-value_
campaign value: _campaign-value_




