---
title: "My Experience With Rebuilding a Product from Ground Up"
date: 2020-10-20T07:30:47+01:00
draft: false
images: ["images/experience-rebuilding-a-product-from-ground-up/experience-rebuilding-a-product-from-ground-up.png"]
tags: [Product,Refactoring,Startup]
---

I joined my present company ([Pettysave](https://pettysave.com)) mid-2019 after concluding my undergraduate studies. In my first few weeks, I didn't feel like I was in a tech company. The business product was initially built by an outsourcing agency. In my opinion, they didn't do a good job with the product. Of course, I wasn't present during the process of developing the product so I did not know how they got there. One thing was sure, the product was very problematic and needed an upgrade.

## Problems With Outsourcing

Outsourcing the development of your business product is not bad but before doing it, you should definitely know the pros and cons. It is usually an easy and cheap short-term option.
Apart from the problems I personally experienced with the outsourcing company that built the initial version of Pettysave, I also did a little research and found that it's an often discussed issue globally. Instead of listing the problems, you can just read these really good articles. They explain what I'd have written here:

- [Five Pitfalls To Avoid When Outsourcing Software Development - Stackoverflow Blog article by Rahul Varshneya](https://stackoverflow.blog/2019/10/01/pitfalls-avoid-outsourcing-software-development)
- [The Challenges of Outsource Software Development - Medium post by Daniel Alcanja](https://medium.com/@danielalcanja/the-new-era-of-outsourcing-software-development-c5c4099342b3)
- [How to solve 13 harsh problems of outsourcing - N-ix article by Nadia Serheichuk](https://www.n-ix.com/how-to-solve-problems-of-it-outsourcing)

## Initial Worries

When I joined the team, I had only one engineering teammate. My teammate was a frontend engineer while I was a backend engineer. I remember being very concerned with the product and how we could make it better. Pettysave management hired two of us to take over the product from the outsourcing agency. After several discussions with the outsourcing agency and my teammate, we decided we'd rebuild the product and rewrite the entire codebase.

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

After deciding the new stack, the next piece of the puzzle was how we were going to migrate the database. This is a very important step that should be done early in the process because it determines if you can move forward or not. We were migrating from Microsoft SQL to MySQL. Luckily for us, they are fairly similar but they have lots of differences regardless. I spent time researching the best approach to solve this. The structure of the old database wasn't the best so I had to make modifications to make it more normalized and to track more information.

## Migrating the database

Because this step was very important, I had to be very careful. I had a few concerns:

- Changing the structure of the database without destroying old data
- Migrating the passwords of users i.e rehashing to fit new hashing algorithm in version 2
- Generating new data to fit new database structure i.e populating new tables or columns created in version 2

I eventually found a tool that could migrate a database in MSSQL to MySQL: MySQL Workbench. You can read this article to know how it works: [How to Migrate from MSSQL to MySQL by Sebastian Insausti](https://severalnines.com/database-blog/how-migrate-mssql-mysql). After several trials, I was able to convert the database to MySQL. The next step was to transform the MySQL database into the new data structure for version 2.

This proved to be a very hard task because I had to write a script for it and test it several times. This took weeks, no kidding. I didn't spend the whole day working only on the script, I was still writing the backend for version 2 alongside. I got frustrated a lot during this process and even scared sometimes.

## Writing the Backend

As I said earlier, I had to rewrite the whole backend infrastructure from the ground up. It was fairly easy because I learnt from the current version of the product and improved on it. I used [Adonis.js](https://adonisjs.com/), a Node.js framework. Adonis.js is a really simple framework to use, it was modelled after [Laravel](https://laravel.com/). It had most of the things I needed to write a savings & investment backend:

- Routing
- Database ORM
- Middleware
- Mailing
- Validator
- Authentication
- Encryption & Hashing
- File Upload
- Redis
- Events
- Testing

I enjoyed most of the time I spent writing the backend code.

## Handover

When I started working on the backend, Pettysave management had requested for the codebase of version 1 from the outsourcing agency. We got the codebase weeks later. So I was able to extract more information from the code. I saw lots of mistakes they made and thought of ways to correct them in version 2.

Apart from the codebase, the outsourcing agency had our domain name, hosting, database and some API keys. So we started the process of retrieving all these. The hardest one to retrieve was the domain name because it was bought in their name so they had to do domain transfer, it took a while to do this. We didn't need the hosting because we weren't going to deploy into a Windows environment, we discarded all old API keys they had access to prevent any funny thing that may occur in the future.

Once we got everything, we were more confident about the launch and to continue the maintenance of the platform.

## Test & Launch

After about 3 months of working on version 2 with my team members, we were set to launch. Others were set to launch, I wasn't. I was yet to finish the database migration script. I focused more on the script and spent much time on SQL. I learnt so much about SQL in that period. When I completed the script and tested it, we were now ready for launch. We had previously shifted the launch date like 4 times. We eventually settled on January 17th 2020. I had deployed a test version of the product so we could be testing internally. That had been going for weeks, this process is very important. It's easy to know if a product was tested before launch. A tested product is more usable than a non-tested one. It's that simple because, during the test, people simulate how users will use it and give feedback on how to improve the usability of the product.

On the launch day, of course, I was still making a lot of edits to the backend and working on the deployment. I had chosen AWS for hosting. So I made use of Docker and deployed to AWS Elastic Container Service (ECS). I wrote an [article about deploying to ECS](https://codechauffeur.com/easy-deployment-setup-with-bitbucket-and-aws-ecs/). Version 2 of Pettysave went live at about 11 am. Poof! that was it.

## Launching Mobile Apps

Version 2 of the website was at [app.pettysave.com](https://app.pettysave.com). We used Cordova to bundle the web application into a mobile app. We submitted the apk for the Android version to Play Store later that day. Releasing on iOS was a longer process. We had to first apply to the Developer account of App Store to get approval. That took about a week before we were able to submit the app on App Store.

## Problems After Launch

Once we launched, we initially experienced a lot of problems with the servers. I had provisioned a very small EC2 (Elastic Compute Cloud) instance so the server kept crashing several times day. I had to stay up all night to ensure it was stable.

Due to the changes in the data structures on the database, there was some information I had to generate. Some customers complained that their data wasn't consistent with the old version, I had to do checks to ensure they were all corrected.

Alongside the API server, we had a task runner (cron job) that was responsible for debiting customers when it's their turn to save. Initially, I had written a simple loop to get all the debit cards and made API calls to our payment processor to charge the card. This was a very bad idea because one error stopped the whole loop and that was occurring every day after launch. I had to do some research and speak with my friends on how to solve this, I eventually used a queuing system to solve it (I'd write an article about later).

The deployment was manual at launch, I had built the docker images on my PC and push to AWS  ECS (Elastic Container Registry). After a week or more, I set up continuous delivery on Bitbucket so that whenever the master branch was updated, a build is triggered and deployed without any manual process. This saved me a lot of time and headache. I explained how I did this in [this article](https://codechauffeur.com/easy-deployment-setup-with-bitbucket-and-aws-ecs/).

## After Launch

One thing was clear once we launched, we had to be ready to answer our customers' questions on complaints. We were ready, we had installed an instant chat service directly on the app. We received lots of complaints about the new changes. Many customers were happy with them, some weren't but it was a generally positive response. We also set up our support mail [support@pettysave.com] which I handled. I handled the complaints from customers for several months. This made me have a deeper understanding of what to build and what's important to customers.

It's been 9 months since then and the progress we've made has been fantastic. We acquired more users in the first 3 months of version 2 than the first one year of version 1. That's more than 100% increase in user growth. Revenue also went up and is still going up.

## Lessons I Learnt

I really loved the experience of working a product from the ground up. The joy of building something that adds value to people is really great. Rebuilding a product gives you flexibility and allows you to correct old mistakes and make new and better approaches to solving old problems. The problem the product was solving didn't change, the product did.

- You cannot optimize the code you haven't written - until you write the code before you can optimize it
- Focus on the most important aspects of the product - there are many other aspects that are not so important and could distract you
- Listen to your customers, your customers tell you their problem - it's your job to figure out the solutions to their problems
- Prepare for failure - when building a product, you should know that things go wrong so be prepared for this
- Iterate fast - receive feedback from customers and use it to build and iterate new features
- Measure progress and success - before improving a product, you should have success metrics you'd use to determine if your new version is successful
- Chill - there will always be work to do, don't overwork yourself, have a plan and include rest in your plan
- Celebrate - celebrate your wins, after releasing new features you should celebrate as it grants you the reward for your work

Pettysave is an online savings and investment platform that helps people achieve their financial goals, you can sign up with my referral link to receive a **NGN250** bonus when you start saving. Sign up here: [app.pettysave.com/s/EMMA16522](https://app.pettysave.com/s/EMMA16522)