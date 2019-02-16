---
title: "My Sunday Project — Static S3 Hosting"
date: 2018-06-04T12:50:20+11:00
draft: true
---

# My Sunday Project — Static S3 Hosting

Make the good way of serving your front-end the easy way.

Before I start, I might as well address some of the acronyms here for people who may be unfamiliar with AWS Services, or some other internet concepts:

*   **Why S3?** S3 or _Simple Storage Service_ is exactly that. A simple way of storing anything you want on something called a _bucket._ Where S3 excels is the fact that it’s essentially serverless (this doesn’t mean it doesn’t run on servers. Just that as far as you’re concerned there are no servers to manage), meaning that all operational headaches like patching, upgrading, and maintaining uptime are not your responsibility. This way you can focus on the important stuff like writing awesome apps!
*   **What is CloudFront?** CloudFront is a [Content Distribution Network](https://en.wikipedia.org/wiki/Content_delivery_network) or CDN for short. This is particularly useful when running an app, as any outage you may have won’t affect it’s availability to your users (as far as just serving content is concerned).
*   **What is a WAF (**[**Web Application Firewall**](https://en.wikipedia.org/wiki/Web_application_firewall)**)?** This is great for filtering, monitoring, and blocking traffic to your HTTP application. In our case, we’re blocking everything _except_ a whitelisted IP.

![](https://cdn-images-1.medium.com/max/1600/1*YsTv59tGk_vctpF5qgeVOg.jpeg)When you get that site working at the lowlow cost of $0.40/month (check out my dist/index.html for more context)

After having looked through a few examples of using S3 buckets to serve static content, I couldn’t help but wince at the fact that most of these tutorials were getting people to create public buckets. I decided to make it easy to set up a CDN.

While this may be a relatively easy way of hosting your front-end, I couldn’t help but feel that there could be an easier way of fronting your app with a CDN by default. So, I set up a little boilerplate repo and created two separate CloudFormation templates.

While the initial set up of everything takes roughly 20 minutes (you are caching your front-end on all kinds of edge locations around the world), deployments take roughly 5 seconds (depending on your internet connection).

I wanted to ensure that I could also lock down my S3 bucket resources to something like an Office or Home IP address, so I set up a separate template that also creates a Web Application Firewall (WAF) blocking by default, and only allowing traffic from a defined IP range.

While the set up of S3 + CloudFront (CDN) + WAF may sound relatively expensive, I found it would actually be quite cheap to run a site that you wouldn’t expect to get much traffic to. The nice thing about CloudFront is that you don’t necessarily pay per the amount of requests to your distribution, but the amount of data you transfer out. For a simple app, you’d be lucky to transfer anything close to 100GB/month (which costs $4.25USD).

Take a look at the repo and let me know if there’s anything that concerns you. I think I’ve set the README up to be relatively simple to follow.

[https://github.com/jordan-simonovski/static-s3-boilerplate](https://github.com/jordan-simonovski/static-s3-boilerplate)
