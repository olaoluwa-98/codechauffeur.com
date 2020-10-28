---
title: "Building More Resilient Backend Infrastructure"
date: 2020-10-25T06:23:44+01:00
draft: true
images: ["images/building-more-resilient-backend-infrastructure/building-more-resilient-backend-infrastructure.png"]
tags: [Backend,Infrastructure,Architecture]
---

When building for the web, the backend infrastructure is usually the part that needs the most security and resilience. Over the years, the web has grown very large. There are billions of people surfing the web today. To put it in perspective, Facebook has over 2.7 billion monthly active users as of the second quarter of 2020 (1), Instagram has over 1 billion monthly active users (2), Twitter has over 330 million (3). Compare this to the current population of the world which is about 7.8 billion as of October 2020 (4).

So when building software for the web, you need to put this in consideration. Compared to the late 90's and early 2000's, whatever we are building today has the potential of being used by billions of people. Millions of people could be using our software simultaneously. To ensure we are building what can serve this high population, a lot of things have to be considered when building the infrastructure of the backend. No doubt, the frontend and other parts of a software matter just as much but in this article, my focus is on backend.

I have spent more than 2 years writing backend for softwares that serve tens of thousands of users, I have experienced a lot challenges and I can only imagine the challenges I'd face when the users of those softwares grow into hundreds of thousands, millions and billions. One thing I have realized is that in software, experience teaches you a lot of things you would not learn from trainings or tutorials. I thought to compile challenges that you can face in building for a growing population of users.

I got this idea while talking with one of my friends, Nicholas, an engineer at Paystack. At that time, I was confronting a challenge I encountered with an implementation I wrote for a particular part of the codebase I manage at my company. I spoke with him and he was able to profer a viable solution.

Whenever I encounter a major bug and it's taking more than 10 minutes or more to figure out, I decide to stop using the conventional ways of fixing bugs. If a bug is taking me too long to fix, it most likely means that I don't have enough knowledge in the domain of that that bug is showing up. For example, if I encounter a bug that has to do with reading files in Node.js and it's taking me more than 10 minutes to figure out then I need to back out from attempting to fix the bug. What I do is read a few articles or videos of working with files in Node.js. I also reach out to people who I know are experts in the domain. I could figure out what's wrong from this or just having the knowledge will streamline my search for how to fix the bug. This has long term benefits. This is just by the way.

Back to the point of this article. How do we build a resilient backend? This isn't dependent on the programming languages or frameworks you use. Writing code is an art, the more creative the artist is the better the work of art. In building a resilient backend, there a number of things I have found out from experience.

## What ever can go wrong will go wrong

This is a very relevant idea to have when building backend infrastructures. You should note things that as they come to your mind and ensure to come back to them. While coding, you may have thoughts that some implementation you wrote may be problematic or may break in certain situations. It's better to either stop and consider all the ways things can go wrong and handle them properly or to comment in the code that you have some concerns so you or anyone who looks through the code could attempt to fix it.

## Single point of failure

The larger your backend infrastructure, the more attention you need to give to it. You need to constant monitoring all the moving parts and ensure they are smooth. If you notice unusual traffic in certain areas, you should note them and attend to them. You should look out for bottlenecks and single points of failure. A single point of failure is a part of your infrastructure that if it fails, the whole backend infrastructure will not be able to function. You should either reduce as many single points of failure as possible or work very hard to secure and ensure they are very reliable.

## Automation

As your backend infrastructure grows bigger, you need to automate a lot of things you previously did manually. Some examples:

- running database migration
- running builds
- code linting and formatting
- backups
- deployment
- and many more

## Monitoring

Monitoring your how well your backend is performing will give you insights on how to improve it. You should use robust monitoring tools depending on how large your backend is. Tools like (Prometheus)[https://prometheus.io/], (Grafana)[https://grafana.com/], etc.

## Security

Take security very seriously from the first day. There are many areas to consider security

- Code - perform validation before processing data, add lots of checks to prevent malicious activities. You should also implement rate limiters to prevent DDoS attacks
- Secrets - you should protect all your secret keys
- Server & cluster - ensure you implement things like firewalls and other server related security checks
- Load balancer - apply the same security checks for your load balancers
- Database - ensure no one else can access your db. It's advisable to only allow incoming request from your servers alone to prevent brute force attacks
- Domain - when purchasing a domain, some domain providers offer extra security for your domain

You should consult experts in the security domain to help you tighten your loose ends.

## Routine checks

As an extension of security, you should perform routine checks on your severs to ensure everything is stable. You should have a schedule for this, it could be daily, weekly, monthly, quarterly or any one that works for you and your team. This could help you to catch errors on time. It keeps you informed and you can know how frequently certain errors show up and how you can prevent them.

## Code quality

Your codebase is being used by two sets of people. The customers it's serving and the developers that are working on it. A code that works is good for customers, a code that's readable is good for developers. Your code quality will help you iterate faster and build new features faster. You'd be able to have more developers work on the codebase because it will be easier to read and understand.

A lot of things go into ensuring high code quality. Things like documenting, commenting where needed, having good workflows (such as git flow), having a checklist before allowing code merge into the main branch.

## Automated tests

You should write tests to cover for as many scenarios as possible. This will make you more confident of your code and reduce the amount of manual tests you have to carry out. You should also add a test step to your continuous integration so no feature is shipped without passing its tests. This is extremely useful when refactoring implementations, you run the test before refactoring, you run the test after refactoring. This will help you know if you broke the code. Of course, you may not be able to write automated test for everything, there are times you have to test manually.

## Common database tricks

I have used SQL databases more than I have used NoSQL databases. Most of the common tricks in this list are for SQL databases.

- Transactions - there are certain flows in your application that needs to run a number of database writes that must all happen in a sequence and if one fails, all should fail. You can use database transactions to achieve this
- Racing conditions - a racing condition occurs when two database queries try to access and update the same row simultaneously. This causes computational inconsistencies. You can fix this problem using specific select queries. You can check this [stackoverflow question](https://stackoverflow.com/questions/2364273/how-to-make-sure-there-is-no-race-condition-in-mysql-database-when-incrementing) to learn more.
- Pagination - when fetching data from a user generated table, it's best to paginate because attempting to fetch all at once will be problematic as the table grows very large.
- Others - there are a lot more issues to consider when using databases. It's better to consider and implement them from day one.

## Timezone issues

## Failover, fallbacks, retries and rollbacks

- failover for cron jobs
- queues
- ability to rollback deployment
- constant feedback loops
- writing scripts to automate checks since data grows large quickly
- fallback or failover for third party api integration
  - exponential back off
- write more utility functions & middlewares to reduce duplication

Talk about reading articles from large companies that have solved problems you are facing - don't take everything they say as truth of course.

Talk to Nicholas and ask him to contribute to this according to his experience
Talk to Tochukwu and ask him to contribute to this according to his experience

References

(1) https://www.statista.com/statistics/264810/number-of-monthly-active-facebook-users-worldwide

(2) https://backlinko.com/instagram-users

(3) [https://ng.oberlo.com/blog/twitter-statistics](https://ng.oberlo.com/blog/twitter-statistics#:~:text=Summary%3A%20Twitter%20Statistics,-Here's%20a%20summary&text=There%20are%20330%20million%20monthly,female%20and%2066%20percent%20male.)

(4) https://www.worldometers.info/world-population