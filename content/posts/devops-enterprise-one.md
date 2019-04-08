---
title: "DevOps & Enterprise — Introducing Cultural Change to Big Organisations"
date: 2017-03-12T11:50:20+11:00
author: "Jordan Simonovski"
description: "How introducing DevOps and SRE within the constraints of your organisation can help you win battles in your fight for better IT Operations."
tags: ["devops","enterprise","culture"]
cover: "https://cdn-images-1.medium.com/max/1600/1*XssIvsJiNPGxxeorIzpYPw.gif"
---

_(Apologies for my overuse of gifs. I like to break up my text with a gif every now and then)._

Also keep in mind that this covers the history and definition of DevOps, with [Part Deux](/posts/devops-enterprise-two/) covering the really meaty stuff about change, implementation, and results.

### In the Beginning there was Systems Administration

So, for those of us currently in Enterprise Organisations _The Cloud_ sounds like this very scary/exciting concept. The Sysadmins at your company which have usually been in charge of your environments, deployments, and operations, look at _The Cloud_ similarly to the way that the Ape-Men looked at _The Monolith_ in _2001: A Space Odyssey._

{{< image src="https://cdn-images-1.medium.com/max/1600/1*XssIvsJiNPGxxeorIzpYPw.gif" alt="Monolith" position="center" style="border-radius: 8px;" >}}

Operations teams in traditional organisations are tasked with safeguarding the running systems against the risk of change by introducing launch and change gates (which we commonly know as change approvals). 
As outages occur, certain checks and steps are added into your change process, adding additional effort to a change which then drives a certain behaviour in development teams. _Since changes are so hard to orchestrate and get approved, we will introduce more change to our change and increase the risk of the change breaking._ Does this problem sound familiar? It’s a fairly common problem in the enterprise world. 
What you end up with here is changes becoming so lengthy, tedious, and costly that they effectively become an event. Your team needs to raise a change which then needs to be approved by certain levels of leadership, and then reviewed by a _Change Approvals Board_, which then hands you your next step of raising a request with the Ops team responsible for deploying your change to production.

While this may sounds like a relatively sane process, it doesn’t exactly scale as the amount of changes increases. The same ops team which was doing this before is now trying to prioritise requests from twenty different teams, each with their own mechanisms of deployment. The Ops Team now has to rush to get these releases out the door to meet their SLAs, so the development teams can meet their SLAs, so product owners can meet their objectives. 
The Ops team eventually becomes sloppy, as does anyone doing manual, repetitive tasks. Things get missed, new people join the Ops team and things don’t get properly handed over, or the team simply can’t cope with the amount of changes being prioritised. Things then break.
Once they break, the entire production line comes to a halt. Your development team can’t provide value to customers, as well as several other teams. The Ops team focuses solely on resolving that problem, and other work piles up. Teams become impatient, and work gets escalated only to get shot down by the Ops Lead saying that they need to deal with a _Severity One_ issue.

{{< image src="https://cdn-images-1.medium.com/max/1600/1*NZaNH0SFe67HtTxKrPXkAg.png" alt="this is fine" position="center" style="border-radius: 8px;" >}}

Your team’s stakeholders have now grown impatient. They’re now wasting their entire day waiting for two weeks worth of changes to go out to production which they then need to test, only to find bugs or typos which then need to be fixed by the development team trying to request another change to the already inundated Ops Team. Stakeholders eventually become frustrated and lose confidence in their development teams, and less funding is pushed to the teams which are already underfunded. People become frustrated with their ability to change anything, and eventually leave the company.

What also ends up happening to your Operations team is that as systems grow in complexity and traffic volume, this generates a corresponding increase in events and updates, meaning that The Operations team needs to grow to absorb the additional work.

