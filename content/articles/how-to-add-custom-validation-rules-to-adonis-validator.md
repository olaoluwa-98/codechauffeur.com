---
title: "How to Add Custom Validation Rules to Adonis Validator"
date: 2020-03-16T23:53:32+01:00
draft: true
tags: [nodejs,validation,adonis]
---

If you are reading this post, I assume you must have used *AdonisJs* or you must have heard about the framework. 
[AdonisJs](https://adonisjs.com) is a NodeJs framework that was inspired by Laravel. The framework was modelled after [Laravel](https://laravel.com), if you have worked with Laravel, you will find AdonisJs very familiar.

Like most robust frameworks, *AdonisJs* comes with a validator that helps you in validating data on your server. However, the default validator does not come with every possible rule, you sometimes have to implement your own. The good news is that it is very easy to write custom rules and add them to the validator. In this post, I will walk you through how to do that.

First, we have to setup our application - let's call it *<name>*. To start, you have to install `adonis` globally with ` npm i -g @adonisjs/cli` then create a new application with `adonis new <name>`. Check [installation page](https://adonisjs.com/docs/4.1/installation) for more information.
Here is what our folder structure will look like:

```
├── app 
│   ├── Models
│   ├── Validator
├── config 
├── database 
│   ├── migrations 
│   └── seeds 
├── public 
├── resources 
│   └── views 
├── start

```
Start the application server by running `adonis serve --dev`. You can open the application on `localhost:3333`, Adonis serves on port 3333 by default. You should have this page in the image below if everything ran correctly.
IMAGE

Now, let's create a page that collects data. We'll edit `start/routes.js`.

Add this line to the file:
```
Route.get('/check-character', 'CharacterController.showPage').as('check-character')
```

We have added our `/check-character` route, now we have to create our `CharacterController` controller to handle this route. We'll do this by running: `adonis make:controller CharacterController --type http`. Great!, now we have `app/Controllers/Http/CharacterController.js`, let's create our `showPage` method to handle the route.
Put these in `app/Controllers/Http/CharacterController.js`:


Create the view for the route: `adonis make:view check-character`. This will create ` resources/views/check-character.edge`

Put this in the check-character view.
