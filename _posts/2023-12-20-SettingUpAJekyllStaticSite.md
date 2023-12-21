---
layout: post
title: Setting up a Jekyll static site, hosted on GitHub
slug: setting-jekyll-static-site-hosted-github
date: 2023-12-20T14:02:45.000Z
tags:
  - Jekyll
categories:
  - Static Site
---

# Setting Up My Blog Site with Jekyll and Chirpy Theme

![Image about setting up Jekyll static site hosted on GitHub](/assets/img/posts/Jekyll-Site-Setup.png)

## Introduction

In this post, I'd like to share the history of my previous profile site and the reasons behind migrating to a new one based on Jekyll with the Chirpy theme. Additionally, I'll provide a step-by-step guide on how to set up your own site with the same configuration.

### My Old Site

Originally, I had a custom PHP application running with a MySQL database, powered by jQuery, and styled with Bootstrap CSS. While it was an interesting project, it required constant attention due to changes in PHP versions, database updates, and frontend library changes. Maintaining it became a challenge, and I decided it was time for a change.

## Discovering Jekyll and Chirpy

### Jekyll's Appeal

I began exploring Jekyll as an alternative for my blog site. To get started, I followed the comprehensive documentation available at [Jekyll's official website](https://jekyllrb.com/docs/). This provided me with a solid foundation to work with Jekyll locally.

```bash
# Install Jekyll locally (assuming you have Ruby installed)
gem install bundler jekyll

# Create a new Jekyll site
jekyll new my-blog-site

# Navigate to your new site's directory
cd my-blog-site

# Serve the site locally for testing
bundle exec jekyll serve
```

### The Chirpy Theme

During my exploration, I stumbled upon the Chirpy theme for Jekyll. Its clean and modern design caught my attention, and I decided to adopt it for my blog.

I highly recommend reading the Chirpy documentation at Chirpy Theme Documentation to get a better understanding of its features and customization options.

[Chirpy Documentation](https://chirpy.cotes.page/)

To get started, following the Chirpy guide, I decided to use what they call the Chirpy starter, this essentially clones the current [Chirp Starter repo](https://github.com/cotes2020/chirpy-starter) into a well named repository on your own Github profile. This comes complete with the Github actions configured to publish your pages upon commit to your main or master branch.

### Your first post

This is where reading the documentation is important as the Chirpy pages explains how to create a new post, naming it correctly and setting metadata in the FrontMatter header so that the Jekyll theme can render it correctly.

The [Writing a New Post](https://chirpy.cotes.page/posts/write-a-new-post/) page covers the main gist of this however what I didn't appreciate when I created my first post was the naming of the markdown file had to have the format `YYYY-MM-DD-TITLE.md` And if I wanted to add tags or categories to my post I was able to set them up in the FrontMatter metadata.

### Setting Up VSCode CMS

To streamline my content creation process, I integrated VSCode with FrontMatter, allowing me to manage and edit my content directly from VSCode.

Here's how to set up FrontMatter in VSCode:

1. Install the FrontMatter VSCode extension.
2. Open your Jekyll project in VSCode.
3. Create or edit a Markdown file for your blog post.
4. Add FrontMatter at the beginning of your Markdown file, specifying metadata such as title, date, and categories.
Example FrontMatter:

```markdown
---
title: "My First Blog Post"
date: 2023-01-10
categories:
  - Technology
  - Jekyll
---

# Your post content goes here.

```

With this setup, you'll have a convenient content management system within VSCode.

### Conclusion

In this blog post, I've shared the journey from my old PHP-based blog site to a new Jekyll-based one using the Chirpy theme. I hope this step-by-step guide helps you in creating your own blog with the same setup. Happy blogging!