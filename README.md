# passport-playstrategy

[*Based on passport-lichess*](https://github.com/ornicar/passport-lichess)

[![npm](https://img.shields.io/npm/v/passport-playstrategy)](https://www.npmjs.com/package/passport-playstrategy)

[Passport](http://passportjs.org/) strategy for authenticating with [PlayStrategy](https://playstrategy.org) using the OAuth 2.0 API.

This module lets you authenticate using PlayStrategy in your Node.js applications. By plugging into Passport, PlayStrategy authentication can be easily and unobtrusively integrated into any application or framework that supports [Connect](http://www.senchalabs.org/connect/)-style middleware, including [Express](http://expressjs.com/).

## Install

```bash
$ npm install passport-playstrategy

# or yarn installation

$ yarn add passport-playstrategy
```

## Usage

#### Create an Application

Before using `passport-playstrategy`, you must register an application with PlayStrategy. If you have not already done so, a new application can be created at [developer applications](https://playstrategy.org/account/oauth/app) within PlayStrategy's settings panel. Your application will be issued a client ID and client secret, which need to be provided to the strategy. You will also need to configure a callback URL which matches the route in your application.

#### Configure Strategy

The PlayStrategy authentication strategy authenticates users using a PlayStrategy account and OAuth 2.0 tokens. The client ID and secret obtained when creating an application are supplied as options when creating the strategy. The strategy also requires a `verify` callback, which receives the access token and optional refresh token, as well as `profile` which contains the authenticated user's PlayStrategy profile. The `verify` callback must call `cb` providing a user to complete authentication.

```js
var PlayStrategyStrategy = require('passport-playstrategy').Strategy;

passport.use(new PlayStrategyStrategy({
    clientID: PLAYSTRATEGY_CLIENT_ID,
    clientSecret: PLAYSTRATEGY_CLIENT_SECRET,
    callbackURL: "http://127.0.0.1:3000/auth/playstrategy/callback"
  },
  function(accessToken, refreshToken, profile, cb) {
    User.findOrCreate({ playstrategyId: profile.id }, function (err, user) {
      return cb(err, user);
    });
  }
));
```

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'playstrategy'` strategy, to authenticate requests.

For example, as route middleware in an [Express](http://expressjs.com/) application:

```js
app.get('/auth/playstrategy',
  passport.authenticate('playstrategy'));

app.get('/auth/playstrategy/callback',
  passport.authenticate('playstrategy', { failureRedirect: '/login' }),
  function(req, res) {
    // Successful authentication, redirect home.
    res.redirect('/');
  });
```

## Examples

Developers using the popular [Express](http://expressjs.com/) web framework can refer to an [example](https://github.com/passport/express-4.x-facebook-example) as a starting point for their own web applications. The example shows how to authenticate users using Facebook.  However, because both Facebook and PlayStrategy use OAuth 2.0, the code is similar. Simply replace references to Facebook with corresponding references to PlayStrategy.

## Credits

This library is a based on [Thibault's passport-lichess](https://github.com/ornicar/passport-lichess) which is based on [Jared Hanson's passport-github](https://github.com/jaredhanson/passport-github).