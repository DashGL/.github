---
title: "Monthly Challenges"
description: "Setting a list of tech spikes to focus on per month to try and get hired"
pubDate: "September 3, 2021"
heroImage: "/img/Tech-Spikes-1024x640-4041206566.png"
author: Kion
---

One issue that I run into with programming, is that I always feel busy. I always feel like there’s something that needs to be done and I don’t have enough time to do it. And I’m always on the computer doing _something_. The problem is that when I finish that something or move on to the next thing, I can never remember what it was that I was doing, or what felt important at the time. It feels like such a huge waste of time, that I generally would prefer to be reverse engineering old games, but instead I’m “busy” doing something else. And if I can’t justify what that something was, then why wasn’t I investing it in reverse engineering games?

My plan to approach this is to focus on monthly challenges. The idea is that each month will be dedicated to a specific topic. and I can justify why I’m focusing on that specific topic. Define how far I want to get. And why I’m working on it over other random side projects. And then be able to track what I was doing for each month, so I don’t forget it as soon as its done.

### Pre-July 2021

Before I broke my challenges into focus for a clear month, I started Whitmir.io, with the intention of making a portfolio piece to get into Freelance. I ran into several challenges with what I expected to be a straightforward side-project. The main issue that I ran into was the rich text editor. The design behind the project was to make a simple blog editor, where a user could create simple documents and then compile them into a book which could be exported in some e-book format.

The issue I had with the rich text editors is that I wanted to have more control of the design and it didn’t fit with how the different libraries I used were structured. And I ended up spending about two weeks going through different options before I reached the end of the month and decided that I had to move on.

In terms of what I learned from this project, I got some hands-on exposure to OAuth and Cors. The idea was to host a strictly static page that could be hosted on Github for free, and then users could login with a Google id, and then documented would be written to and from their Google drive account, to test out the idea of a “server-less” application. Overall that aspect of the application seems to work out. I think I would keep settings for the application, and document for the application in two separate folders. One for the meta data, and the other for documents.

Another option would be if hackmd.io releases an API to interact directly with that. I think a markdown editor might make a lot more sense than a rich text editor in that it makes the format a lot easier to work with. Something that I wasn’t sure about was how to manage the preview for a markdown editor. I think if the screen is big enough to offer a split screen. Otherwise it might be a good idea to split writing text with a button to switch to preview mode.

I think in retrospect that if I approached this again, I would split it up into a few more smaller parts. Make a page that only interacts with Google drive to build a book and pages. Make a page that only interests with Google Drive meta data. Make several different pages and test different options for editors and then have the ability to design around the editors. And then from there I would go back to the drawing board and implement the design. In general through it looks like hackmd.io does a pretty good job of being and editor, and also has the option to export to odf. So the priority for coming back to work on this side-project is quite low.

### July 2021

This was my first “official” monthly challenge. I spent a month to use Shellshop v2 as a way to learn react and TypeScript. And also took the opportunity to use Golang on the back end. What I learned from this is I should be using NextJs for routing and having the option to create and API on the back end. And learned I should be using redux for the model.

### August 2021

Originally my plan for this month was to make an updated version of the DashGL web page using Gatsby. This was changed when I applied for a company. And my homework became the subject of my monthly challenge. The challenge was to get familiar with did’s by creating a flow where a did is created, it’s verified by a third party. And then another party check the validity of the verification.