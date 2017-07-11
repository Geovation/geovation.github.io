# Geovation Blog

This static site is based on [Jeykll](https://jekyllrb.com/).

You can see the original instructions on the internal blog here:

https://geovation.atlassian.net/wiki/display/ET/2017/01/05/Geovation+Technical+Blog

For those without access...

To get started, create a fork, clone and install it like this:

```
git clone git@github.com:jamesgardnergeovation/geovation.github.io.git
cd geovation.github.io.git
gem install jekyll jekyll-paginate jekyll-gist --user-install
gem install jekyll -v 3.4.3 --user-install
```

You can then serve a local version (including your drafts) like this:

```
~/.gem/ruby/2.0.0/bin/jekyll serve --drafts
```

After a few seconds and a bit of output, you should see your site at http://127.0.0.1:4000.

You can add new draft posts in the `_drafts` folder.

The rest of this `README.md` is for the original Hyde theme used to create this site.

# Hyde

If you want to play with Jekyll to see what it does:

- Install Jekyll: `$ sudo gem install jekyll`
- Install gems used by Jekyll: `$ sudo gem install jekyll-paginate jekyll-gist`
- Install Pygments decorator (install Python with Homebrew first, if you haven't already done so): `$ sudo python -m pip install Pygments`
- Install the Pygments gem: `$ gem install pygments.rb`
- Clone this repo.
- Build the site: `$ jekyll build`
- Run the server: `$ jekyll serve`
- Navigate to the blog (http://127.0.0.1:4000/)


You’re done!

# Create a Blog Post
## Creating Post Files

To create a new post, all you need to do is create a file in the `_posts` directory. How you name files in this folder is important. Jekyll requires blog post files to be named according to the following format: `YEAR-MONTH-DAY-title.MARKUP` where `YEAR` is a four-digit number, `MONTH` and `DAY` are both two-digit numbers, and `MARKUP` is the file extension representing the format used in the file.

For example, the following are examples of valid post filenames:

- `2011-12-31-new-years-eve-is-awesome.md`
- `2012-09-12-how-to-write-a-blog.md`

All post files must contain the following YAML Front Matter block at the start (including the triple-dash lines at the start and end of the block):

```sh
---
layout: post
title: the title of your post
meta: A summary that will appear under the link on the blog homepage
author: your name
comments: true if you want people to be able to comment using Disqus, false otherwise
---
```

Once you’re happy with your post, simply add, commit and push it to this repo using git.  It’ll appear on the site immediately with no further action required.

## Working with Drafts
Drafts are posts without a date. They’re posts you’re still working on and don’t want to publish yet. To create a draft create a file in the `_drafts` folder at the site root:

```
|-- _drafts/
|   |-- a-draft-post.md
```

To preview your site with drafts, simply run jekyll serve or jekyll build with the `--drafts` switch. Each will be assigned the value modification time of the draft file for its date, and thus you will see currently edited drafts as the latest posts.

You can add a draft to the repo while the post is being developed - it won’t be automatically published until you move it to the `_posts` folder (with the appropriate filename, as described above) and commit it to this repo.

That’s all you need to get going.
