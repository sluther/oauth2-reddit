# Reddit OAuth2 Provider

[![Build Status](https://img.shields.io/travis/rtheunissen/oauth2-reddit.svg?style=flat-square)](https://travis-ci.org/rtheunissen/oauth2-reddit)
[![Scrutinizer](https://img.shields.io/scrutinizer/g/rtheunissen/oauth2-reddit.svg?style=flat-square)]()
[![Scrutinizer Coverage](https://img.shields.io/scrutinizer/coverage/g/rtheunissen/oauth2-reddit.svg?style=flat-square)]()
[![Latest Version](https://img.shields.io/packagist/v/rtheunissen/oauth2-reddit.svg?style=flat-square)](https://packagist.org/packages/rtheunissen/oauth2-reddit)
[![License](https://img.shields.io/packagist/l/rtheunissen/oauth2-reddit.svg?style=flat-square)](https://packagist.org/packages/rtheunissen/oauth2-reddit)
[![Join the chat at https://gitter.im/rtheunissen/oauth2-reddit](https://img.shields.io/badge/gitter-join%20chat%20%E2%86%92-brightgreen.svg?style=flat-square)](https://gitter.im/rtheunissen/oauth2-reddit)

This package provides Reddit integration for [thephpleague/oauth2-client](https://github.com/thephpleague/oauth2-client).

## Installation

```sh
composer require rtheunissen/oauth2-reddit
```

## Usage

```php
use Rudolf\OAuth2\Client\Provider\Reddit;

$reddit = new Reddit([
    'clientId'      => 'yourClientId',
    'clientSecret'  => 'yourClientSecret',
    'redirectUri'   => 'yourRedirectUri',
    'userAgent'     => 'platform:appid:version, (by /u/username)',
    'scopes'        => ['identity', 'read', ...],
]);
```

#### Requesting an access token 

```php
$url = $reddit->getAuthorizationUrl([
    'duration' => 'permanent', // in order to receive a refresh token
]);
```


After being redirected back, you'll have both code and state parameters.

```php
$accessToken = $reddit->getAccessToken('authorization_code', [
    'code'  => $code,
    'state' => $state
]);
```

#### Requesting an access token without redirecting

You can request access to a 'script' app by using a username and password.
In this case you don't need to use `getAuthorizationUrl`.

```php
$accessToken = $reddit->getAccessToken('password', [
    'username' => $username,
    'password' => $password,
]);

```


#### Using the access token

Reddit requires a few authorization headers to allow authenticated requests using an access token. 
These can be accessed using `$reddit->getHeaders($token)`.
