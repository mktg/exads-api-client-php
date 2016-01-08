# Exads API PHP Client

A simple Object Oriented wrapper for Exads API, written with PHP5.

See Exads API's documentation.

## Features

* Follows PSR-0 conventions and coding standard: autoload friendly
* API entry points implementation state :
 * Campaign
 * Collection
 * Login
 * Payment
 * Site
 * Statistics
 * User
 * Zones

## Requirements

* PHP >= 5.4
* The PHP [cURL](http://php.net/manual/en/book.curl.php) extension
* The PHP [JSON](http://php.net/manual/en/book.json.php) extension
* [PHPUnit](https://phpunit.de/) >= 4.0 (optional) to run the test suite

## Install

### Composer

[Composer](http://getcomposer.org/download/) users can simply run:

```bash
$ php composer.phar require exads/exads-api-client-php:~1.0
```

at the root of their projects. To utilize the library, include
Composer's `vendor/autoload.php` in the scripts that will use the
`Exads` classes.

For example,

```php
<?php

// This file is generated by Composer
require_once 'vendor/autoload.php';
$client = new Exads\Client('https://api.exads.com/url/');
// ...
```

### Standalone (not recommended)

The library ships with a basic autoload.php file which allows you to use it without composer.
See the latest version available : https://github.com/EXADS/exads-api-client-php/releases


```bash
$ mkdir vendor
$ wget -q https://github.com/EXADS/exads-api-client-php/archive/v1.1.1.tar.gz
$ tar -xf v1.1.1.tar.gz -C vendor/
$ rm v1.1.1.tar.gz
```

Then your bootstrap script should look like :

```php
<?php

// This file ships with the library
require 'vendor/exads-api-client-php-1.0.0/lib/autoload.php';
$client = new Exads\Client('https://api.exads.com/url/');
```

## Basic usage of `exads-api-client-php` client


```php
<?php

require_once 'vendor/autoload.php';

try {
    $client = new Exads\Client('https://api.exads.com/url/');
    $apiToken = $client->login->getToken('username', 'password');
    $client->setApiToken($apiToken);

    // ...
    $campaigns = $client->campaigns->all();
    // ...
} catch (\Exception $e) {
    die($e->getMessage());
}
```

## Basic usage of `exads-api-client-php` client with API token authentication


```php
<?php

require_once 'vendor/autoload.php';

try {
    $client = new Exads\Client('https://api.exads.com/url/');
    $apiToken = $client->login->getToken('APItoken');
    $client->setApiToken($apiToken);

    // ...
    $campaigns = $client->campaigns->all();
    // ...
} catch (\Exception $e) {
    die($e->getMessage());
}
```

See `test/Exads/Tests/UrlsTest.php` for a full list of available methods.

#### Passing parameters to end points

As describe in the API documentation, most of the `GET` entry points accept parameters for filtering the result of the call.
In particular, for long collections, you should use `offset` for paginating the results the API returns (you cannot get more than 50 elements out of 1 call, that's when offset comes in play).
See the following examples :

```php
<?php
...

// Getting the total number of carriers
$client->collections->carriers(array('count' => true));

// Getting the carriers 50..100
$client->collections->carriers(array('offset' => 50));
```

Please refer to the documentation for the full list of parameters allowed for each entry point.