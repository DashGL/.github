---
title: "Thoughts on React and Typescript"
description: "An incoherent rant on React and Typescript that will not age well at all"
pubDate: "July 5, 2021"
heroImage: "/img/Screenshot-2021-07-06-at-00-22-39-ShellShop.png"
author: Kion
---

After making some initial effort to learn React and Typescript, I’ll write my thoughts here.

### React

I’ve only been using React for about a week now, but instantly I’m a huge fan and I can see the benefits of it. It allows you to break down different parts of the page into individual components. And allows you to write rules with JSX syntax instead of having to write _document.createElement_ over and over again.

This makes it easier to make changes, and easier to focus on individual aspects of the code. And it makes it easier to import components from npm. In addition the create-react-app also sets up webpack which makes it easier to include npm modules into the repository. And the template already includes placeholder for setting up a progressive web app.

So overall, I have a really good impression of React, I can see how it’s helpful. And I might even dare say, it’s the number one most required “framework” to use client-side. In terms of adjusting, I need to change my mind-set from traditional Javascript. Right now my workflow is making static html and css, splitting them off into modules in Javascript, grabbing the elements, adding event listeners, adding data in, and then at the very end adding render functions as needed.

With React a lot of the focus is on rendering. So I need to figure out how and when to fit data into the flow. But I think that will come as I get more familiar with the framework, and don’t have to think about it as much. Another point of improvement I need to make is including Bootstrap via npm, and then including it in the project in a way that makes sense. Which shouldn’t be too hard as there are probably blog posts and videos that I can reference.

### TypeScript

TypeScript is one that I’ve having a harder time getting adjusted with. And fundamentally I have a problem with the premise that you define types and the compiler will check the types for stupid mistakes for you. I think I’ve used JavaScript long enough that my brain does a lot of that without thinking, and it’s generally simple enough to convert a string to a number, or a string to a date when you need to, as you generally need to do that anyways when getting a value from an input elements, doesn’t seem like it’s saving you a lot of work.

One aspect that probably soured my experience is rather than trying Typescript on its own, I went straight to trying to use TypeScript with React. And this is a really aggravating experience as it’s rarely my types I’m defining, I need to know what types React is defining behind the scenes. So the compiler yells at me for everything. Passing a attribute into a component, bad. Trying to declare a object as a member of the class, bad. And the syntax is less than intuitive.

```javascript
import React from 'react';

interface AppMemory {
	app: React.Component,
	header: React.Component,
	gallery: React.Component,
	product: React.Component,
	checkout: React.Component
};

interface AppBridge {
	mem: AppMemory,
	register: (string, React.Component)=>void
};

const ReactBridge: AppBridge = {

	mem : {
		app : null,
		header : null,
		gallery : null,
		product : null,
		checkout : null
	},

	register : function(key: string, instance: React.Component) {

		console.log('Saving %s', key)
		console.log(this)
		//this.mem\[key\] = instance

	}
}

export default ReactBridge
```

Here I’m trying to throw in a quick hack, that will let me save instances from different components, and let me call functions on them if I need to. So I make an interface to store the memory, and each key is of type React.Component and that works.

And then the function, I try to define two arguments, the first that takes a string, and the second takes an instance of a React Component to save.

But the compiler doesn’t like this. I can use a `React.Component` in the attributes above, and it has no problems. But when I try to use it as an argument, I run into problems. This is probably a quick internet search away from an answer, but it goes to illustrate that while React’s syntax is new, it’s intuitive, it’s what you expect. In contrast, TypeScript’s syntax is frustrating because most of the time, I have no idea what it wants. And at least in terms of using TypeScript with react, editing every other line breaks something.

And all of this boils down to the benefits that TypeScript supposedly saves you headaches in production, by catching errors in development. But I still have problems with that, in that TypeScript generally boils down to a few basic types, strings, numbers, and dates. The thing is, generally the flow of Javascript is to get input from an input field, and throw it into a database. Or get a value from a database, and then render it on the screen. So generally data should be standardized by the JSON you throw to the backend, and get from the backend, and there shouldn’t be too many hops between them.

But the most annoying part I find about TypeScript is that, why doesn’t it provide more data types? JavaScript generally works fine because you’re working with a UI which is generally strings, dates, and a few numbers here and there. Strings and dates don’t need a lot of checking, it’s the numbers that Javascript plays fast and loose with. So if we’re taking Javascript from a loosely typed language to a compiled program that is going to yell at you for everything, why not add uint(8, 16, 32, 64, 128), int(8, 16, 32, 64, 128), float, double, decimal? So you can have more control over the numbers to be able to work with numbers directly in Javascript?

Something like:

```javascript
uint32 a, b, c;
a = 10;
b = 4;
c = a + b;
```

Should compile down into:

```javascript
const uint32 = new UInt32Array(3);
uint32[0] = 10;
uint32[1] = 4;
uint32[2] = uint32[0] + uint32[1];
```

Which seems like it would actually be a huge benefit in terms of being able to offer easy to read syntax for working with Javascript that is annoying to write. Because TypeScript is attempting to be a superset of Javascript, it means you have to append all of these extra gimmicks with semicolons after everything.

```javascript
let age:number = 5
```

In this case, this is kind of an extremely simplified example as age is already inferred as a number. But you have both let which declares a variable, and number which specifies the type of variable. Why not just declare the number instead of let?

```javascript
Number age = 5
```

In other words if you’re trying to take all of the ambiguity out of JavaScript and tell people not to use the “:any” type, because it DeFeAtS tHE PoInT Of TyPEScRipT, then why even have it in there? Just give me the option to make TypeSc ript to a completely hard typed language and stop spitting on my cup cake and calling it icing. Which brings me to the next part of my rant, that a lot of what TypeScript does can already be done in Javascript.

Like Enums. If you need to declare an Enum, you can declare a constant, and then add in key values to that constant, and then turn off the setter. Which is basically what TypeScript does internally anyways. If you need to sanitize some input you can sanitize input, or throw an error.

And my main complaint about TypeScript is that while TypeScript attempts to be a super set of Javascript, it doesn’t seem to be for JavaScript programmers. It seems to be for all of the other programmers who lost their jobs because the web programming took over the world, and now they complain that JavaScript doesn’t have tupples and enums instead of actually learning Javascript. Or it’s for front end coders who are too stupid to not know they shouldn’t be adding two strings together expecting the output to be an int.

So in summary, most of the benefits that TypeScript offers can be implemented or emulated to some degree in Javascript directly. The syntax is intuitive and adds in a ton of stupid fucking colons, and brackets every where. And if it’s going to yell at you for using playing fast and loose with vanilla Javascript, then it shouldn’t even give you the option.

And my last side rant about TypeScript is where you would use it. Because you can write JavaScript to be explict if you know what you’re doing. And React is the first priority for a client side library, so you pretty much need to use it with React if you’re going to use it on the client. And on the sever, if you really want to use a strongly typed language, you can use something like Golang. So in addition to not understanding what the benefits of TypeScript are, I don’t understand why you would want to use it in the first place. If you use React on the client side and Golang on the server side. Really seems like it’s for a bunch of chimps who sit in front of a keyboard, letting VS Code autocomplete everything for you.

I’ll keep making an attempt to try and learn it, but i keep having to try to hold my lunch down as I do.

[https://github.com/typescript-cheatsheets/react](https://github.com/typescript-cheatsheets/react)