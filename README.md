<img align="right" width="175px" src="https://camo.githubusercontent.com/7e57ebd8fa0125653e3b41c87fc4d3a6b61964fc/687474703a2f2f692e696d6775722e636f6d2f7663355a56714c2e706e673f32" />

Symfony Docker Edition 
========================

The *unofficial* Symfony Docker Edition – by [@arzek](https://github.com/arzek)

- PHP 7.1 + PHP-FPM
- Nginx
- Xdebug
- Opcache
- Symfony installer

Table of Contents
==================
- [Installation](#installation)
- [FAQ](#faq)

## Installation

> Before anything, you need to make sure you have Docker properly setup in your environment. For that, refer to the official documentation for both [Docker](https://docs.docker.com/) and [Docker Compose](https://docs.docker.com/compose/). Also, if you're developing on Mac or Windows – *yeah, maybe that's the case* –, make sure you have [Docker Machine](https://docs.docker.com/machine/) properly setup.

1. Install/clone symfony to the symfony folder
2. Build docker-compose
    ```bash
    docker-compose build
    ```
3. Run docker-compose 
    ```bash
    docker-compose up -d
    ```
4. http://localhost/

## Faq
If you have such a problem - `You are not allowed to access this file. Check app_dev.php for more information.`.
Edit app_dev.php:
```php
<?php

use Symfony\Component\Debug\Debug;
use Symfony\Component\HttpFoundation\Request;

// If you don't want to setup permissions the proper way, just uncomment the following PHP line
// read https://symfony.com/doc/current/setup.html#checking-symfony-application-configuration-and-setup
// for more information
//umask(0000);

// This check prevents access to debug front controllers that are deployed by accident to production servers.
// Feel free to remove this, extend it, or make something more sophisticated.
//if (isset($_SERVER['HTTP_CLIENT_IP'])
//    || isset($_SERVER['HTTP_X_FORWARDED_FOR'])
//    || !(in_array(@$_SERVER['REMOTE_ADDR'], ['127.0.0.1', '::1'], true) || PHP_SAPI === 'cli-server')
//) {
//    header('HTTP/1.0 403 Forbidden');
//    exit('You are not allowed to access this file. Check '.basename(__FILE__).' for more information.');
//}

require __DIR__.'/../vendor/autoload.php';
Debug::enable();

$kernel = new AppKernel('dev', true);
if (PHP_VERSION_ID < 70000) {
    $kernel->loadClassCache();
}
$request = Request::createFromGlobals();
$response = $kernel->handle($request);
$response->send();
$kernel->terminate($request, $response);

```

