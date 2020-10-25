---
title: "Building More Resilient Backend Infrastructure"
date: 2020-10-25T06:23:44+01:00
draft: true
images: ["images/building-more-resilient-backend-infrastructure/building-more-resilient-backend-infrastructure.png"]
tags: [Backend,Infrastructure,Architecture]
---

When building for the web, the backend infrastructure is usually the part that needs the most security and resilience. Over the years, the web has grown very large. There are billions of people surfing the web today. To put it in perspective.
// write number of users facebook, IG and other large companies have compared to the total population of the world.

So when building software for the web, you need to put this in consideration. Compared to the late 90's and early 2000's, whatever we are building today has the potential of being used by billions of people. Millions of people could be using our software simultaneously. To ensure we are building what can serve this high population, a lot of things have to be considered when building the infrastructure of the backend. No doubt, the frontend and other parts of a software matter just as much but in this article, my focus is on backend.

I have spent more than 2 years writing backend for softwares that serve tens of thousands of users, I have experienced a lot challenges and I can only imagine the challenges I'd face when the users of those softwares grow into hundreds of thousands, millions and billions. One thing I have realized is that in software, experience teaches you a lot of things you would not learn from trainings or tutorials. I thought to compile challenges that you can face in building for a growing population of users.

I got this idea while talking with one of my friends, Nicholas, an engineer at Paystack. At that time, I was confronting a challenge I encountered with an implementation I wrote for a particular part of the codebase I manage at my company. I spoke with him and he was able to profer a viable solution.



talk about steps to take when you have issues, like - searching google, then talking to someone, etc



ideas for resilience

- failover for cron jobs
- pagination
- queues
- ability to rollback deployment
- writing unit tests
- writing functional tests
- monitoring
- occasional checks
- securing all loop holes
- constant feedback loops
- writing scripts to automate checks since data grows large quickly
- racing conditions
- db transactions
- fallback or failover for third party api integration
  - exponential back off
- what ever can go wrong will go wrong.
- eliminating single points of failures
- write more utility functions & middlewares to reduce duplication
- commenting
- use git flow or similar branching models for people who use git - make a joke for those who don't use
- Hiding secrets
- Securing db access and other types of access
- Do security checks - consult a hacker
- fire walls


Talk about reading articles from large companies that have solved problems you are facing - don't take everything they say as truth of course.


Talk to Nicholas and ask him to contribute to this according to his experience
Talk to Tochukwu and ask him to contribute to this according to his experience
