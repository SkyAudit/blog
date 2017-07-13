Skycoin Blog
============

http://blog.skycoin.net/blog

Hosted on github pages.

This blog uses [hugo](https://gohugo.io/) to generate a static website from markdown files.

Refer to hugo documentation for full detail.

Content: Create or Amend Posts
==============================

Look in the `content/` folder.  Posts are written in markdown.

Locally, the blog can be previewed with:

```sh
hugo server -theme=hugo-xmin
```

Once ready to publish, build the static files:

```sh
hugo
```

This will write static files to `docs/`.

Then, commit the changes and push.

Themes: Layout and Styling
==========================

Look in the `themes/` folder and refer to hugo docs.
