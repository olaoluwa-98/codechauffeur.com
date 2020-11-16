---
title: "How to Write Tests in Adonis.js"
date: 2020-11-16T05:12:08+01:00
draft: true
images: ["images/how-to-write-tests-in-adonis-js/how-to-write-tests-in-adonis-js.png"]
tags: [NodeJs,Testing,Unit Tests, Functional Tests,AdonisJs]
---

Testing is one of the popular things people often talk about but don't do that much. There are people who would boast of having 100% coverage and there are people who would say they don't write tests. Which ever side of the argument you're on, you should know that testing has its benefits.

My rule for testing is to treat testing like an investment. Try to consider the returns for the test before writing it. If the returns are higher than the cost, then it's definitely worth it. Although there are aspects of your project you may have to compulsorily test.

In this article, I'd show you how to write tests in [Adonis.js](https://adonisjs.com/). Adonis.js comes with its own testing tools.

Note: *this article is written for Adonis.js v4.1*.

## First step

Install @adonis/vow if you don't already have it in your project.

```sh
adonis install @adonis/vow
```

Next, register the provider in the `start/app.js` file aceProviders array:

```js
const aceProviders = [
  '@adonisjs/vow/providers/VowProvider'
]
```

After `@adonis/vow` is installed, the following are created:

- **vowfile.js**: this file is loaded before your tests are executed. It's used to define tasks that should occur before/after running all tests
- **.env.testing**: this file contains the environment variables used when running tests. This file gets merged with .env, so you only need to define values you want to override from the .env file
- **test**: a directory where all test files are stored

## Writing tests

The setup for tests on Adonis.js is really simple. After the steps above, you're ready to start writing tests. There are two types of tests Adonis.js offers:

- Unit tests - written to test small pieces of code in isolation
- Functional tests - written to test your application like an end-user

You can learn more about these [in the docs](https://adonisjs.com/docs/4.1/testing)

<!-- Depending on your application. You may need to connect with a database, if you don't need it then its fine to omit it. -->