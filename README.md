# Oracular Vernacular

This repository contains the blog of [Rhombus
Network](https://rhombus.network/).

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
bundle exec jekyll serve --incremental
```

## Creating a post

To create a post, start by creating a new branch.

Then duplicate `_posts/3018-12-15-test.md`, and rename
it with the naming convention `YYYY-MM-DD-blog-title.md`.

Update an image for the post by uploading it to `/assets/images`.

Change the YAML front matter to reflect the name of the author, their twitter
handle, the date, etc.

If the date is in the future, the post will become live on that date.

The post is simply markdown.

## Publishing a post

Create a pull-request with your post, and ask for a review.

To deploy the post, simply merge to `master`.

Please avoid lines greater than 80 characters. You may wish to use a code
autoformatter such as `prettier` to enforce this.
