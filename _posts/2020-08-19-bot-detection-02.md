---
title: Bot Detection - Lesson 02
tags:
 - security
 - automation
---

## The Webdriver

This is lesson 02 in this series where I’m dumpin what I’ve learnt about bot detection. 

The webdriver is based on the [W3 standard](https://w3c.github.io/webdriver/) to let you control user agents. 

According to the official documentation 

```
navigator.webdriver
Defines a standard way for co-operating user agents to inform the document that it is controlled by WebDriver, for example so that alternate code paths can be triggered during automation.
```

Chromium and gecko both follow the same standard.

[Chrome WebDriver Status](https://chromium.googlesource.com/chromium/src/+/master/docs/chromedriver_status.md)

[GitHub - mozilla/geckodriver: WebDriver for Firefox](https://github.com/mozilla/geckodriver)

With these standards you can follow the basics with the language of your choice.

1. Session management
2. Context management
3. Navigation
4. Element traversal and search
5. Clicking
6. Documentation with screen capture

## Examples in Python

Geckodriver 

```python
from selenium import webdriver
driver = webdriver.Firefox(executable_path='/path/to/your/geckodriver')
driver.get('http://alifarooqui.net')
```

Chromedriver

```python
from selenium import webdriver
import pickle

from selenium.webdriver.chrome.options import Options
from fake_useragent import UserAgent
from selenium.webdriver.common.desired_capabilities import DesiredCapabilities

ua = UserAgent()
userAgent = ua.random

chrome_options = webdriver.ChromeOptions()
chrome_options.add_argument("--headless")
chrome_options.add_argument("--use-fake-ui-for-media-stream")
chrome_options.add_argument("--disable-gpu")
chrome_options.add_argument("--user-data-dir=/home/ubuntu/")
chrome_options.add_argument('user-agent=' + userAgent)
driver = webdriver.Chrome(options=chrome_options)

driver.get("https://alifarooqui.net")
```