# Build notes

## Local setup

- Clone code from this repo (or fork it)
- Get theme `git submodule add https://github.com/theNewDynamic/gohugo-theme-ananke.git themes/ananke`
- Serve hugo site locally `hugo serve -D`
- Edit code and see it live at http://localhost:1313

## Create a new post

- Create a post in content/posts
  - Filename format: YYYY-MM-DD-short-name.md
- Fill in top section
- Write post in markdown format
- Save it, git commit, push,
  - Git commit format (Posts only): YYYY-MM-DD Add,Edit,Remove short-name

```
---
author: <Your handle>
categories:
- Uncategorized
ct_period_last_updated:
- default
date: "YYYY-MM-DDTHH:MM:SSZ"
id: 952
title: Hello Hugo, Goodbye WordPress
url: /to-hugo-from-wordpress/
---
```

# To Do

- [ ] Edit all the old content that broke from WP export
- [ ] Learn and correctly use categories and tags
- [ ] Personalize the theme
- [ ] Sunset WP instance and switch domain to Hugo site
- [ ] Google Analytics

# Historical

## Initial setup notes

Followed Hugo [quick start](https://gohugo.io/getting-started/quick-start/)

1. Install git
2. Install hugo
3. Create site
4. Export WordPress content to jekyll zip
5. Import Jekyll zip to Hugo

Set up free hosting with netlify.com
Followed gohugo.io [hosting on Netlify](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/)
