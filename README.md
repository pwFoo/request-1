Anax Request
==================================

[![Latest Stable Version](https://poser.pugx.org/anax/request/v/stable)](https://packagist.org/packages/anax/request)
[![Join the chat at https://gitter.im/mosbth/anax](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/canax?utm_source=badge&utm_medium=badge&utm_campaign=pr-badge&utm_content=badge)

[![Build Status](https://travis-ci.org/canax/request.svg?branch=master)](https://travis-ci.org/canax/request)
[![CircleCI](https://circleci.com/gh/canax/request.svg?style=svg)](https://circleci.com/gh/canax/request)

[![Build Status](https://scrutinizer-ci.com/g/canax/request/badges/build.png?b=master)](https://scrutinizer-ci.com/g/canax/request/build-status/master)
[![Scrutinizer Code Quality](https://scrutinizer-ci.com/g/canax/request/badges/quality-score.png?b=master)](https://scrutinizer-ci.com/g/canax/request/?branch=master)
[![Code Coverage](https://scrutinizer-ci.com/g/canax/request/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/canax/request/?branch=master)

[![SensioLabsInsight](https://insight.sensiolabs.com/projects/4f795dfa-05de-495b-b40a-32b5728c6143/mini.png)](https://insight.sensiolabs.com/projects/4f795dfa-05de-495b-b40a-32b5728c6143)

Anax Request module for wrapping all request related information.

The module essentially wraps access to `$_GET, $_POST, $_SERVER`, information send through the HTTP BODY and calculates url details (current, site, base, route path) from the request uri.

The module provides a framework unified way to access these global variables and it provides a way to inject a certain setup when using the module for unit testing.



Table of content
------------------

* [Class, interface, trait](#class-interface-trait)
* [Configuration file](#configuration-file)
* [DI service](#di-service)
* [Access as framework service](#access-as-framework-service)



Class, interface, trait
------------------

The following classes, interfaces and traits exists.

| Class, interface, trait            | Description |
|------------------------------------|-------------|
| `Anax\Request\Request`             | Wrapper class for request details and related. |



Configuration file
------------------

There is no configuration file for this module.



DI service
------------------

The session is created as a framework service within `$di`. The following is a sample on how the session service is created.

```php
"request" => [
    "shared" => true,
    "callback" => function () {
        $request = new \Anax\Request\Request();
        $request->init();
        return $request;
    }
],
```

1. The object is created.
1. The init-method reads information from the environment to find out the url of the request.

The service is lazy loaded and not created until it is used.



Access as framework service
------------------

You can access the module as a framework service.

```php
# $app style
$app->request->getRoute();

# $di style, two alternatives
$di->get("request")->getRoute();

$request = $di->get("request");
$request->getRoute();
```



Create and init an object
------------------

This is how the object can be created. This is usually done within the framework as a sevice in `$di`.

```php
# Create an object
$request = new \Anax\Request\Request();

# Set (reset) globals, useful in unit testing
# when not using $_GET, $_POST, $_SERVER
$request->setGlobals();

# Init the class by extracting url parts and
# route path.
$request->init();
```



Extract url and route parts
------------------

When the object is initiated you can extract url and route parts from it. This is based on the current url.

```php
# Get site url including scheme, host and port.
$request->getSiteUrl();

# Get base url including site url and path to current index.php.
$request->getBaseUrl();

# Get current url as base url and attach
# the query string.
$request->getCurrentUrl();

# Get script name, index.php or other.
$request->getScriptName();

# Get HTTP request method, for example
# GET, POST, PUT, DELETE.
$request->getMethod();

# Get route path as a string.
$request->getRoute();

# Get route path parts in an array.
$request->getRouteParts();
```



Get and set `$_SERVER`
------------------

You can get and set values in the PHP global variable `$_SERVER`.

```php
# Read a value
$request->getServer($key);

# Read a value and use $default if $key is not set.
$request->getServer($key, $default);

# Set a value
$request->setServer($key, $value);
```

You are reading and setting values in a copy of `$_SERVER`, so you are not actually editing the global variable, just the internal representation within the class.



Get and set `$_GET`
------------------

You can get and set values in the PHP global variable `$_GET`.

```php
# Read a value
$request->getGet($key);

# Read a value and use $default if $key is not set.
$request->getGet($key, $default);

# Set a value
$request->setGet($key, $value);
```

You are reading and setting values in a copy of `$_GET`, so you are not actually editing the global variable, just the internal representation within the class.



Get and set `$_POST`
------------------

You can get and set values in the PHP global variable `$_POST`.

```php
# Read a value
$request->getPost($key);

# Read a value and use $default if $key is not set.
$request->getPost($key, $default);

# Set a value
$request->setPost($key, $value);
```

You are reading and setting values in a copy of `$_POST`, so you are not actually editing the global variable, just the internal representation within the class.



Get and set request body
------------------

You can get and set the value in the HTTP request body. Sometimes the HTTP request body is used to send parameters to an route.

```php
# Read the body
$request->getBody();

# Set the body
$request->setBody($content);
```

You are setting values in a copy of the actual body, so you are not actually editing it, just the internal representation within the class.



License
------------------

This software carries a MIT license.



```
 .  
..:  Copyright (c) 2013 - 2018 Mikael Roos, mos@dbwebb.se
```
