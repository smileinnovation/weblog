Thanks for taking time to create content for our tech-blog 🤩

Here's a simple checklist to be sure everything is ready to squash/merge your contribution into our repo 👍:

- [ ] File name is `YYYY-MM-DD-title-with-dashes-in-place-of-spaces.md` or `YYYY-MM-DD-title-with-dashes-in-place-of-spaces.markdown`
- YAML section contain: 
  - [ ] `title`
  - [ ] `date`
  - [ ] `layout: post`
  - [ ] `category` (only one)
  - [ ] `author`, which is your pentagram (ex: thmil for Thibault Milan)
  - [ ] `tags` as an `[array,of, tags]` or in a list format
  - [ ] `image` is a relative URL pointing to `assets/images/posts/YYYY/MM/DD/XX.ext` where XX is starting at 00 and iterating over if needed.
  - [ ] `subtitle` (optional) is a multiline text which will be used as intro on pages which are listing your post
- [ ] Embeds are made with: 
  - `{% twitter "TWEET_URL" %}` for tweets
  - `{% youtube "URL" %}` for youtube video
  - `{% gist GIST_ID %}` for gist
  - `{% google_map %}` to insert a map (once per post). 🚨 It require extra YAML content to work. Checkout CONTRIBUTING.md 
- [ ] Your pictures are located in `assets/images/posts/YYYY/MM/DD/XX.ext` where XX is starting at 00 and iterating over if needed.

If you're co-redacting this post, please use just one author in the YAML section but include a paragraph at the end with all the authors names.

If you want to sign this PR with all the authors, checkout out some [informations about co-authoring (fr)](https://docs.github.com/fr/pull-requests/committing-changes-to-your-project/creating-and-editing-commits/creating-a-commit-with-multiple-authors) here. 
