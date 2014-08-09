---
layout: post
title:  "CSS and JavaScript Resizing Based on Window Width: Let's play nice"
date:   2014-08-06 12:41:16
categories: Javascript CSS
---

### Looking back on this issue, there probably was a simpler way to do this.

I could have made this all CSS. I could have made this all JavaScript. But, there were other factors that prohibited me from messing around with this inherited code too much. We had a deadline and a limited amount of hours. That's what happens when you have clients. And when you're hard pressed for time and looking at the code for the very first time, you kind of just wing it.

I do however think this was elegant enough. So I will post it.

### To explain the issue:
The client wanted to have a video implemented on their website. It had to be a popover, and it was embedded with Vimeo. All fantastic, and as it is a responsive site, my job was to make sure the video did just that, respond to the browser size.

I *originally* thought I had it figured out. I had a modal (handmade because the guy who previously made the site, did it responsive by hand, no bootstrap), and I had the video embedded in the modal. And I had CSS media queries setting the width of the video container.

{% highlight sass %}
 .video_container_inner {
                padding-top: 15px;
                @media screen and (min-width: 1200px) {
                    width: 800px;
                }
                @media screen and (max-width: 1199px) {
                    width: 640px;
                }
                @media screen and (max-width: 959px) {
                    width: 500px;
                }
                @media screen and (max-width: 639px) {
                    width: 320px;
                }
                #ek_video {}
            }
{% endhighlight %}

Did that, fantastic. Come to find out, a lot of video hosters don't really provide you with a responsive video (facepalm). So I had to go and write that one up too. Here, jQuery comes galloping in, my white knight! And we used:

{% highlight javascript %}
	$(window).width();
{% endhighlight %}

### Let me tell you right now
Do not attempt to use this. Just don't. May work all fine and dandy in your fancy Mac in all the browsers. I think it even worked in IE (don't remember). But it did not work in Windowz 7 Chrome.

And why is that you ask?

Because:

{% highlight javascript %}
	$(window).width();
{% endhighlight %}
or
{% highlight javascript %}
	$(window).innerWidth();
{% endhighlight %}

are not equal to

{% highlight javascript %}
	window.innerWidth
{% endhighlight %}

### Your CSS media queries are based on window width
Webkit does not include the scrollbars as part of the width, but Firefox, Opera and IE do.

What I thought was a beautifully functioning $(window).width(); query for the iframe player, was actually terrible in a certain version of Chrome. The reason is that although the developer tools were saying the width of the window was 1200px, $(window).width(); was showing a value 15px (or so) less. Obviously this was an issue, since the queries between CSS and JavaScript were not operating at the same time, causing the video the jump all over the page instead of sitting snuggle within

{% highlight html %}
<div class="video_container_inner"></div>
{% endhighlight %}

### Scrollbars are evil and Standards are Unicorns
This wouldn't be an issue if every browser treated Scrollbars equally, but as we know, standards are unicorns and that would make it too easy.

To fix this, I found a good little snippet on the web to get the actual width of the window:

{% highlight javascript %}
function getWindowWidth() {
		windowWidth = 0;
		if (typeof(window.innerWidth) == 'number') {
			windowWidth = window.innerWidth;
		}
		else {
			if (document.documentElement && document.documentElement.clientWidth) {
				windowWidth = document.documentElement.clientWidth;
			}
			else {
				if (document.body && document.body.clientWidth) {
					windowWidth = document.body.clientWidth;
				}
			}
		}
	}
{% endhighlight %}

This gets the same exact value as your CSS at the same exact time. Note that this no longer uses the jQuery knight who had come galloping in. It's plain JavaScript using window.innerWidth. This operates much differently and grabs the *right* values. TADA!

So when I plugged it in to this:

{% highlight javascript %}
function resizePlayer(){
	getWindowWidth();
	if ($("#ek_video").length > 0){
		if(windowWidth >= 1200){
			v_width = 800;
			v_height = 488;
		}if(windowWidth > 960 && windowWidth < 1200){
			v_width = 640;
			v_height = 390;
		}if(windowWidth > 640 && windowWidth < 960){
			v_width = 500;
			v_height = 305;
		}if(windowWidth < 640){
			v_width = 320;
			v_height = 195;
		}else if(windowWidth < 400){
			v_width = 300;
			v_height = 195;
		}

		$("iframe").attr("width", v_width);
		$("iframe").attr("height", v_height);
	}
}
{% endhighlight %}

my video was working right! No issues, no bugs, and the queries firing off at the same time.

The end.

