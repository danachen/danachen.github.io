---
layout: post
title: Customizing a blog and portfolio site with Jekyll
category: blog
---

It's been a while since I've worked on this site. Although Jekyll was relatively easy to set up direclty from Github, customizing it has been a pain. When I first forked the site a few years ago, the default theme was, and still is, Minima. And attempt to switch up the theme results in a plethora of dependency issues, whether bewteen Github pages and Jekyll, or between Github pages and Ruby version, or between a plug-in and Jekyll. I gave up trying to customize it and let it sit in dust.

But alas, it's high time the site gets a makeover. I cloned a local version of the site, and here's a few simple things done to spruce it up.

For starters, I changed the avatar specified in `_config.yml` from the one imported from Github, to a specified one in the image folder `avatar: ../images/my_img.jpg`. 

Then I added a new subsection to the site that would house projects. I'm sure there's a multitude of ways how this can be achieved, but I simply filtered blog entries, and had them placed in either a `blog` section, or a `project section` based on categories.

- Create two folders, `blog/_posts`, and `projects/_posts`, and move the relevant blog posts from `_posts` into their respective folders. Note that the folder name `_posts` is how the theme picks up the blog posts, and folder(s) of this name can be placed in the root folder or any number of subfolders. The folder `_posts` is no longer necessary.
- Add the label category to the top of every blog post page. You probably already have something like: `layout: post` specified. Now we need to add `category: blog` or `category: project` to each post in the two folders created above.
- In `_layouts/default.html`, add the nav menu item, in this case: `<a href="{{ site.baseurl }}/projects">Projects</a>`. This will require we create the project page below.
- In the current menu configuration, the home page is configured by `index.html`. Since we had added the expectation of a new project page above, we need to create a `project.html` page.
- To allow filtering of content based on categories, we need to filter blog posts based on the relevant category. In the case of projects, we need to add `{% if post.category == "project" %} ...  {% endif %}` to the iteration of site posts. In the case of blog posts, we need to add `{% if post.category == "blog" %} ...  {% endif %}`.
