if you are brave to install bundle in your machine:

* ```bundle exec jekyll serve --drafts```
* ```bundle exec jekyll help```
* ```bundle exec jekyll draft "my new post"```
* ```bundle exec jekyll publish _drafts/my-new-post.md```

if you have docker you can run all those commands like this: `docker run --rm --volume="$PWD:/srv/jekyll" -it jekyll/jekyll bundle update`

or if you want to serve the site locally just run `docker-compose up`

for more commands using docker see https://github.com/envygeeks/jekyll-docker/blob/master/README.md
