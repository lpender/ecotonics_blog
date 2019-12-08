# Ecotonics

This repository contains the blog of [ecotone](https://ecotone.io/).

Framework: [Jekyll](https://jekyllrb.com/)

Deployment: [Github Pages](https://pages.github.com/)

## Philosophy

The overall philosophy behind building this blog is described most completely
and eloquently in the article [Goodbye Medium, Hello
Jekyll](https://intersect.whitefusion.io/open-web/goodbye-medium-hello-jekyll)

In short, the idea is that we blog to our own personal blog (to maximize
reputational benefits) and then [cross-list to
Medium](https://help.medium.com/hc/en-us/articles/217991468-SEO-and-duplicate-content)
in order to maximize SEO and "network-effect".

## Getting started

```
bundle install
bundle exec jekyll serve
```

## Creating a post

To create a post, start by creating a new branch.

Then duplicate `_posts/3018-12-15-test.md`, and rename
it with the naming convention `YYYY-MM-DD-blog-title.md`.

Update an image for the post by uploading it to `/assets/images`.

Change the YAML front matter to reflect the name of the author, their twitter
handle, the date, etc.

If the date is in the future, the post will become live on that date.

The post content is simply
[markdown](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet).

## Publishing a post

Create a pull-request with your post, and ask for a review.

To deploy the post, simply merge to `master`.

Please avoid lines greater than 80 characters. You may wish to use a code
autoformatter such as `prettier` to enforce this.

## Hiding a post

Hiding posts is accomplished by setting options in the YML front matter.

### So that it is not externally viewable

To hide your post from XML feed, sitemap, and front page.

```
  sitemap: false
```

It will still be viewable by users who go directly to the link.

### So that it is not viewable until a certain date

You can set the date to a date in the future and the post will not be viewable
by anyone until that time.

i.e.

```
  date: 3018-01-20 12:00:00 -0500
```

### So that it cannot be viewed at all

To hide your post from all viewers

```
  published: false
```

## New Post

```
jekyll page "New Page"
jekyll post "New Post"
jekyll draft "New Draft"
```
