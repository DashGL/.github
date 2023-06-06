---
title: "DashGL GatsbyJs Revisions"
description: "Looking into implementing the DashGL website as a GatsbyJs static site"
pubDate: "August 15, 2021"
heroImage: "https://i.imgur.com/BC3OqR2.jpg"
author: Kion
---
 


Right now I'm working on creating a Github pages hosted
version of the DashGL web site with GatsbyJs. I think that
this should allow me to separate managing the tutorials
with managing the web site. If I can set up the website around
a specific set of rules, I can focus on writing turorials
using MarkDown in Github Readme's and then I can simply
re-generate the site with new content. 

I've finished blocking out the site. And as we go along, I'll simplify
and get rid of tags that aren't really doing anything. Since we're done
with the layout, we can change our attention to the content.
And in terms of content, we have three categories, the blog, the tutorials
and the dash model format. And then we can work out what our sources
should be, and how to get content from those sources.

For the blog, we're going to be using Hackmd.io, because it
seems to be the easiest place to write markdown, and then display
it as a blog. However, the blog is our lowest priority. And it
might actually take the most time in terms of development, so 
we don't have to focus on this quite yet. Though we might use
the markdown as a sample source, and render a few blog pages. We'll
put off the search page depending on how much Gatsyby, with the combination
of React and GraphQL, facilitates writing an easy search and and filter.

Next we have the model format. And I'm tempted to think that I might not need
to have a source for this, in favor of writing all of this as static
content, since it's not likely to change for a while. We can create the
structure with static pages. And then if we figure out where and how
we want store this information for editing, then we can do it.

Which leaves the tutorials. And the easy answer is Github. And the more
complicated answer is how. I'm tempted to have some kind of JSON in
the repositiory that gives the structure of the turotial, and the order
of mark down files that are needed for the tutorial. I'm not sure if
there is a plugin that will do what we need, or if I'll have to take
a small detour to write a plugin.

I'm tempted to take the write-your-own plugin first, because I think
that will give me the most control over how I want to structure my data sources,
and how I compile them into a web page. But before I do that, I'll
make a list of potential existing plugins, and then look at the documentation
to see how intensive it is to write a plugin. 

Here's the documentation for how to create a Source Plugin: https://www.gatsbyjs.com/docs/how-to/plugins-and-themes/creating-a-source-plugin/

Though, it looks like there is a source Github: https://www.gatsbyjs.com/plugins/@mosch/gatsby-source-github/?=source%20github if it lists all of the files in the directory.

To make a quick note, there is a plugin for handling images.
Placeholder Images: https://www.gatsbyjs.com/plugins/gatsby-plugin-image/