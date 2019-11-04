---
title: "On the Importance of Tooling"
date: 2019-11-04T18:30:20+11:00
author: "Jordan Simonovski"
description: "Why is writing internal tooling for developers so important? How do businesses benefit from the upfront investment in robust tooling?"
tags: ["infra","tooling","developers","developer-experience","devops"]
cover: "/img/tool-time.png"
---

After spending some time focusing on building tooling for development teams, I've come to see that there is a lot of value in building tooling that developers prefer to use over alternatives that they could othwerise find to get their jobs done.

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

Let's first think about what a developer needs to start configuring to deploy a simple app with authentication. A few things that immediately come to mind:

- Configuring an envoy configuration. This includes having to configure the provider configuration, and which routes you want to be authenticated to something like Auth0.
- Configuring your helm chart. This includes also setting up egress only on your envoy sidecar and not your API which is sitting behind the sidecar.

Both of the above mentioned steps require developers to have to understand the implementation detail of the infrastructure they're working on. This also leaves room for human error, and various differences in the ways things are configured.

Let's look at a simple example of abstracting this away from the developers into a tooling configuration.

```
routes:
  - path: /ping
    authUsers: ['public']
    container: service
  - path: /service
    authUsers: ['customers']
    container: service
  - path: /admin
    authUsers: ['admins']
    container: service

containers:
  - image: ${DOCKER_REGISTRY}/auth-service:latest
    name: service
    port: 8080
    healthCheck:
      - CMD-SHELL
      - ps -ef | grep python || exit 1
    environment:
      default:
        SERVICE_NAME: SERVICE_SIDECAR
```

This configuration examples exposes only things that developers need to care about when setting up a new application.

- Which routes to authenticate
- Which port to run on
- What the container is
- How we check for a healthy container
- environment variables

This completely abstracts ~800 lines of configuration that developers will otherwise have to battle in order to get an application up and running.

Adding in new functionality like support for an Envoy sidecar is something we can now do completely under the hood without significantly affecting people consuming these tools. This also gives us the added benefit of a standardised approach to authentication which gives us better security in the future, as we can make centralised changes in the form of a tooling update which developers will get for free.

Another example of completely abstracting away the implementation detail is the ability to make sweeping infrastructure changes under the hood, and roll them out in the form of a tooling change. Something we did recently was completely replace a horrible load balancing setup with a solution like Traefik, and all we needed to do was tell developers to redeploy their apps to pick up the new infrastructure changes.

## How Can We Make It Easier for Dev Teams to Own Their Apps?

One concern with writing tooling that abstracts so much from developers is the potential to make everything too much of a black box that makes it hard to debug applications. 
What our tooling also needs to give the developers is an easy way to set up instrumentation of some kind on their applications that allows them to debug things without needing to jump into boxes. If the need to SSH into a box to debug a problem arises, you should be investing more time in instrumenting more of your infrastructure and applications so you can easily find issues on a platform like DataDog or Honeycomb when things _do_ come up.

Helping teams own their applications is not something tooling alone can fix. We can help them put the pieces of the puzzle together to be able to have some kind of instrumentation on their applications, but the rest of it comes down to communication with other teams, usually in the form of a workshop or brownbag. 

I won't cover too much of that in this post, but a TL;DR; of that would be to help them understand: 

- How to diagnose a problem using the tools provided
- How good instrumentation can give us a better indication of what is going wrong instead of logs
- How to set up good monitors to notify us when something _is_ going wrong.


## Value for Infrastructure Teams

There is so much value in investing in great tooling that helps infrastructure teams move faster and safer. If you have a set of standardised tooling that you maintain to help developers deploy their applications, you may want to eventually add more functionality into it that would otherwise become a Herculean effort.

Let's consider something like rolling out a set of standardised tags on our stuff in AWS. I've been in environments where you'll get a stern email from someone in an operations team telling you that you need to have a `TEAM` tag applied to your apps. Or maybe you'll be in a situation where a team wants to roll this out, but due to the sprawling nature of app setups it becomes such a big piece of work that can never get prioritised.

With something like centralised tooling, there is a small communication piece to tell developers that their builds will fail and that they'll need to add a `team` config to their tooling, but the rest of it comes down to updating the tooling to fail builds if they're missing some things that may be required for compliance reasons.

As an infra team, you can also sleep soundly knowing that the apps that have been deployed have followed the guard rails that you've set up with your tooling and that you likely don't have an app with port 22 open to the world.

## Concluding Thoughts

Sure, some infra teams may want to just write YAML all day while complaining that developers keep doing dumb things, but I'd question whether or not they're helping the company at all with that kind of attitude. At the end of the day, operating complex systems is a collaborative exercise, and we need to be helping development teams own their applications and reduce the amount of toil our team is dealing with on a daily basis. It's also worth noting that your job in an infra team isn't to just set up the best Kubernetes Cluster ever, but to also help developers deliver value without the undue stress of working in an environment that is completely foreign.

So, when it comes to building out tooling I'd say that you don't need to ask for permission to make people's lives easier. A lot of the time you'll get answers like "This isn't a direct value add to the business" or "This isn't part of your OKRs", but a good upfront investment in tooling will not only repay your team in spades, but also every other team that is releasing their apps to production on a daily basis.