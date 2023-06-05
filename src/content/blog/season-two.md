---
title: "DashGL Tutorials Season 2"
description: "Embracing SDL for DashGL Tutorials and Web Design Deliberations"
pubDate: "October 10, 2019"
heroImage: "/img/dgl-season2.jpg"
author: Kion
---


Now that I”ve finally settled on a new design for Dashgl.com, the next step is to start making tutorials. I’ve been thinking about if I want to continue with GTK or if I want to switch to SDL. And while the original focus of using GTK was to provide more resources for making Linux applications, recently when looking at the options for making applications for different open source platforms, my first thoughts have been “i wonder if SDL will compile for this”. So I think with season two of tutorials, I’ll try switching to SDL as it will run on Linux as well as the GTK applications will, gives me more options for OpenGL versions, and I wasn’t taking advantage of the file menu or widgets anyways.

For approach I’ve been thinking that starting with the cube might be the best way to get adjusted. My previous difficulties with SDL was managing the input and the game loop, but I think I’ve found a sample for that, so it’s not as much f a problem as it was before. I think I can use the FreeGlut introduction as a way to get familiar with SDL in a way that doesn’t require any input from the user, as well as providing an avenue to test the matrix library that I was thinking about using before. It also allows me to follow up with OpenGL 1.0 on Brickout and then OpenGL 2.0 for Invaders and Astroids respectively. And then from there I can plan out more tutorials and do other testing.

The next thing on the agenda is to follow the Freeglut book, make a new blog post for each chapter. And I have prior source to help me out in terms of fitting the pieces together. And then once all of the pieces are in place with the blog, then I can think of what I need to work on in terms of web site structure for the DashGL tutorials. Right now I’m thinking that the best way is to put the files in a static site generator, so that all of the html is a static html file, and nothing needs to run server-side to render the pages in advance once they’ve been generated. I think that gives me the easiest option for making clean looking links.