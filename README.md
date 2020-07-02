# What is it ?
This is the site published in https://geovation.github.io
Github runs Jekyll automatically. So you can write Jekyll markdown pages and Github will convert it in a website. In this site, we are using a template called [Minimal Mistakes Jekyll Theme](https://mmistakes.github.io/minimal-mistakes/)
More info about Jekyll in https://jekyllrb.com/

# How to write a new blog ?
just create a new page in `_posts` using the naming convention (data_title.md). See the existing ones !

# How to test ?
just run `docker-compose up` and open `http://localhost:4000` with a browser with `open http://localhost:4000`.
To update the packages just run `docker run --rm --volume="$PWD:/srv/jekyll" -it jekyll/jekyll bundle update`
For more info read https://github.com/envygeeks/jekyll-docker/blob/master/README.md
