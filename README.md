
```
$ docker run -id --volume=$(pwd):/srv/jekyll --publish=4000:4000 --name=jekyll -e "TZ=Asia/Taipei" jekyll/jekyll:3.5.1 bash

$ docker exec -ti jekyll bash
```

```shell=
$ cd /srv/jekyll

$ bundle install

$ bundle exec jekyll serve
```