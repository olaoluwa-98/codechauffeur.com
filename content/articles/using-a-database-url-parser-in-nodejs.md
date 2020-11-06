---
title: "Using a Database URL Parser in NodeJs"
date: 2020-05-24T09:13:37+01:00
draft: false
tags: [NodeJs,Database,12Factor,env,url]
images: ["images/database-url-parser/cover.png"]
---


One of the points in [The Twelve-Factor App](https://12factor.net/) is `Config`.

 An app’s config is everything that is likely to vary between deploys (staging, production, developer environments, etc). Most applications that require persistent storage use databases. Applications connect to databases using certain parameters and credentials which are usually stored in a type of `Config`. The database config usually includes a database driver, port, name, username and password.

You must have seen one/more of these env variables in projects:

- DB_CONNECTION
- DB_HOST
- DB_PORT
- DB_USER
- DB_PASSWORD
- DB_DATABASE

These separate env variables can be combined into a single variable using a URL parser. [parse-database-url](https://www.npmjs.com/package/parse-database-url) is a simple NodeJs library that parses database configurations passed in as URLs.

You can name the single combined variable `DATABASE_URL`. The structure of the `DATABASE_URL` is:
```sh
DATABASE_URL=driver://username:password@hostname:port/database_name
```

An example is:

```sh
DATABASE_URL=mysql://root:password123@localhost:3306/app_db
```

To use the library, simply install with npm:

```sh
npm install parse-database-url
```

Import the library and parse the `DATABASE_URL`:

```js
const parseDbUrl = require("parse-database-url");
const dbConfig = parseDbUrl(process.env.DATABASE_URL);
```

`dbConfig` is an object with the database configurations. You can apply this to wherever you set `Config` in your application.

```js
{
  "driver": "mysql",
  "user": "root",
  "password": "password123",
  "host": "localhost",
  "port": "3306",
  "database": "app_db"
}
```

By using this process, you reduce the number of env variables.

Note: There are some known issues with this method. If your database password contains some special characters, it'll break the parsing and the result will be inconsistent. Also, if you put a non-existing driver, the library does not validate it.
