---
title: "On the Importance of Tooling"
date: 2019-11-04T18:30:20+11:00
author: "Jordan Simonovski"
draft: true
description: "Why is writing internal tooling for developers so important? How do businesses benefit from the upfront investment in robust tooling?"
tags: ["infra","tooling","developers","developer-experience","devops"]
cover: "/img/tool-time.png"
---

After spending some time focusing on building great tooling for development teams, I've come to see that there is a lot of value in building great developer tooling. 

I'm probably going to just vomit a bunch of stuff that's been on my mind in regards to building out developer tooling,but the most important thing for me is how much better this makes _everyone_ at their jobs.

## Focusing on a Great Developer Experience (DX)

One thing I've noticed that a lot of companies tend to miss is a good investment in Developer Experience or DX. We've all worked in an environment where the ops team has handed down some kind of crazy arcane bash script that does a bunch of crazy shit that we'll never understand, or maybe we've worked in an environment where the operations teams thought DevOps meant giving developrs an Ansible script they wrote and hoping for the best.

One of the biggest issues with writing deployment tooling for developers isn't just making the thing functional, but also giving developers something that they _want_ to use. A lot of the time, you'll get asked questions like "Why would I use your tooling, when X does the same thing?", and I'd argue that in this case you should probably be asking yourself _why_ you're building out tooling. Is this merely to scratch the coding itch that you have because you've been doing nothing but writing YAML for the last two months?

Questions you should be asking yourself are things like:
- What am I doing for developers by writing this tooling?
- Can I give them an easy-to-understand way of interfacing with the infrastructure we've been building?
- How am I vendoring my tooling? Is it easy for developers to download and use? How do they pick up the updates we're making to tooling?

By focusing on building a great DX into your tooling, you're also fostering a better buy-in into the ecosystem that you're building. As you're building more and more complexity into your infrastructure, you want your developers to be able to consume your changes for free. 

## Not Everyone Needs to Care About Implementation Detail

So, you've set up your fancy new Kubernetes Cluster using Helm, and you've told all of your developers that in order to use your fancy new infra that they'll need to use Helm to package their deployments. My question to you is why should they even care?
Arguably to a certain extent people need to understand the basics of what they're doing, but do they _really_ need to know about all of the bits and pieces of your infrastructure in order to get a basic _Hello World_ app going?

We need to be careful when abstracting a lot of the implementation detail away from developers that we're not also removing some critical functionality that they may find useful. In this case, we need to look to set up safe defaults for our applications.
We may know that a lot of our applications in a cluster may run on some pretty small memory requirements, so we can set safe defaults on those, but also give developers the functionality to override them if they need to.

A great example of abstracting away implementation detail is the Auth implementation that we set up using Envoy. We decided that we would love to use Envoy for handling our Authn/Authz concerns, but also didn't want developers to have to understand the intricacies of an Envoy configuration file. 

- Safe defaults
- Under the hood changes
- Add new functionality

## How Can We Make It Easier for Dev Teams to Own Their Apps?
- How do we give them monitoring for free?

## Value for Infrastructure Teams
- Migration Efforts become a _redeploy_ for dev teams


So, when it comes to building out tooling I'd say that you don't need to ask for permission to make people's lives easier. A lot of the time you'll get answers like "This isn't a direct value add to the business" or "This isn't part of your OKRs", but a good upfront investment in tooling will not only repay your team in spades, but also every other team that is releasing their apps to production on a daily basis.