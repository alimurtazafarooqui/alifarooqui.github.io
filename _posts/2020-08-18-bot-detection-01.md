---
title: Bot Detection - Lesson 01
tags:
 - security
---

## The basics

On a recent project, I’ve been working on bot protection capabilities. This has allowed me to take a deep dive into the world of headless browsers, bot creation and evasion. It’s a murky world where the good guys are building website scrapers and automation utilities to feed pipelines for legitmate purposes, and the less said about the bad guys, the better. 

Disregarding the morality behind bot creation, it’s a surprisingly nuanced cat and mouse game where various security service providers and application security teams will constantly work on upgrading their bot detection capabilities, while the bot creators spend just as much time add entropy to their solutions to get around these measures.

## Common tools

There are a number of headless browser automation utilities and wrappers, but the most popular ones by far are selenium and puppeteer.

 
*Puppeteer*
Puppeteer is a Node library which provides a high-level API to control  headless and nonheadless chrome. This tool is maintained by Google themselves. You can create a single page crawler, take screenshots and create PDFs and even create automation pipelines for your application with this tool. 

*Selenium*
Selenium offers a similar set of features but in multiple languages including python. With the wide community support, there are wrappers that allow you to get feature parity to achieve similar levels of functionality along with the ability to drop in your favourite webdriver to use with this framework. 


## Stealth

With many of these headless tools, the idea is for developers to build scalable solutions that can visit the websites of their choice. The lower the computational overhead, the easier and cheaper it is to scale these solutions. 

However, headless browsers have some tell tales. 

* They normally leave the useragents as “Headless_x_y_x” out of the box
* They’ll leave meta data specific to the webdriver in the requests
* Certain browser features will be left disables. This can include things like sensor information or notification options
* They will have no language or region specific information which makes them easier to pick up

These are the simplest of details, and with a script running serverside, you should be able to enforce a simple webdriver check.

However, all of the above are very easy to update or hide. So easy, that the first google entry for me let me copy and paste the contents of a script, add a `driver.get("myurl.com")` and I was able to evade the simple checks. 

Stick around for Lesson 2 for my findings around stealth based webdrivers and entropy checks.

## Further Reading

Due to the nature of the issue and the stakes involved, there is a large amount of research put into stopping nuanced bots from scraping or attacking websites. A number of easily consumable resources are available on the internets which I’ll reference below for further reading 

[Intoli and it's resources](https://intoli.com/)
[Puppeteer from Google](https://developers.google.com/web/tools/puppeteer)
[Selenium with Python](https://selenium-python.readthedocs.io/)
[Browser detection from Antoine Vastel](https://antoinevastel.com/bot%20detection/2019/07/19/detecting-chrome-headless-v3.html)
[Antibot Scripts from sannysoft](https://bot.sannysoft.com/)
[Datadome's bot protection writeup](https://datadome.co/bot-management-protection/bot-detection-how-to-identify-bot-traffic-to-your-website/)

