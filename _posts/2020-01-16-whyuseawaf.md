---
layout: post
title: Why do you have a WAF?
tags:
 - security
---

> Not everything that counts can be counted, and not everything that can be counted counts. 
>
> William Bruce Cameron

## Start by asking the right questions

After a series of recent conversations with friends who primarily work as a security consultants, I was left wondering whether classic WAFs still had a place in the current landscape. As a concept, a web application firewall is advertised as a component in your infrastructure that creates a “shield” against your application and the bad actors on the other side. 

<img src="/assets/whyuseawaf1.png" alt="whyuseawaf1" width="450"/>

This isn’t true. A WAF isn’t a silver bullet. In a lot of cases, for most organisations and development teams, it is an HTTP proxy deployed with regex matches against URLs that triggers many false positives until you disable the rules to make it redundant. 

However, if you’ve worked with a platform team or in close proximity to a security operations centre, you’ll find organisations often get bot traffic and random bursts of traffic from hard to pinpoint sources. For most security teams, a partially trained WAF with an intrusion detection system is sufficient. 

<img src="/assets/whyuseawaf2.png" alt="whyuseawaf2" width="1000"/>

But what happens when you get a large amount of traffic from bots trained to do more than just hit your endpoints. What if they’re not only harder to detect, but what if the standard regex matching rules just don’t work against them. 

Here’s a breakdown of the four generations of bots looks like. 


* Generation One bots: cURL-like patterns that normally originate from a single origin
* Generation Two bots: Headless tools that use cookies.
* Generation Three bots: These bots can simulate mouse and finger movement and are able to cycle through thousands of IPs.
* Generation Four bots: These are intelligent bots that can get around entropy calculators in some cases by randomising mouse and finger movement to behave more like humans. These bots are designed to mimic humans, and are used for API abuse, card cracking and credential stuffing. 

<img src="/assets/whyuseawaf3.png" alt="whyuseawaf3" width="200"/>

As you can see, a standard WAF is almost useless against these better equipped that have been designed to counter a lot of the out-of-the-box solutions available on the market. 

## So what should you do?

Platform and application security needs to be layered. WAFs, when trained properly, are still a valuable component in your organisation’s security posture.

Start by setting up short and long term goals for your platform. Make sure your developers are onboard and understand what you’re trying to achieve. You can start with a small list and build on this based on your team’s velocity and bandwidth. 

1. Put together a small network diagram for your application. It doesn’t need to be professional, it doesn’t need to be pretty, it just needs to give you and your team a rough idea of which components to consider. 
2. Get visibility to your traffic. If you don’t have this already, you’re in trouble and you need to go back to the drawing board. I’d suggest the elastic stack, but there are cloud native alternatives that might be just as easy to set up. 
3. Depending on the cloud your service lives in, put as much of your infrastructure in an “infrastructure as code” repository as possible. Not only does this make it easy to roll back and try again, it also lets you keep track of your infrastructure. 
4. Choose a WAF that you can train in isolation. This means a preproduction environment with all of the rules you’ll need. This also means setting up a testbed with a variety of legitimate and malicious user journeys that you can run against your preproduction environment. 

<img src="/assets/whyuseawaf4.png" alt="whyuseawaf4" width="400"/>

## Which WAF to waffle with?

Not all WAFs are created equal. The classic WAF should provide you with the following features

1. A mechanism to version control your rulesets
2. A way to update your rules when you need to
3. A simple rate limiting mechanism
4. Enough logging data for you to know when your rules are invoked and when they haven’t

Following this pattern, I chose the AWS WAFv2 for testing purposes. This, along with the ability to add other managed rulesets from third party vendors and a mechanism to test user journeys using automated Jmeter allowed for an OK testbed. Once deployed in preproduction, you can spend sometime figuring out which rulesets work and which don’t. I would suggest automating as much of this process as possible. Manual WAF training is boring, and should be avoided where possible. 

The new AWS WAF can be paired with a cloudfront distribution to allow you to remove some of the latency and add sufficient routing logic using lambda@edge. 

<img src="/assets/whyuseawaf5.png" alt="whyuseawaf5" width="200"/>


## Long term goals
Achieving some semblance of control against the elements on the world wide web is hard. 

But once you’ve got the basics right, you need to start trying to get ahead of the game.  

Keep an eye out for the next post where we’ll talk about some of the longer term goals. 
