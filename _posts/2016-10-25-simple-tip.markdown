---
layout: post
title:  "Simple tip for blogging with jekyll!"
date:   2016-10-25 08:37:00 +0100
categories: blogging
---

After having spent some time working with jekyll (yes I'm actually working on a series of posts) I realized I needed a better solution for running jekyll locally without having to change config files.

I noticed that to run the github pages repository locally with jekyll on windows I had to explicitly set the url parameter to `https://localhost:4000` otherwise any theming wouldn't be applied.

My first approach was to have two lines in the `_config.yml` and comment these appropriately when running either locally or on github, but I noticed that I constantly failed to remember to either include or exclude the changes to this file when pushing to github meaning I had to make a minor change and push again. 

My second approach was to be more explicit when running the `bundle exec jekyll serve` command and pass the url parameter which should work by simply specifiying `url=https://localhost:4000` but that doesn't seem to work, or at least I haven't figured out how to appropriately pass this argument and making jekyll use it.

Thus my third approach which now works fine for me: I've created a very small `_local_config.yml` file which only includes the following line 

```
url: "https://localhost:4000" # the local url during development
```

Then I can run locally by running: `bundle exec jekyll serve --config "_config.yml,_local_config.yml"` since the later configuration parameters will overrun the earlier specified.

Since I'm also using PowerShell as the console, I created a small PowerShell script to run this configuration locally as follows:

```powershell
bundle exec jekyll serve --drafts --config "_config.yml,_local_config.yml"
```

Please note the `--drafts` parameter which allows me to work locally with drafts that aren't public and available yet.

I'm really liking this setup, blogging with Visual Studio Code, running jekyll locally and then hosting on GitHub!