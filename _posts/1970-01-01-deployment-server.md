---
layout: post
title: Deployment and Server
order: 230
---

TODO

* [Capistrano](http://capistranorb.com/)

Alternative solutions like Mina do not have any major advantages at the moment.
Or a custom Deploy workflow for big projects with tools like Ansible

### VPS, Dedicated server

* [DigitalOcean](https://m.do.co/c/20534050b97f) (my ref link ;) you receive $10 credit after registration)
* [Amazon Web Services]()
* [Namecheap, domains registrator](https://www.namecheap.com/?aff=62428) (my ref link ;) )

### PaaS

* [Heroku](https://www.heroku.com/)

### CI 

* [Jenkins](https://jenkins.io/)
* [Travis CI](https://travis-ci.org/)
* [CircleCI](https://circleci.com/)

### DevOps

TODO

* Ansible
* Chef
* Puppet
* Docker

### Local development

TODO

* Vagrant
* Docker

### Local tunnels. Exposing dev env in the Internet

The solution for a question "how do I expose a local server behind a NAT or firewall to the internet?". Literally when your dev machine does not have a dedicated IP, but you need to...

* make a demo of localhost web app (and can not deploy it to some server)
* make a quick test of web app (api) on your mobile device
* accept a webhook from some service or oAuth redirect

Local tunnels allows you to easily share a local web service development machine without messing with DNS and firewall settings. And here are some free tools:

**Ngrok** is a very powerful standalone tool. Has a lot of paid options, but very basic are given for free.

* no dependencies, just download `ngrok` binary
* web page with connection stats
* supports config file 

Expose `localhost:3000` as easy as 

```
ngrok http 3000
```  

You'll get url like this `https://83832de1.ngrok.io`

Another way, if you already use Vagrant - the easies way is to use **Vagrant Share**

```
vagrant share
```

You will need to make one time registration at [https://atlas.hashicorp.com/](https://atlas.hashicorp.com/), then to run `vagrant login` with registered credentials.

Vagrant detects where your HTTP server is running in your Vagrant environment and outputs the endpoint that can be used to access this share.  
You'll get url like this `http://lucky-man-7777.vagrantshare.com`
As a bonus it supports SSH sharing via 

```
vagrant share --ssh
```

* [Ngrok](https://ngrok.com/)
* [Vagrant Share](https://docs.vagrantup.com/v2/share/index.html)

### Protect non-production servers with HTTP Auth

Basic Auth could be used as the main form of authentication for very simple apps, or admin backends. It is often used to prevent access to staging servers by strangers and search engines (to prevent indexing of non-production pages).

In Rails, the simplest basic auth could be added like this:

```ruby
class ApplicationController < ActionController::Base
  http_basic_authenticate_with name: "admin", password: "hunter2"
end
```

More advanced ways can be found in the [Rails docs](http://api.rubyonrails.org/classes/ActionController/HttpAuthentication/Basic.html).

In any other Rack based server, you can use the `Rack::Auth::Basic` middleware:

```ruby
use Rack::Auth::Basic, "Restricted Area" do |username, password|
  [username, password] == ['admin', 'pass']
end
```
