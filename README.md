# Arooo - A Room Of One's Own
[![Build Status](https://travis-ci.org/doubleunion/arooo.svg)](https://travis-ci.org/doubleunion/arooo)

## Welcome :rocket::rocket::rocket:

This is a membership application app written by members of [Double Union](http://doubleunion.org/), a feminist hacker/makerspace for women in San Francisco.

This application has a lot of cool features, including:
* Prospective members can apply for membership
* Current members can vote and comment on applications
* Current members can see a directory of members
* Current members can pay dues via Stripe
* Membership coordinators can manage member status

The application supports three levels of membership: members, key members, and voting members, where any member can see and comment on an application, but only voting members can vote. Membership coordinators can set whether the app is accepting applications, accept or reject individual applications, manage membership levels, and review dues status.

It is currently very Double Union specific, but we are in the process of extracting the Double Union things out of it, so that other feminist hacker/makerspaces can use it, too! We are open sourcing the app now so that we can work with other hacker/makerspaces on that process. This app supports an application process that helps us maintain a safe space for our members, and we want this app to help other groups that have the same goal.

If you're interested in using Arooo for your org and want to get in touch, [make an issue](https://github.com/doubleunion/arooo/issues/new) and we can chat. :dancer:

## How to Contribute

We use [GitHub issues](https://github.com/doubleunion/arooo/issues) for feature development and bug tracking, so take a look for things that you can work on, and comment with any questions you have about half-baked issues.

## Development setup

If you are new to Rails, follow the [RailsBridge Installfest instructions](http://installfest.railsbridge.org/installfest/) for getting your environment set up.

1. Standard Rails app setup
    * `cp config/database.example.yml config/database.yml`
    * Optional: edit database.yml if you want to change advanced things
    * `rake db:create`
    * `rake db:migrate`
    * `rake db:test:prepare`

1. Go here and set up an application: http://github.com/settings/applications/new
    * Application name: Whatever you want
    * Homepage URL: http://localhost:3000
    * Authorization callback URL: http://localhost:3000/auth/github/callback

1. `cp config/application.example.yml config/application.yml`

1. Edit config/application.yml
    * set `GITHUB_CLIENT_KEY` and `GITHUB_CLIENT_SECRET` to the Client ID and
      Client Secret from your Github application
    * don't forget to restart your Rails server so it can see your shiny new GitHub key & secret

1. Use `rake secret` to generate a secret token, then set that as an environment variable called SECRET_TOKEN.
   `export SECRET_TOKEN= <what rake secret generated >`

## Specs

Write specs! Yay! Especially for anything involving authorization walls.

Run `rake db:test:prepare` after you pull or make any changes to the app, generally

Make sure `bundle exec rspec` passes before pushing your changes.

## Rails console

Development: `$ bundle exec rails console`

Production: `$ heroku run rails console`

## Populate development database

To add a bunch of users to your dev database, you can use `bundle exec rake
populate:users`. They will have random states.

## User states

The current User state machine can be found in `app/models/user.rb`, but since
it probably won't change much and it's the main moving piece of the
application, here's a quick summary.

Valid states:

* `visitor`: default state, no admin access, no application access
* `applicant`: not yet a member, no admin access, can only
  view/edit/save/submit their application
* `member`: access to member admin section, cannot vote
* `key_member`: access to member admin section, cannot vote, has keys to the space
* `voting_member`: access to member admin section, can vote, has keys to the space

Manually changing a user's state from the rails console:
```
> user = User.find_by_username('someusername')
> user.state # => "visitor"

> user.make_applicant!  # => true
> user.make_member!     # => true
> user.make_key_member! # => true
> user.make_voting_member! # => true
> user.make_applicant!  # => raises invalid state transition error

> user.update_attribute(:state, 'applicant') # bypasses normal checks & succeeds
```

## Contributing

If you are new to GitHub, you can [use this guide](http://railsbridge.github.io/bridge_troll/) for help making a pull request.

1. Fork it
1. Get it running
1. Create your feature branch

  ```
  git checkout -b my-new-feature
  ```

1. Write your code and specs
1. Commit your changes

  ```
  git commit -am 'Add some feature'
  ```

1. Push to the branch

  ```
  git push origin my-new-feature
  ```

1. Create a new Pull Request, linking to the GitHub issue url the Pull Request is fixing in the description
1. If you find bugs, have feature requests or questions, please file an issue.


## Reviewing

This section only pertains if you have doubleunion/doubleunion write/push access.

1. Read through the GitHub issue that the Pull Request is fixing
1. Code review the Pull Request, commenting on any potential issues, improvements, or telling the person how awesome their code is
1. After the Pull Request is reviewed & fixed (if necessary) and the Travis CI build is passing, merge the Pull Request into `master`
1. Delete the branch, and close the relevant issue if not referenced in the Pull Request already
1. Deploy! (see below)


## Deploying

This section only pertains if you have Heroku & Deployment access.

1. Add Heroku remotes to your `.git/config`

  ```
  [remote "production"]
     url = git@heroku.com:doubleunion.git
     fetch = +refs/heads/*:refs/remotes/heroku/*
  [remote "staging"]
     url = git@heroku.com:doubleunion-staging.git
     fetch = +refs/heads/*:refs/remotes/heroku/*
  ```

1. Pull down the latest code from `master`

  ```
  git checkout master
  git pull --rebase origin master
  ```

1. If Travis CI tests are passing, push to the `staging` environment

  ```
  git checkout master
  git pull --rebase origin master
  git push staging master
  ```

1. If needed, perform rake tasks or set ENV variable settings on `staging`
1. Test [staging](http://doubleunion-staging.herokuapp.com/)!

  ```
  username: doubleunion
  password: meritocracyisajoke
  ```

1. After confirming that the code works on `staging`, push it to `production`!

  ```
  git checkout master
  git pull --rebase origin master
  git push production master
  ```

1. If needed, perform rake tasks or set ENV variable settings on `production`

## A Room of One's Own

This app is named after a famous Virginia Woolf essay. You can learn more about it [on Wikipedia](http://en.wikipedia.org/wiki/A_Room_of_One's_Own)!

Also, here is a puppy that is saying "arooo":

[![A puppy howling](http://cdn3.sbnation.com/assets/2875387/v728228.gif)](https://www.youtube.com/watch?v=2Tgwrkk-B3k)

## Security

If you find a security vulnerability with Arooo, please let us know at admin@doubleunion.org. Thank you!

## License

Copyright (C) 2014 Double Union

This program is free software: you can redistribute it and/or modify it under the terms of the GNU General Public License as published by the Free Software Foundation, either version 3 of the License, or (at your option) any later version.

This program is distributed in the hope that it will be useful, but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

See the LICENSE.txt file for the full license.
