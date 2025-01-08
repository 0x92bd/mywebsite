---
date: '2025-01-06T23:47:40+06:00'
draft: false
title: 'Building My First Website with Hugo'
author: 'rakib'
image: ''
toc: true
tags:
    - Hugo
    - Cloudflare_pages
    - Github
    
---


In this article, I’ll share my experience creating a personal portfolio and blog website using [Hugo](https://gohugo.io/), a powerful static site generator. Unlike traditional content management systems (CMS) like WordPress, Hugo generates precompiled webpages, making your site incredibly fast, secure, and easy to maintain.

## Why Hugo?
Hugo allows you to write content in Markdown, making blog writing simple and efficient. Its flexibility and speed make it an excellent choice for static websites.

---

## Installing Hugo
To install Hugo on an Arch-based Linux system:
```bash
sudo pacman -S hugo
```
Ensure that you have Hugo v0.128.0 or later:
```bash
hugo version
```

---

## Creating Your Hugo Website
Follow these steps to create a Hugo site with the [Hugo Profile](https://themes.gohugo.io/themes/hugo-profile/) theme:

- Create a new site:
```bash
hugo new site mywebsite --format="yaml"
cd mywebsite
git init
git submodule add https://github.com/gurusabarish/hugo-profile.git themes/hugo-profile
cp themes/hugo-profile/exampleSite/config.yaml .
hugo server
```
- Edit the `config.yaml` file to customize your site. You can safely delete the default `hugo.yaml` file.

### Best Practices
- **Avoid modifying theme files directly**: Instead, copy files to the `layouts` directory, maintaining the same structure as in the theme directory.
  
Example: To modify `about.html`, place it in `layouts/partials/sections/about.html`.

---

## Hosting on GitHub
To host your Hugo site using GitHub:

1. **Create a GitHub Repository**:
- Log in to GitHub and create a new public repository.
- Skip adding a README file for now.

2. **Generate an API Token**:
- Go to **Settings > Developer settings > Personal access tokens (classic)**.
- Create a new API token for authentication.

3. **Push Your Site to GitHub**:
Run the following commands in your Hugo site directory:
```bash
git remote add origin https://github.com/<your-gh-username>/<repository-name>
git config --global user.name "<your name>"
git config --global user.email "<your email>"
git remote set-url origin https://<github-username>:<apikey>@github.com/<your-gh-username>/<repository-name>.git
git branch -M main
git push -u origin main
```

---

## Deploying with Cloudflare Pages
Deploying your site to Cloudflare Pages is straightforward:

1. Log in to the [Cloudflare Dashboard](https://dash.cloudflare.com/).
2. Navigate to **Workers & Pages > Create application > Pages > Connect to Git**.
3. Select your GitHub repository and configure the build settings:

| Configuration Option | Value            |
|----------------------|------------------|
| Production branch    | `main`          |
| Build command        | `hugo`          |
| Build directory      | `public`        |

### Specify a Hugo Version
To use a specific version of Hugo in Cloudflare Pages:

1. Go to your Pages project settings.
2. Under **Environment variables**, create a variable `HUGO_VERSION` and set its value to your desired version (e.g., `0.140.2`).

---

## Conclusion
Using Hugo with GitHub and Cloudflare Pages is a simple yet powerful way to build and deploy a fast, secure, and scalable website. By following the steps above, you’ll have a professional portfolio or blog ready to share with the world.

Happy building!
