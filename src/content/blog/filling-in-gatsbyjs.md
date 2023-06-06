---
title: "Filling out the DashGL site redesign"
description: "Planning on expanding out the GatsbyJs Landing page into more sections"
pubDate: "August 15, 2021"
heroImage: "https://i.imgur.com/T2WlMNn.jpg"
author: Kion
---


Making some progress with the DashGL site redesign. I've managed to 
get the basic functionality of the tutorials working. So from the landing
page we have a list of the three most recent tutorials, and you can
change the page to a list of tutorials. 

From the list of tutorials, you can't filter which tutorial you are looking
for, but right now with a limited number of tutorials, that functionality
is not a high pririty. Once you click on a tutorial, you can see the table
of contents for that tutorial, and then you can click on a less to see that
lession specifically.

Now that I'm writing this post and thinking about it, I might change that
functionality. I have the slug as the table of contents, and the slug with
00 as the introduction. I might make the introduction the slug, and then
get rid of the table of contents page. Because when clicking on the tutorial,
you probably expect to be taken directly to the tutorial instead of 
having to go through two clicks to get where you expected to go in the first
place. So that's a change that will probably happen sooner than later. And
it also has the benefit of not needed to have a featured image and description
in the front matter. I can probably better be served included a short
description of each stap in the first chapter of the mark down as a preview for
the tutorial.

Which means for the tutorials, the next step will be to start to port over
all of the information from the previous sites into the markdown files on 
Github. Though right now I'm tempted into starting to fill out more parts
of the site. The other aspects of the site are: 

1. Blog
2. Static pages
3. Dash Model format

For the Dash Model format, it definitely provides the option to start writing
out the 2.0 standard. I think there are a bunch of small improvements that
could be made over the current 1.6 standard. And if I'm going through to make
the documentation for the pages, I might as well document for the 2.0 standard
going forward. Though if I make any changes to the standard, I will need a 
way to separate the documentation for the different standards. Which means
that I might copy and paste the 1.6 definition as-is. Also I'm not sure where
the documentation should "live". I'm tempted to make a Github repository, and
then separate the versions by branch. Not sure if I can do that for the wiki
or not. 

For the static pages we have

- Contact
- Support 
- Contribute 
- About
- License
- Mascot

These are one off pages, and even if I can't make them look nice.
I should at least get pages with the information in there, so they
can be addressed. For contact I should put my contact information. 
For support I should put my wallet addresses, so that people can 
support me that way, and I should probably also put roadmaps and
goals in there. Though if I have a decent job, I probably don't
need to be as forthcoming about support. I should probably stick to
more open-source options like LibraPay, and wallets, possibly 
ko-fi that are more one-time contributions and skip out on Patreon.

For the about, I can include what DashGL, what the motivations are
and why I started it. For the License I can include that all of
the tutorials and included under Creative Commons share-alike. The
source code in the tutorials and FreeBSD zero clause, and any libraries
included in the tutorials are under permissive license, such as
MIT. And last we include information on the mascot, specifically Dashie
and provide download links for the 3d models of her. 