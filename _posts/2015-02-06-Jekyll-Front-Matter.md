---
date: 2015-02-06 20:31:00
title: Jekyll Front Matter
tags: [Jekyll]
---

Okay, I figured out how to sort posts by time; apparently it was a case of [rtfm](http://jekyllrb.com/docs/frontmatter/#predefined-variables-for-posts). Just set the key `date` in your post's front matter to the time you wrote the post.

For example, the front matter for this post is

```
---
date: 2015-02-06 20:31:00
title: Jekyll Front Matter
tags: [Jekyll]
---
```

Another useful tip is to add keys you find yourself repeating into your `_config.yml`, eg. adding `layout: post` in every post's front matter. Again another [rtfm](http://jekyllrb.com/docs/configuration/#front-matter-defaults), my `_config.yml` now includes a section like this

```
# Set default layout for all files in _posts to post
defaults:
  scope:
    path: "_posts"
  values:
    layout: "post"
```