---
title: "My Experience With Rewriting a Codebase from Scratch"
date: 2020-10-17T07:26:47+01:00
draft: true
images: ["images/experience-rewriting-a-codebase/experience-rewriting-a-codebase.png"]
tags: [Product,Refactoring]
---

I joined my present company ([Pettysave](https://pettysave.com)) mid 2019 after concluding my undergraduate studies. In my first few weeks, I didn't feel like I was in a tech company. The business product was initially built by an outsourcing agency. In my opinion, they didn't do a good job with the product. Of course, I wasn't present during the process of developing the product so I did not know what went down to produce the product at that time. One thing was sure, the product was very problematic and needed an upgrade.

## Problems With Outsourcing

Outsourcing the development of your business product is not bad but before doing it, you should definitely know the pros and cons. It usually one of the easiest and short-term cheap option.
Apart from the problems I personally experienced with the outsourcing company that built the initial version of Pettysave, I also did a little research and found that it's a highly discussed issue globally. Instead of listing the problems, you can just read these really good articles. They explain what I'd have written here:

- [Five Pitfalls To Avoid When Outsourcing Software Development - Stackoverflow Blog article by Rahul Varshneya](https://stackoverflow.blog/2019/10/01/pitfalls-avoid-outsourcing-software-development)
- [The Challenges of Outsource Software Development - Medium post by Daniel Alcanja](https://medium.com/@danielalcanja/the-new-era-of-outsourcing-software-development-c5c4099342b3)
- [How to solve 13 harsh problems of outsourcing - N-ix article by Nadia Serheichuk](https://www.n-ix.com/how-to-solve-problems-of-it-outsourcing)

## Initial Worries

When I joined the team, I had only one engineering teammate which made just two of us. My teammate was a frontend engineer while I was a backend engineer. I remember being very concerned with the product and how we could make it better. Pettysave management hired two of us to take over the product from the outsourcing agency. After several discussions with the outsourcing agency and my teammate, we decided we'd rebuild the product and rewrite the entire codebase.

The initial version of the Pettysave product was built with:

- ASP.NET (Onion Architecture) - For the website
- Microsoft SQL Server - Database

I had no experience with the .NET stack and generally did not like it anyway. It was a no-brainer for me to start learning what I didn't like. I spent a week or more doing research on which stack we were going to use. I read a lot of articles and talked to friends.
After much research, I had two options for the backend stack: Node.js or Python. The frontend stack was decided on time: Vue.js. After looking at my current skill-set, I decided to go with Node.js.
This eventually became the new stack for Pettysave:

- Vue.js - Frontend
- Node.js (Adonis.js) - Backend
- MySQL - Database
- Redis - In-memory Database

After deciding the new stack, the next piece of the puzzle was how we were going to migrate the database. This is a very important step that should be done early in the process because it determines if you can move forward or not. We were migrating from Microsoft SQL to MySQL. Luckily for us they are fairly similar but they have lots of differences regardless.