# christoph-otte.github.io

This repository is used for deployment of my personal website [christoph-otte.github.io](https://christoph-otte.github.io)

The website is built with [Jekyll](https://jekyllrb.com/), hosted on [GitHub Pages](https://pages.github.com/).

## Local Development

### Prerequisites

Install Ruby and Bundler:

```bash
sudo apt install ruby ruby-bundler ruby-dev build-essential
```

### Setup

Install all required gems locally into the project (only needed once, or after changes to `Gemfile`):

```bash
bundle config set --local path 'vendor/bundle'
bundle install
```

### Run

Start a local test server:

```bash
bundle exec jekyll serve
```

Visit

```bash
http://localhost:4000
```

Changes will be recognised automatically. The page will be built immediately.

## Add New Posts

Create a new markdown file and name it according the following convention:

```
YYYY-MM-DD-title-of-the-post.md
```

Every Markdown file starts with the following header:

```markdown
---
layout: post
title: This is a New Post
tags: [tagname]
math: true   # optional, enables KaTeX
---
```