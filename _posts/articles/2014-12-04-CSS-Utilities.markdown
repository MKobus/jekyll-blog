---
layout: post
title:  "CSS Utilities: Making Code Easier to Deal With"
date:   2014-12-04 22:33:16
categories: CSS Sass
---

###I recently read an article on the importance of utility classes in CSS.

See <a href="http://benfrain.com/enduring-css-writing-style-sheets-rapidly-changing-long-lived-projects/" target="_blank">article</a> by Ben Frain.

Ben Frain has a lot of valid and interesting points. After I read the article, I walked away and mulled on it for a while.

After a couple projects, I'd come to the realization that utilities were gold mines. At our company, we primarily use Bootstrap for anything new we create. Bootstrap is loaded with utility classes, such as "text-center", or "pull-left". But those are very generic. But even then, they still make my day developing sites, just a little easier.

And then I inherited another project, that was all sorts of messy code. Aside from the fact that it was pure CSS (We are a Sass house, and when I see CSS stylesheets that a developer worked in, I cringe), it was a mess and finding the root cause of the styling problems just made me miserable.. It becomes a war zone and a shot in the dark "grep", hoping that you find exactly what you're looking for.

And although Sass cleans up 70%ish of this problem, making your code a lot easier to read and maintain, you still have little issues here and there that could just save you some time in the long run. And maybe not just yourself, but a new team member as well. So I decided to start using utility classes for the sake of everyone.

### Deciding to do and actually implement are twi different things

What if we want to get a little more detailed? For example, a recent design we got (for a site that will be live in a couple days: exciting!), had social icons placed in several locations on the site. But they all utilized the same assets. Normally, based on the past, I would have styled each separate group. In the end, it becomes 2x more code than needed.

So I approached it a little differently. I had this little bit in a navbar, and something resembling this in the footer of the site:

{% highlight html %}
<ul class="nav navbar-nav navbar-right social-links hidden-xs">
  <li class="icons twitter"><a target="_blank" href="https://twitter.com/ZiftrCoin"></a></li>
  <li class="icons reddit"><a target="_blank" href="http://www.reddit.com/r/ziftrCOIN/"></a></li>
  <li class="icons google"><a target="_blank" href="https://plus.google.com/112307351927891911137"></a></li>
  <li class="icons email"><a target="_blank" href="mailto:?subject=I wanted you to see this site&amp;body=Check out this site http://www.ziftrcoin.com."></a></li>
</ul>
{% endhighlight %}

Note that the unordered list has a class name of "social-links" and each list item has a utility class of "icons" and then a name resembling what the icon is.

It's pretty straight forward, but it gives you an idea of how you can approach other situations. To style "social-links" in all places on the site, I created an included Sass file named __utilities.scss_ and placed it near the top of the hierarchy include list.

{% highlight sass %}
.social-links {
  list-style-type: none;
  margin-bottom: 0;
  padding-left: 0;
  li {
    display: inline-block;
    height: 23px;
    width: 24px;
    margin: 10px 5px;
    cursor: pointer;
    &.icons {
      background-image: url(../images/social_nav_sprite.png);
      background-repeat: no-repeat;
      background-size: 98px 48px;
      &.twitter {
        background-position: 0px 0px;
        &:hover { background-position: 0px -24px; }
      }
      &.reddit {
        background-position: -24px 0px;
        &:hover { background-position: -24px -24px; }
      }
      &.google {
        background-position: -49px 0px;
        &:hover { background-position: -49px -24px; }
      }
      &.email {
        background-position: -73px 0px;
        &:hover { background-position: -73px -24px; }
      }
    }
  }
}
{% endhighlight %}


Now, I can go and place an element with the class name "social-links" anywhere, along with a list item with the class names "icons twitter", and it will use the utility specified sprite and position.

This approach isn't limited to just images or sprites. I've used it with sections on a site that have mostly identical styling, section titles, even button elements. It just has to become habit.

Am I perfect at this? No. It takes a lot of time and practice to remember to create and use utility classes. So far, probably 50% of the code on this upcoming site could have used utility classes and we are only doing it for a fraction of that. But it's something I'm actively working on doing more often. But sometimes things like creeping deadlines and muscle memory cause you to create unelegant code. It's a lot of hard effort to get into the mindset of doing this.

But it's something nice to strive for.
