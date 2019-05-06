[Mastodon](https://github.com/tootsuite/mastodon)
===
https://joinmastodon.org

## Install
1. `curl https://raw.githubusercontent.com/tootsuite/mastodon/master/.env.production.sample -o .env.production`
1. `docker-compose run --rm web bundle exec rake mastodon:setup`
1. `docker-compose up -d`

## use custom source code
1. `git clone https://github.com/user/mastodonrepo app`
2. remove `build` comment out for `docker-compose.yml`
3. run `docker-compose build`
