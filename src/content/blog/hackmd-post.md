---
title: "First Note with Hackmd.io"
description: "Blog post from when I was thinking about switching from WP to Hackmd.io"
pubDate: "August 15, 2021"
heroImage: "https://i.imgur.com/mdc2Xc9.jpg"
author: Kion
---

I've been thinking about making updates to DashGL.
And I think the format that I want to use is split the web site
between a blog and tutorials. 

For the tutorials, I can have each tutorial have its own Github / Gitlab
repository. The root of the repository would have a readme file that contains
a table of contents for all of the steps and a abstract / description 
of the project. 

Each step in the tutorial would be given by a folder in the repository. Each
folder would have it's own read me that would serve and the article for that
step. 

I could them complile these into one tutorial that would be referenced from
the webpage. And that way when I work on tutorials, I could simply focus on 
doing the programming and writing the readmes for the repository, which 
I need to do anyways. And that should avoid having to go back and making the
web page as a separate project after the fact.

The tutorials should all be in numbered order for each step, and should
follow the order set by the table of contents, or otherwise by the step 
number defined in the name of the folder. 

For the blog, I think I was thinking the easiest approach would be use a
tool such as Hackmd.io. And this would allow me to create a blog, where 
more recent posts come first and then other posts come in reverse 
chronological order. 

And I think that should be a good balance that should allow me to easily
update the site. When I specifically want to focus on a tutorial, I can
focus on Github and the repository. And when I want to add a quick or
simple note, or sort my thoughts out, I can use the blog format to
add posts to the site.

What remains is the site itself. And recently I've found Bootstrap, and
React and are pretty impressed with the way they combine to break a 
website down into components for simplicity. I'n not sure if I would need
to roll-my-own, or if there is a template engine that already exists that
could work.

I specifically like the [half-height-carousel](https://mdbootstrap.com/freebies/carousel-half-cover/) template, and I think that's the general style I 
would go for on the front cover. A concept image at the top, followed by
the recent blog posts, followed by a list of tutorial cards. For each
tutorial we could use the [post template](https://mdbootstrap.com/freebies/post/).
Not sure if we would need something more complicated for the blog or not.

I think we generally need a list that scrolls until there are no posts,
with some indication of what the month and year are, and if there are any
tags on the post or not.

For the format, I think the easiest option would be to have a static site
generator, that way the site would be indexed from search engines, and
anyone would be able to reference the site even without Javascript. 
I think that if Javascript is enabled, then any page change, we grab 
the JSON object required to generate the page, and then hangle the history
object, so we aren't using more packets than we need to. But testing for
that can wait until we generate the site, and otherwise we should be 
looking into libraries like Gatspy to see if the functionality we're
looking for has already been packaged. 