Does this sound a bit hectic and unstable? **It Is.** In [The DevOps Handbook](https://www.amazon.com/DevOps-Handbook-World-Class-Reliability-Organizations/dp/1942788002), this gets described as a downward spiral, which to me just looks like an upside-down poo emoji. It’s also because the emoji is fairly indicative of the situation you find yourself in.

What happens with this is that:

*   Everyone starts to become busier as work piles up
*   Work starts to take a bit more time to be completed
*   Communications between teams become slower
*   Work queues become a little longer
*   Work becomes more tightly coupled
*   Smaller changes cause bigger failures
*   We become more fearful and less tolerant of making changes
*   We might as well put our nudes in a calendar, because most of our work is now firefighting.

#### The Human and Organisational Costs

In this downwards spiral, people often feel stuck in a system which preordains failure and leaves them powerless to change the outcome. This powerlessness is often associated with burnout, with associated feelings of fatigue, cynicism, hopelessness and despair. This creates a culture where people are too scared to do the right thing because of their fear of punishment, failure, or jeopardising their livelihood.
I guess, this happens to people who don’t want to leave. What you do lose is a lot of your technical talent who are there because they want to do the right thing and be great at their jobs.

{{< image src="https://cdn-images-1.medium.com/max/1600/1*KLrJj9oMpdMLBGAxPC7Giw.gif" alt="dead inside" position="center" style="border-radius: 8px;" >}}

The Organisational costs that you introduce broadly fall into two categories. Direct and indirect costs:

*   **Direct Costs** — are neither subtle nor ambiguous. Running a service with a team that relies on manual intervention for both change management and event handling becomes expensive as the service and/or traffic to the service grows.
*   **Indirect Costs** — Can be subtle, but are often more expensive to the organisation than direct costs. These costs arise from the fact that the Dev and Ops Teams are quite different in background, skill set, and incentives. The split between Dev and Ops can easily become one of not just incentives, but also comms, goals, and eventually trust and respect.

> The opportunity cost of the value that we could all be creating is staggering. We believe that we are missing out on around **$2.6 trillion** of value creation a year. — Gene Kim, Jez Humble, Patrick Debois & John Willis

So, at the and of this whole ordeal, we find our organisation in the following situation:

{{< image src="https://cdn-images-1.medium.com/max/1600/1*EsFESfuo2am9PCMpt0cnlQ.gif" alt="slow down" position="center" style="border-radius: 8px;" >}}

* * *

### Introducing DevOps

{{< image src="https://cdn-images-1.medium.com/max/1600/1*PkJ2JPaSRMP_3aEtrTTUTA.gif" alt="princess bride" position="center" style="border-radius: 8px;" >}}
No, knowledge of Configuration Management tools does not make me “DevOps”

For those of you out there who have been in the industry enough to be cynical of any new buzzword being thrown around like _Agile_ or _Lean_, don’t fret. DevOps is just a label that’s been applied to the collaboration between development and operations teams. Google has been doing this for over a decade, and has progressed far beyond it calling it _Site Reliability Engineering_ (which I admit, is a different discipline altogether).

While many LinkedIn recruiters would have you think otherwise, DevOps isn’t knowledge of Ansible, or AWS, or Bash Scripting.

{{< image src="https://i.imgur.com/sx1CWq5.png" alt="devops" position="center" style="border-radius: 8px;" >}}

Not a method, or framework, or body of knowledge, or even **_shudders_**a vendor platform.

DevOps is a philosophy of unifying development and Operations at the culture, practice, and tools levels to achieve accelerated and more frequent deployment of changes to production.

To avoid the traditional operational fate of having to scale linearly to deal with service size and total traffic of a system, teams need to code and focus on robust and reliable automation or otherwise they will drown in operational tasks that could easily be automated.

In [_listicle_](https://en.wikipedia.org/wiki/Listicle) form, DevOps can involve a combination of a wide range of ideas:

*   Greater collaboration between Dev and Ops.
*   **Accelerated feedback.** Teams need to know about potential issues as soon as possible.
*   **Data Driven.** Measure and monitor everything you can to improve objective feedback. This is inclusive of your day-to-day work and not just your applications.
*   **Continuous improvement.** Iterate on solutions, learn from mistakes, and fail fast.
*   **Always be prepared to fail.** Working in IT, we should be aware that no amount of change approvals or hand-offs will remove the fact that technology is complex and that things can fail at any moment.
*   **Automation as key** to improving deployment speed and frequency without sacrificing quality.
*   **Infrastructure as code.** Ideal scenario is to have a single package to contain the code for the application as well as the environment to run it in.

### So, what did we want to do?

What you’ll hear a lot of organisations say is _“Well, we’re not Netflix”_, which is my opinion is a bit of a cop-out. This doesn’t mean that you shouldn’t focus on building resilient systems, and it doesn’t mean that you shouldn’t focus on operating better.
We had the luxury of implementing our new philosophy to a greenfield project, so we wanted to be _as Netflix_ as we could. We wanted to ensure that we thought about the following while we designed our pipelines, infrastructure, and deployment patterns:

*   **Resilience first.** We did not want to allow any of our infrastructure into a production environment without first destroying it and ensuring that it could bring itself back up without manual intervention.
*   **Destroy. Rebuild. Repeat.** Every code deployment would replace our entire stack. We wanted our team to become comfortable with the fact that we are constantly destroying our infrastructure.
*   **Doing it right is better than doing it quickly.** While hot deploys deploy to an existing server a quicker, we instead opted to rebuild and incur the extra feedback time.
*   **Infrastructure as code.** Maintained by the development team. Nothing was to be manually provisioned. We wanted our team to understand how everything fit together.
*   **Deployments should be a non-event.** I know, I know. We’ve had countless consultants come in and preach it to us. This time was going to be for real.

While none of this may seem particularly revolutionary, many enterprise organisations have ended up using cloud services the same way they would have used their data centres. 
This means that servers are independently provisioned, and from what I’ve heard about some organisations, this is done via a request to an operations team.
Configuration management tools like Ansible, Chef, or Puppet are then used to install middleware like Java, Apache, etc. The same config management tools are then used to deploy code to those servers.

{{< image src="https://cdn-images-1.medium.com/max/1600/1*xNzClkurRjqkkK9fzKuW-Q.gif" alt="wrong wrong wrong" position="center" style="border-radius: 8px;" >}}
That’s just like, my opinion.

I’m not sure if you can see something incredibly wrong with this, but let’s just see what we can find from that short blurb:

1.  This method of deployment encourages the use of static infrastructure. If developers need to request a server from another team, they are already shoe-boxing themselves into a static infrastructure scenario.
2.  Static infrastructure also means static monitoring. You’ll likely see monitoring being set up by server (i.e. CPU monitoring per IP Address).
3.  Oversized instances. Because the infrastructure is static and hard to ramp up during peak load times, the servers end up being provisioned with more juice than they’d usually need. Just in case. You also pay a lot more for oversized instances.
4.  Configuration management tools such as Ansible depend on the SSH protocol to log into a server and copy files, set up user policies, run the application, etc. Can you see what might happen here? The usage of static SSH keys. Unless your organisation has a good key rotation tool, key management starts to become a bit of a chore which consciously adds a potential vulnerability to your system.
5.  Deploying to a particular server via configuration management tools means manual maintenance of host files, knowing our servers by name (your devs should never know their servers in prod by their IP address), and manual disaster recovery.
6.  Creating the same problem we had in our data-centres. This static infrastructure design means that we still have a dependency on our operations teams to patch our servers. You’re already giving another team non-value-adding work.
7.  Operations becomes a manual and conscious task that our development teams need to make time for in their day-to-day work. This again becomes non-value-adding work.

So now we’ve gone through what is wrong with the current way that large organisations operate, what DevOps can bring to the table, and what we were planning to do.

In the second, and final part of this semi-informative, semi-rant series, I wanted to take you through what we did, what our constraints were, and what we achieved overall.

**Click for** [**Part Deux**](/posts/devops-enterprise-two/)
