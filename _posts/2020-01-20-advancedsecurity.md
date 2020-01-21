---
layout: post
title: Building a more robust security focussed deployment pipeline
tags:
 - security
---

I bored myself writing that title. If you’re still here, we’re following on from the previous post about why WAFs still exist, and whether they add any value to your infrastructure. 

> If you know the enemy and know yourself, you need not fear the result of a hundred battles. If you know yourself but not the enemy, for every victory gained you will also suffer a defeat. If you know neither the enemy nor yourself, you will succumb in every battle.”

> Sun Tzu, The Art of War

After recently deploying and testing the new AWS WAFv2, I decided to dig deeper to see if other alternative security layers add any value to applications that have public APIs. 

## Bringing the stakeholders inline
We’ve all worked with large teams where the risks are put on backlogs, and the technical debt pile grows as your teams add prioritise features and push maintenance work further down the list of visible work. 

<img src="/assets/advancedsecurity1.jpg" alt="advancedsecurity1" width="600"/>


It is your responsibility to make sure the stakeholders understand why security and operability matter. The stakeholders need to understand the importance of data, what the risks are and decide what resources are required to keep on top of these risks. 

The cost of an unpatched security vulnerability or an unattended API scraping attack isn’t something that is immediately obvious to stakeholders. This needs to be visualised in a way that makes sense, not only to them but to allow you to prioritise your backlogs. 

## IAST and RASP

IAST stands for Interactive Application Security Testing and IAST tooling allows security vulnerability checks to be run with automated tests during and after your application deployments.

Gartner defines RASP as “a security technology that is built or linked into an application or application runtime environment and is capable of controlling application execution and detecting and preventing real-time attacks”. 

Unlike a WAF, which lives at the edge of your network, RASP tools are designed to run in the application security management layer and uses mechanisms like dynamic binary analysis, JVM replacement and virtualisation for better decision making. 

<img src="/assets/advancedsecurity2.jpg" alt="advancedsecurity2" width="500"/>


In simple terms, it is a security layer that lives closer to the application, which provides it with better visibility to the calls being made to the application and therefore, more information to block malicious calls. 

#### Why this might not work with your team
 Introducing a new runtime requires a number of prerequisites including convincing your development teams to add the runtime to the deployment. 

You’ll also want to consider any latency or additional compute necessary for this runtime change. Depending on your application, you might get pushback on this. 

There is also almost always a case that needs to be made about changing something close to the application runtime instead of adding one of the next generation WAFs. WAF vendors have been working on addresses automated tuning, monitoring and API protection too so making a case for a RASP needs to be balanced with your team’s risk tolerance. 

#### Performance Degradation

Performance degradation concerns are legitimate in some cases. In recent studies using the the Contrast RASP agent with a Node app deployment. Turning on blocking or monitoring mode can add up to 3 times the overhead in response time per request. This is due to the nature of the RASP deployment. 

The smallest application response time increase was 55%. This could potentially be a deal breaker. I would however suggest setting up a test bed and following the procedure highlighted below. 

1. Test your application performance in request time per second with a standard set of user journeys without a WAF or any network hops. 
2. Deploy your application with your current network stack, and measure the this again. 
3. Run the application with your automated “malicious” user journeys and make a note of this too. 
4. Now deploy your RASP and run the same three set of tests again.

<img src="/assets/advancedsecurity3.png" alt="advancedsecurity3" width="600"/>


For a lot of teams, this is should provide enough feedback to go ahead with a more refined proof of concept.

#### Language support

Some languages and platforms do not have mature offerings. This could be an immediate deal breaker. Gartner provide a list of twenty vendors with RASP capabilities and this should be the starting point for your proof of concept. 

<img src="/assets/advancedsecurity4.png" alt="advancedsecurity4" width="700"/>


## Next Generation WAFs
We previously discussed the four generations of bots. These next generation WAFs provide better intelligence when dealing with crafty bots and malicious actors that know what they want from your infrastructure. 

For any application that is getting scraped or experiencing credential stuffing, you want some level of anti-automation defense. 

There are multiple vendors that provide additional bot detection capabilities. Here is the recommended list of vendors that I would start with. 

1. Shape Security
2. Imperva - Distil Networks
3. Signal Sciences

<img src="/assets/advancedsecurity5.png" alt="advancedsecurity5" width="300"/>



## Avoid SOC Fatigue
Your security operations centre team are short on staff, resources and visibility in a lot of cases. The fact that most security “bundles” add to their pain instead of reducing it should be a concern for most application development teams. 

When deploying any solution, the acceptance criteria for completion needs to include a list of achievable goals. 

1. A clear rollout plan which includes additional visibility for all first responders on your team.
2. Additional runbooks for out of hours support. This should be put together with the help of your SOC. This needs to be adjusted based on the skillset of the first responders. 
3. A set of feedback loops that the SOC has the ability to participate in. This includes any logging, tuning, training or performance measurement tweaks.

Securing your platform and addressing your risks might require additional human interaction. This is something you should always keep in mind. 

<img src="/assets/advancedsecurity6.png" alt="advancedsecurity6" width="700"/>



## Agile Security Operations
When addressing the security infrastructure for any team, I start the process by making sure I have a few prerequisites in place. 

1. A mechanism to test the current state of your application. 
2. A testbed or sandboxed environment that is like for like for your current infrastructure, but that can be torn down and built back up. A lot of teams are uncomfortable for their preprod environments to be use for such tests so make sure you have the ability to create these resources in a separate environment or account. 
3. The scope for your automated tests. 

Here’s what your pipeline might look like. 

<img src="/assets/advancedsecurity7.png" alt="advancedsecurity7" width="1200"/>


## What's next?

Setting up a functional pipeline is hard work. This requires the love, care and attention of a skilled team of developers, platform engineers and security engineers. 

It's easy to build. Everyone wants to build these pipelines. You now need to focus on maintaining it. Focus on the technical debt left behind everytime there is a feature added to your estate. 

Keep an eye out for the next blog post where we'll talk about creating these pipelines and keeping them in shape. 


## References
[Dynamic Binary Instrumentation Technology - Kunping Du, Fei Kang, Hui Shu, Li Dai](https://doi.org/10.2991/citcs.2012.132)
[Insertion, Evasion, and Denial of Service - Ptacek, Newsham](https://www.cs.unc.edu/~fabian/course_papers/PtacekNewsham98.pdf) 
[ITSP Brighttalk Webinar - Sean Martin](https://cdn02.brighttalk.com/core/asset/video/283443/mp4/320/video_1507140442.mp4)
[Investigating the Effectiveness of RASP - Alexander Fry](https://www.sans.org/reading-room/whitepapers/application/runtime-application-self-protection-rasp-investigation-effectiveness-rasp-solution-protecting-vulnerable-target-applications-38950)
[Jumpstarting Your DevSecOps Pipeline with IAST and RASP](https://www.youtube.com/watch?v=9QJZkPISB58)
[Weaving Application Security into the Software Development Life Cycle](https://www.contrastsecurity.com/weaving-application-security-into-the-software-development-life-cycle)
[Continuous Application Security at Scale with IAST and RASP — Transf…](https://www.slideshare.net/planetlevel/continuous-application-security-at-scale-with-iast-and-rasp-transforming-devops-into-devsecops-64283374)
[DevOpsSec - Jim Bird](https://www.oreilly.com/library/view/devopssec/9781491971413/ch04.html)
[Gartner Emerging Security Technology Review 2017](https://www.gartner.com/en/documents/3803473/emerging-technology-analysis-runtime-application-self-pr)
