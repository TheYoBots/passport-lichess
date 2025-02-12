# passport-lishogi

[*Based on passport-lichess*](https://github.com/ornicar/passport-lichess)

[![npm](https://img.shields.io/npm/v/passport-shogi)](https://www.npmjs.com/package/passport-shogi)

[Passport](http://passportjs.org/) strategy for authenticating with [Lishogi](https://lishogi.org)
using the OAuth 2.0 API.

This module lets you authenticate using Lishogi in your Node.js applications.
By plugging into Passport, Lishogi authentication can be easily and
unobtrusively integrated into any application or framework that supports
[Connect](http://www.senchalabs.org/connect/)-style middleware, including
[Express](http://expressjs.com/).

## Install

```bash
$ npm install passport-shogi
```

## Usage

#### Configure Strategy

The Lishogi authentication strategy authenticates users using a Lishogi account
and OAuth 2.0 tokens. The client ID and secret obtained when creating an
application are supplied as options when creating the strategy. The strategy
also requires a `verify` callback, which receives the access token and optional
refresh token, as well as `profile` which contains the authenticated user's
Lishogi profile. The `verify` callback must call `cb` providing a user to
complete authentication.

```js
var LishogiStrategy = require('passport-shogi').Strategy;

passport.use(
  new LishogiStrategy(
    {
      clientID: 'arbitrary-unique-id',
      callbackURL: 'http://127.0.0.1:3000/auth/lishogi/callback',
    },
    function (accessToken, refreshToken, profile, cb) {
      User.findOrCreate({ lishogiId: profile.id }, function (err, user) {
        return cb(err, user);
      });
    }
  )
);
```

#### Authenticate Requests

Use `passport.authenticate()`, specifying the `'lishogi'` strategy, to
authenticate requests.

For example, as route middleware in an [Express](http://expressjs.com/)
application:

```js
app.get('/auth/lishogi', passport.authenticate('lishogi'));

app.get('/auth/lishogi/callback', passport.authenticate('lishogi', { failureRedirect: '/login' }), function (req, res) {
  // Successful authentication, redirect home.
  res.redirect('/');
});
```

## Examples

Developers using the popular [Express](http://expressjs.com/) web framework can
refer to an [example](https://github.com/passport/express-4.x-facebook-example)
as a starting point for their own web applications. The example shows how to
authenticate users using Facebook. However, because both Facebook and Lishogi
use OAuth 2.0, the code is similar. Simply replace references to Facebook with
corresponding references to Lishogi.

## Credits

This library is a based on [Jared Hanson's passport-github](https://github.com/jaredhanson/passport-github).