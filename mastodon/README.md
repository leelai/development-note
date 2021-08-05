# Mastodon development note

## Website
- [Project home](https://joinmastodon.org/)
- [Source](https://github.com/mastodon/mastodon)

## Developer Environment Setup

- Macbook M1(apple silicon)
- postgres (https://postgresapp.com/)
- redis (brew install redis)
- nvm (node managment tool https://github.com/nvm-sh/nvm)
- node 12
- [rbenv](https://github.com/rbenv/rbenv) (brew install rbenv)
- ruby 2.7.2
  
# Step by step

```
$ git clone https://github.com/mastodon/mastodon.git
$ cd mastodon
$ npm install
```

## Create env file

```
$ touch .env
```

Add below to .env
```
OAUTH_REDIRECT_AT_SIGN_IN=false
LOCAL_DOMAIN=localhost:3000
LOCAL_URL_SCHEME=http
```

## Run redis server

```
redis-server
```

## Prepare db in postgres

```
export RAILS_ENV=development
/Users/leelai/.gem/ruby/2.7.0/bin/rails db:setup
```


## Start mastodon

```
foreman start
```
then visit http://localhost:3000

## Truble shooting
### Bundle install error
- ERROR: could not find idn library!
```
brew install libidn

gem install idn-ruby -v '0.1.0' -- --with-idn-dir=/opt/homebrew/Cellar/libidn/1.38/
```

### Foreman start error

- database "mastodon_development" does not exist

```
$ export RAILS_ENV=development
$ /Users/leelai/.gem/ruby/2.7.0/bin/rails db:setup
```
- http parser error (不確定error log是什麼)

```
Add below into Gemfile
gem 'http-parser', '~> 1.2.3'
```

### Ruby install 2.7.2 problem

- readline compile fail
- 發現是iterm2環境髒掉, 改用內建term可以解決(髒掉只是猜測,問題尚未釐清)

### Ruby version not correct

```
$ rbenv init -
$ rbenv local 2.7.2 
```

## Misc

- [eval](https://unix.stackexchange.com/questions/23111/what-is-the-eval-command-in-bash)