# Build notes

## Local setup

- Install git and go
- Clone code from this repo (or fork it)
- Get theme `git submodule add https://github.com/Greenjam94/theme-hackerunderdev-gohugo.git themes/hackerunderdev`
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

# Initial setup notes

Followed Hugo [quick start](https://gohugo.io/getting-started/quick-start/)

1. Install git
2. Install go
3. Install hugo
4. Create site
5. Export WordPress content to jekyll zip
6. Import Jekyll zip to Hugo

Set up free hosting with netlify.com

# References

Followed gohugo.io [hosting on Netlify](https://gohugo.io/hosting-and-deployment/hosting-on-netlify/)
