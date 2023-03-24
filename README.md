## Before your start

Welcome everyone 👋

Welcome to the hitchhiker's guide to the galaxy for contributing to our Tech blog (<https://tech.smile.eu>). If you are here, it’s mostly because you will write some piece of awesome content and contribute to the fame and renown of Smile.

### Tone

Following the brand identity guideline, we must be friendly but professional, accessible to the non-expert and feel knowledgable without being mister Know-It-All.

Your content must be pleasant to read and fluid. You can take your inspiration from the already existing content on our blog.

### Rules

Some rules apply if you want to contribute. They are few but very important.

1. Content must be written in English (British or American, as you like).
We are targeting an international audience and content which is written in French will make us less visible.
2. Always include a wide, plain page, header picture. It doesn’t need to be something relevant. You can use some abstract pictures or metaphors.
You can use [unsplash.com](https://unsplash.com) which offers free to use pictures, don’t forget to quote the author.
3. Always include a short introduction.
Also known as subtitle, it will be displayed as an excerpt of your article on search results and social media sharing.

## How to write content

First, you'll need to join the team on github. You can write to <thibault.milan@smile.eu> and provide your github nick handle (@something).

Then, you'll need to choose how you want to write. You can either do it online on github.com (very very simple interface) or github.dev (Microsoft Code Web Studio) or, of course, you can use your own IDE.

### A. I'll use github.com (very simple interface)

1. Go on [the branch list](https://github.com/smileinnovation/weblog/branches) and create a new branch name `writing/[your-name]`, pressing the green button.
![Create a new branch](https://p190.p3.n0.cdn.getcloudapp.com/items/rRug0W6g/11ad0e49-6d6a-408f-b718-ad56f1d15f8b.gif?v=d1846024ccf4287af50892bef3637ede)
2. On [the code view](https://github.com/smileinnovation/weblog/tree/main), switch on your newly branch
![Switch to your new branch](https://p190.p3.n0.cdn.getcloudapp.com/items/YEueQkR4/4bdd2914-e306-4289-8256-097974aeb2f1.gif?v=792c834a06051a852de88ce1a55b01f1)
3. Navigate to `_posts` and click on `Add file` > `Create a new file`

### B. I'll use my own IDE

1. Clone our repository on your laptop
2. Create a new branch `writing/[your-name]`
3. Start writing in `_posts/`

### Then, for everyone

1. Insert a front-matter header
```yaml
---
layout: post # Always use this
url: # just complete here if the content is from another platform. If so, paste the URL here
title: "Smile’s Innovation Watch #32" # Use " "
subtitle: |- # If you want to use subtitle, multiline, follow this example
    It’s December once more and, if you’ve not already managed the 🎁 “presents” situation, you must know that delivery is a complete mess this…
slug: smiles-innovation-watch-32 # optional, let you customize the slug
description: # optional, will override SEO description tag
tags: # one per line, like this. You can use whatever you want here to describe your content.
 - meta
 - bitcoin
 - paypal
 - autonomous-cars
 - ebcdic
category: techwatch #only one category. Optional
author: thmil #the author slug. Please read Author section of this document or Contributing.md
image: "assets/images/posts/0*dJorJpg38rjFtUJe.jpg" #your image.
---
```

2. Write your content under the front-matter. Using markdown or (x)HTML. [If you don't know markdown, read this](https://www.markdownguide.org/basic-syntax/).
3. Put your pictures into `uploads/posts/[year]/[month]/[date]/`. If you're on github.com, you'll have to navigate to / create thoses folders, then on `Add File` > `Upload`
4. When you're done, create a PR to our `main` branch and follow the checklist. On github.com, you should be prompted to create the PR when you save your main file (but you can wait you uploaded your pictures + create the content).

## Helpers
While you're writing your content, you may felt limited to what you can do. That's why, we included some rich content helpers below:

### Youtube

Directly in the content:
 `{% youtube "https://www.youtube.com/watch?v=ho8-vK0L1_8" %}`

### Google maps

In front-matter:
```yaml
location:
  - latitude: 51.5285582
    longitude: -0.2416807
  - latitude: 52.5285582
    longitude: -2.2416807
  - title: custom marker title
    image: custom marker image
    url: custom marker url
    latitude: 51.5285582
    longitude: -0.2416807
```
Then, in your content `{% google_map %}`

### Gist

In your content `{% gist c08ee0f2726fd0e3909d %}` or `{% gist c08ee0f2726fd0e3909d test.md %}`

### Twitter

In your content `{% twitter https://twitter.com/rubygems/status/518821243320287232 %}`
