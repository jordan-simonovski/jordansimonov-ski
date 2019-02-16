---
title: "There and Back Again"
date: 2018-04-23T12:50:20+11:00
draft: true
---

# There and Back Again

## How my move out of a product team to an infrastructure team, and back again changed my view on DevOps and end-to-end ownership of products.

When I started my career in IT, I was working in an organisation where so many bits and pieces of the Software Delivery Life Cycle were out of my control. Many developers I worked with had just come to terms with the fact that every deployment of our app would take about a day of effort, as well as the involvement of three to four other teams.

To me and to many other developers I worked with, deployments were painful, scary, and in most cases tedious.

![](https://cdn-images-1.medium.com/max/1600/1*TMOPe9-PGh6D36cBBd0DPw.gif)Ops every time a Dev wants to touch infrastructure.

I decided to move into an infrastructure team and learn as much as I possibly could to see how I could help out my fellow developers. Moving into a purely infrastructure related role, I was able to learn an incredible amount about what good infrastructure looked like, and how it could be used to improve the quality of life of an everyday developer.

While my job focussed on building and maintaining systems, I found my passion in deployment tooling, and trying to make deployments as painless as possible. Having read the theory on what good DevOps as a culture looked like, I felt like I was missing something.

I found that a lot of organisations had basically rebranded the Sysadmin role as a _DevOps_ role (which usually implied that it was the same job, just in the cloud). While the focus on replacing infrastructure with something much more modern, I felt as though we weren’t exactly _DevOps._

A lot of teams were structured in their own silos as Infrastructure Teams, SRE Teams, Cloud Engineering Teams, and many other variants of the same thing, which also meant that engineers in Dev and Ops teams still didn’t know what was expected of them.

I kept asking myself how _DevOps_ as a culture can happen in an organisation. Bits and pieces were set in motion, but there still seemed to be a breakdown in communication.

Some approaches have been trying to _give_ developers more ownership of their infrastructure. But as I was soon to learn, they had enough on their plate without having to think about lower level operational concerns of their infrastructure.

I finally got the chance to experiment with DevOps and what that meant to me when I joined a much smaller company as a Full Stack Engineer. In my first couple of months, I gave myself the task of building the infrastructure I thought would be most needed such as docker orchestration, CI/CD, logging, and monitoring.

Getting back into building applications, I suddenly found myself thinking not just about the code I was writing for the products I was developing, but also about how robust our ping checks were, how meaningful our logging was, improvements to our docker images, and architectural improvements of our applications.

That being said, there was never an imposed expectation on the rest of the development team to understand and actively work on fixing these issues. We were all focussed on different parts of the application, and that’s where it clicked for me. Goal driven work for each of us gave us the ability to focus our energy where it mattered.

I wasn’t constantly context switching dealing with trivial problems other teams were facing, and the developers on my team weren’t trying to learn operations while also being effective at their day job.

The introduction of DevOps to your organisation doesn’t suddenly mean that everyone has to become a jack-of-all-trades, but it also doesn’t mean that developers should have an expectation that everything that happens after they push their code is _magic_ either.

In my experience as an infrastructure guy, here’s what skills I think developers should have to build a healthy DevOps culture:

*   **You should know how to build and deploy a production version of your app.** If you use Docker, you should be able to build your own container.This is especially important if you’re willing to invest in developing with docker locally. If you use something else, that’s fine, as long as you know the steps to build a production version of the app, and all of the runtime deployment dependencies.
*   **You should have some kind of understanding of the CI systems in place.** Know how to pipeline your build, and how to add steps to it. You should also know how to debug build issues.

![](https://cdn-images-1.medium.com/max/1600/1*-ssg-aIlk6BC23nuJUxqTA.png)

*   **Help operations when outages occur.** While they may be building and maintaining the supporting infrastructure, breakages often occur when changes happen. The apathetic mentality of _Not my problem_ is unacceptable. Every problem is _our_ problem.
*   **Take collective ownership of cost.** This one is relevant if you’re working with public cloud offerings. Be careful what you run!
*   **Understand how you can build robustness into your applications.** While it may not always be the most interesting type of work, doing things like improving your health check endpoint to include connectivity checking of your dependencies (databases, caches, etc), changing the way you host static files (moving to S3), and many more quality of life improvements make apps easier to debug when things go downhill.
*   **You should be on-call for the applications you write/maintain.** While this may be a bit more contentious one, this remains my opinion. It’s unfair to expect the ops guy to know how to debug Node, Java, PHP, and C# applications. This falls into the bucket of every problem being _our problem._

![](https://cdn-images-1.medium.com/max/1600/1*YdlvdLe-CJgCZD4stt__rw.jpeg)

What I’m trying to say is that a successful DevOps culture is dependent on **empathy**.

_Infrastructure engineers should be able to focus on building out robust, secure, and fault-tolerant platforms._

_Developers should be able to focus on building and maintaining their applications._

Both roles allow us to deliver value to the businesses we work for, and to the customers we love to delight with the awesome products we write.
