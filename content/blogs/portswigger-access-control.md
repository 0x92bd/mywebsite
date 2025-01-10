---
date: '2025-01-10T16:31:21+06:00'
draft: false
title: 'Portswigger Access Control'
author: 'rakib'
tags:
    - portswigger
    - burpsuite
    - webapp_testing
    - access_control
image:
description:
toc: true
---

In this article I will share walkthrough to solve access control vulnerability labs in portswigger academy.
<br></br>

## Lab 1: Unprotected Admin Functionality

### Overview

This lab contains an unprotected admin panel. Our goal is to delete the user `carlos` to solve the lab.

### Solution

1. Access the `robots.txt` file to find the admin panel link:

![robots.txt](/img/blog/20250105223919.png)

2. Navigate to the admin panel URL and log in without credentials.

![adminpanel](/img/blog/20250105224013.png)

3. Delete the user `carlos` to complete the lab.

![complete](/img/blog/20250105224100.png)

---
<br></br>

## Lab 2: Unprotected Admin Functionality with Unpredictable URL

### Overview

This lab's admin panel is at an unpredictable location disclosed in the application. The goal is to delete the user `carlos`.

### Solution

1. Inspect the product page source code to find the admin panel URL:
```
var isAdmin = false;
if (isAdmin) {
   var adminPanelTag = document.createElement('a');
   adminPanelTag.setAttribute('href', '/admin-061w6y');
}
```
![jscode](/img/blog/20250105225009.png)

2. Navigate to the admin panel and delete carlos.

![admin](/img/blog/20250105225046.png)
![solved](/img/blog/20250105225131.png)
<br></br>

## Lab 3: User Role Controlled by Request Parameter

### Overview

This lab uses a forgeable cookie to identify administrators. The objective is to access the admin panel and delete carlos.

### Solution

1. Log in with the provided credentials:
```
Username: wiener
Password: peter
```

![login](/img/blog/20250105215143.png)

2. Inspect the request headers. Note the Admin cookie:
```
Cookie: Admin=false; session=Jiu7Q6cGNKF9NY0sL1GV75R1Un3Zrqk4
```
3. Modify the Admin cookie to true using Burp Suite:
```
Cookie: Admin=true; session=Jiu7Q6cGNKF9NY0sL1GV75R1Un3Zrqk4
```
![cookie](/img/blog/20250105215805.png)

4. Modify the Admin cookie to true in browser and refresh to access admin panel and delete `carlos`.

![admin](/img/blog/20250105220021.png)
![solve](/img/blog/20250105220107.png)
<br></br>


## Lab 4: User ID Controlled by Request Parameter with Unpredictable User IDs

### Overview

This lab requires escalating privileges by identifying carlos's GUID and submitting his API key.

### Solution

1. Log in using the provided credentials:
```
Username: wiener
Password: peter
```
2. Inspect your account page to find your API key.

![api](/img/blog/20250105225717.png)

3. Find carlos's GUID by inspecting his blog posts:

![guid](/img/blog/20250105230236.png)
```
https://0add00be037d4278819b3ed400bb0085.web-security-academy.net/blogs?userId=6edbadd6-1268-4466-83f9-0dbebb148cad
```
4. Use Burp Suite to request carlos's API key:

![api-burp](/img/blog/20250105230454.png)

5. Submit carlos's API key to complete the lab.

![solve](/img/blog/20250105230732.png)
<br></br>

## Lab 5: User ID Controlled by Request Parameter with Password Disclosure

### Overview

This lab involves retrieving the administrator's password by manipulating the id parameter, then deleting carlos.

### Solution

1. Log in using the provided credentials:
```
Username: wiener
Password: peter
```

![login](/img/blog/20250105205039.png)

2. Inspect the request for the account page. Modify the id parameter to `administrator` using Burp Suite and retrieve the administrator's password from the response:
```
GET /account?id=administrator
```
![modify](/img/blog/20250105205308.png)

3. Log in as the administrator:
```
Username: administrator
Password: 61dp6dq38bab1d50921x
```
![admin](/img/blog/20250105205701.png)

4. Access the admin panel and delete `carlos`.

![solve](/img/blog/20250105205747.png)
