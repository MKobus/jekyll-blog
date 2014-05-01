---
layout: post
title:  "Google Maps API Integration"
date:   2014-05-01 17:39:00
categories: web-development
---

After [sites.everythingfarm.com](http://sites.everythingfarm.com) went live, the team decided there now was time to implement the option to drag the pin inside the google map on the rendered page. The idea behind this was that farmers are sometimes so obscurely located, we can't always expect *the* Google to be always right. So, we need to allow our rural, living in the boonies, farmers to show exactly how far into the badlands you may have to venture to get some choice pickings of your summer fruit.

Well of course, whenever something like this gets decided, it becomes a Front-End thing. So I began my journey with googling the Google Maps API and how to use it. Originally we were using Iframes but functionally, they do nothing, and can do nothing. To implement the map, we had to transform the entire thing to a `<div>` and load the map through a piece of JavaScript. Now, I'm not going to go through all the boring details, but just point out a few key things I learned throughout this journey:

You no longer need an API key (or so I believe)
=============

Lots of replies on the Google forums or Stackoverflow, have provided me with the assumption that one no longer needs an API key to make Google Maps work on your page. Now, I tried it out and it worked. But I guess we will have to see. Google does say not to worry, and that if a threshold does get reached, they will contact you. 

I found out you can skate by with this:
	<code>'https://maps.googleapis.com/maps/api/js?v=3.exp&sensor=false&'</code>

Bootstrap and Google Maps do not play nice
=================

They don't. It's plain stupid. But no body has the time to figure out what exactly is messing with it. So there's a workaround until you have the lazy Sunday to look at it (let's be honest. You never will.) If you want to know what happens with Bootstrap and the map `<div>`, it's pretty easy to see. It looks stupid. And there will be one tiny square of a map in the upper left hand corner of the div, and no amount of dragging is going to save you. Nope, the best way to go about it is to load this piece of code in your initialize() function:
	
	google.maps.event.addListenerOnce(map, 'idle', function() {
		google.maps.event.trigger(map, 'resize')
		map.panTo(marker.position)
	})

How to get the coordinates of an Address
====================

At first I thought it was going to be easy. Then I thought it was going to be hard. And then I just didn't know. But Google is your friend. Remember that. All you need to find the coordinates of a user typed address, is to use this link:
		<code>	 url = 'http://maps.googleapis.com/maps/api/geocode/json?address=' + getCoordinatesAddress + '&sensor=true'
	        $.getJSON(url, function(data) {
	            var location = data.results[0].geometry.location;
	            // coordinates are location.lat and location.lng
	            console.log(location.lat)
	            console.log(location.lng)
	        })<code>
 As you can see, the <strong>url</strong> variable has a simple geocode address with <strong>getCoordinatesAddress</strong> in there. This was passed as a simple value. Spaces and all. Google couldn't care less. Note the jQuery getJSON call. You will have to parse the response, as you see with `var location`. And the console logs were there just for me to make sure I was getting the right data from the response. Console.logs are your friend.

 That's it really. I was done in half a day, and it was great. Learned a bit about Google APIs and it was great.