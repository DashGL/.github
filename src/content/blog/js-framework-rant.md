---
title: "Rambling about Javascript Frameworks"
description: "Quick rant about how frameworks can the domain model from the programmer"
pubDate: "October 5, 2020"
heroImage: "/img/JavaScript-Frameworks-1024x576-827921780.jpeg"
author: Kion
---


– Typescript  
– React  
– Anglular  
– Framework 7

I’ve been looking into switching jobs recently. And while most of my freetime is spent analyzing file formats, my day job is working on web applications. In general my philosophy for web is that I want as little “magic” as possible between me and the code that I’m writing. The easiest example of this is jQuery. Back in the 2000’s there were still a lot of differences with browsers, and jQuery was the hottest library that managed the differences between the browsers, and did it with short and generally intuitive syntax. The problem was that when ever you searched, “how do i do x in Javascript?”, you would get 20 pages of results about jQuery and you had to filter through those to try and find any mention of how to implement what you wanted in vanilla Javascript.

And my issue with jQuery was that while it was a simple and effective way to write code, I didn’t want to have something that was interpreting Javascript for me. Sure it might be easier to write `$(‘#elem’).removeClass(‘active’)`, than it is to write something like:

```javascript
var elem = document.getElementById('elem');
var class_text = elem.getAttribute('class');
var classes = class_text.split(" ");
var index = classes.indexOf("active");
classes.splice(index, 1);
elem.setAttribute('class', classes.join(" "));
```