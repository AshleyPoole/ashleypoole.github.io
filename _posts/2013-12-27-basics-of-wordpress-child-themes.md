---
layout: post
title: Basics Of Wordpress Child Themes
date: 2013-12-27 15:08
categories: [general]
---
Well I have to start this post off by admitting I've been doing Wordpress theme modifications completely wrong! Overtime I had slowly gotten into the bad practice of modifying the parent themes directly to achieve what I wanted which had been working out though with one big drawback.  Theme updates.
<blockquote>Updating the parent themes directly started with just the odd little tweak here and there, though as time went on it quickly snowballed.</blockquote>
Updating themes on various websites I either owned or hosted and managed for people became a headache which in turn lead to infrequent updates purely because updating was a time consuming process as well as remembering which files I had modified. (Luckily I always made a backup of the theme file before I modified it and suffixed it with .BAK, so finding these theme files weren't too hard).
<h4>The solution? Wordpress Child Themes.</h4>
I have now succumb and fully embraced child themes in Wordpress. Gone are the days I fear updating the parent themes and having to redo all my tweaks and now I'm free to update the parent theme as often as I like with the authors or communities changes.

To start using a child theme, create a new folder within your /wp-content/themes/ folder with your active themes name suffixed with '-child'. I.e if my active themes name were 'twentyfourteen' then my child themes folder would be 'twentyfourteen-child'. For the remainder of this blog post, I will be using twentyfourteen as my theme name so you will need to substitute this for your own.

Even if you don't want to modify the CSS (Style) of the website you must create a style.css file within your child theme for Wordpress to correctly recognise it. Create a file new within /wp-context/themes/twentyfourteen-child/ called style.css with the below content.

{% highlight css %}
/*
 Theme Name:     Twenty Fourteen Child
 Theme URI:      http://www.ashleypoole.co.uk
 Description:    Twenty Fourteen Child Theme
 Author:         Ashley Poole
 Author URI:     http://www.ashleypoole.co.uk/about-ashley-poole/
 Template:       twentyfourteen
 Version:        1.0.1
*/

@import url(&amp;amp;quot;../twentyfourteen/style.css&amp;amp;quot;);

{% endhighlight %}



Now goto the Dashboard &gt; Themes and active your child theme. Note - Doing this may loose certain theme settings like the logo, etc, so you may need to reconfigure these.

Now that we have our style.css within our child theme we can make any tweaks without those getting overwritten when the parent theme is updated. Simples.

As well as overwriting the styles you can also override an entire file within the parent theme. A personally example I find myself often repeating is to overwrite footer.php with a empty file within my child theme which as the affect of removing the footer from the website. For example, here's the <a href="https://github.com/AshleyPoole/WordpressCustomisations/blob/master/Child%20Themes/quark-child/footer.php" target="_blank">footer.php</a> file for <a href="//ashleypoole.co.uk" target="_blank">//ashleypoole.co.uk</a>.

There are plenty of other tweaks that can be made using child themes. For more information I recommended viewing the below page from Wordpress on child themes. Url in caption of image.

<img class=" wp-image-762  " alt="Wordpress Child Themes" src="{{ "/img/2013/12/wordpress-childthemes.jpg" | prepend: site.assetsbaseurl }}" width="479" height="404" /> <a href="http://codex.wordpress.org/Child_Themes" target="_blank">http://codex.wordpress.org/Child_Themes</a>
