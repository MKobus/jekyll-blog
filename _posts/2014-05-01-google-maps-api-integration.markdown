---
layout: post
title:  "Google Maps API Integration"
date:   2014-05-01 17:39:00
categories: web-development
---

<p>After <a href="http://sites.everythingfarm.com">sites.everythingfarm.com</a> went live, the team decided there now was time to implement the option to drag the pin inside the google map on the rendered page. The idea behind this was that farmers are sometimes so obscurely located, we can&#39;t always expect <em>the</em> Google to be always right. So, we need to allow our rural, living in the boonies, farmers to show exactly how far into the badlands you may have to venture to get some choice pickings of your summer fruit.</p>

<p>Well of course, whenever something like this gets decided, it becomes a Front-End thing. So I began my journey with googling the Google Maps API and how to use it. Originally we were using Iframes but functionally, they do nothing, and can do nothing. To implement the map, we had to transform the entire thing to a <code>&lt;div&gt;</code> and load the map through a piece of JavaScript. Now, I&#39;m not going to go through all the boring details, but just point out a few key things I learned throughout this journey:</p>

<h1>You no longer need an API key (or so I believe)</h1>

<p>Lots of replies on the Google forums or Stackoverflow, have provided me with the assumption that one no longer needs an API key to make Google Maps work on your page. Now, I tried it out and it worked. But I guess we will have to see. Google does say not to worry, and that if a threshold does get reached, they will contact you. </p>

<p>I found out you can skate by with this:
    <code>&#39;https://maps.googleapis.com/maps/api/js?v=3.exp&amp;sensor=false&amp;&#39;</code></p>

<h1>Bootstrap and Google Maps do not play nice</h1>

<p>They don&#39;t. It&#39;s plain stupid. But no body has the time to figure out what exactly is messing with it. So there&#39;s a workaround until you have the lazy Sunday to look at it (let&#39;s be honest. You never will.) If you want to know what happens with Bootstrap and the map <code>&lt;div&gt;</code>, it&#39;s pretty easy to see. It looks stupid. And there will be one tiny square of a map in the upper left hand corner of the div, and no amount of dragging is going to save you. Nope, the best way to go about it is to load this piece of code in your initialize() function:</p>
<div class="highlight"><pre><code class="text language-text" data-lang="javascript">google.maps.event.addListenerOnce(map, &#39;idle&#39;, function() {
    google.maps.event.trigger(map, &#39;resize&#39;)
    map.panTo(marker.position)
})
</code></pre></div>
<h1>How to get the coordinates of an Address</h1>

<p>At first I thought it was going to be easy. Then I thought it was going to be hard. And then I just didn&#39;t know. But Google is your friend. Remember that. All you need to find the coordinates of a user typed address, is to use this link:
        <pre><code class="text language-text" data-lang="javascript">
        url = &#39;http://maps.googleapis.com/maps/api/geocode/json?address=&#39; + getCoordinatesAddress + &#39;&amp;sensor=true&#39;
            $.getJSON(url, function(data) {
                var location = data.results[0].geometry.location;
                // coordinates are location.lat and location.lng
                console.log(location.lat)
                console.log(location.lng)
            })
            </code></pre>
 As you can see, the <strong>url</strong> variable has a simple geocode address with <strong>getCoordinatesAddress</strong> in there. This was passed as a simple value. Spaces and all. Google couldn&#39;t care less. Note the jQuery getJSON call. You will have to parse the response, as you see with <strong>var location</strong>. And the console logs were there just for me to make sure I was getting the right data from the response. Console.logs are your friend.</p>

<p>That&#39;s it really. I was done in half a day and now have an implemented draggable pin in testing.</p>
