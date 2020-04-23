---
title: "Debugging Adonis With Chrome DevTools"
date: 2020-04-23T09:00:00+01:00
draft: false
description: "Debugging Adonis or Node.js can be easier using the debugger keyword"
images: ["images/debugging-adonis-with-chrome/cover-image.png"]
tags: [Node.js,Debugging,AdonisJs]
---

Debugging Node.js can be very difficult. Most times when something goes wrong it would take a while to get to the root of the problem because the error message may not be descriptive enough or
there's a variable that you expect to have a value but it's `null` or `undefined`.

The first thing that comes to mind when debugging in Node.js is usually to use `console.log`. This usually will get you a solution but in the long run, it wastes time.
We've all been in a situation where we had to use `console.log` multiple times because once was not enough. In a bid to quickly solve the problem we spend more time logging at different
points of the application.

But it does not have to be that way. There's a much simpler and more effective way of debugging in Node.js. It's by using a very simple keyword:

```js
debugger;
```
****
As simple as this is, many people do not know about it. The keyword is more popular for Javascript on the browser because it doesn't need anything more than just typing it anywhere in your code.
In Node.js it's a little different, you have to do more than just typing the keyword. To enable debugging in Node.js, in addition to adding `debugger;` in your code, you have to run the application using:

```bash
node inspect <file_name.js>
```

There are other ways to debug Node.js aside from `node-inspect`, some are:

- **Chrome DevTools**: I'd expand more on this later in the article.
- **Visual Studio Code**: It's the most effective if you already use VS Code.
- **Visual Studio**
- **Some JetBrains IDEs**
- **Eclipse IDE**

## Debugging with Chrome DevTools

We'll be using [AdonisJs](https://adonisjs.com) (a Node.js web framework) as an example. To enable debugging mode on Adonis run:

```bash
adonis serve --dev --debug
```

![Adonis debug](/images/debugging-adonis-with-chrome/adonis-debugger.png)

This debugger makes use of **Chrome DevTools** so head over to Chrome (If you have it installed). You should see the Node.js icon when you enable the DevTools. Clicking it will open a new window for debugging.

![Chrome dev tools](/images/debugging-adonis-with-chrome/chrome-dev-tools.png)

Whenever the execution of your application encounters the keyword `debugger`, it'll automatically pause execution and open the Chrome debugger window. You can do a lot of things in this window:

- See the value of scope and global variables
- View the call stack: allows you to see all the functions that were executed up until the current function being executed
- Resume execution
- Step over to the next function call
- Step into the next function call
- Step out of the current function
- Pause execution where exceptions are thrown
- and many more

Voila! That's it. With this, you can easily debug your AdonisJs application and not have to `console.log` everywhere😂.

You can check [Node.js docs](https://nodejs.org/api/debugger.html) for more information on debuggers.