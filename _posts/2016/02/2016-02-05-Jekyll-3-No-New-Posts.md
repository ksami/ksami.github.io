---
layout: post
date: 2016-02-05 02:09:06
title: Jekyll 3 No New Posts
tags: [jekyll, github-pages]
---

Long story short, I wrote a new post, pushed to Github Pages, no new post appeared, and it's now been 4 hours of me trying to solve this issue.


## The Fix
First, the fix, so all you other frustrated people can get on with whatever you were doing.

In `_config.yml`, add a new line for

```
future: true
```


#### The Explanation
Github Pages [recently updated](https://github.com/blog/2100-github-pages-now-faster-and-simpler-with-jekyll-3-0) to Jekyll 3. Breaking changes for the most part have been handled well with Github providing instructions in the linked post. 

After much digging around, even going to the point of starting a VM to install Jekyll to build locally since my inferior Windows can't handle it, I found an [issue on Jekyll's repo](https://github.com/jekyll/jekyll/issues/4010) which describes a similar issue.

The problem is, I've always been writing posts without timezone specified in the front matter of my posts, for example, the front matter for this post is

```
---
layout: post
date: 2016-02-05 02:09:06
title: Jekyll 3 No New Posts
tags: [jekyll, github-pages]
---
```

Apparently, somewhere along the line when going from Jekyll 2 to 3, it stopped taking into account the system's timezone somehow, which causes Jekyll to think I am dating the post in the future; this particular Jekyll feature will prevent future posts from being published until the time is right.

Therefore, there are actually a few workarounds for this problem. The first would be to specify a timezone for every datetime stamp in each posts' front matter eg.

```
---
date: 2016-02-05 02:09:06 +0800
---
```

which would frankly be quite annoying to do.

The second, as suggested in the Github issue, would be to set the `timezone` variable in `_config.yml`. This method, however, does not work for me, perhaps I got the value wrong or Singapore is not an accepted timezone.

Finally, the method I suggested at the start of this post, which would effectively disable Jekyll's feature of preventing future posts too.