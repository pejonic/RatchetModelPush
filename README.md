RatchetModelPush
================

[![Build Status](https://travis-ci.org/WyriHaximus/RatchetModelPush.png)](https://travis-ci.org/WyriHaximus/RatchetModelPush)
[![Latest Stable Version](https://poser.pugx.org/WyriHaximus/ratchet-model-push/v/stable.png)](https://packagist.org/packages/WyriHaximus/ratchet-model-push)
[![Total Downloads](https://poser.pugx.org/wyrihaximus/ratchet-model-push/downloads.png)](https://packagist.org/packages/wyrihaximus/ratchet-model-push)
[![Code Coverage](https://scrutinizer-ci.com/g/WyriHaximus/RatchetModelPush/badges/coverage.png?b=master)](https://scrutinizer-ci.com/g/WyriHaximus/ratchet-model-push/?branch=master)
[![License](https://poser.pugx.org/wyrihaximus/ratchet-model-push/license.png)](https://packagist.org/packages/wyrihaximus/ratchet-model-push)
[![PHP 7 ready](http://php7ready.timesplinter.ch/WyriHaximus/RatchetModelPush/badge.svg)](https://travis-ci.org/WyriHaximus/RatchetModelPush)

Model Push for the [CakePHP Ratchet plugin](http://wyrihaximus.net/projects/cakephp/ratchet/).

## What is Ratchet ##

Ratchet for CakePHP brings [the Ratchet websocket](http://socketo.me/) server to CakePHP. Websockets allow you to utilize near-real-time communication between your application and it's visitors. For example notifying a page the associated record in the database has been updated using the [Pushable behaviour](http://wyrihaximus.net/projects/cakephp/ratchet/documentation/model-push.html).

## Getting started ##

#### 1. Requirements ####

This plugin depends on the following plugins and their dependencies and are pulled in by composer later on:

- [Ratchet](https://github.com/Wyrihaximus/Ratchet)
- [RatchetCommands](https://github.com/WyriHaximus/RatchetCommands)

#### 2. Composer ####

Make sure you have [composer](http://getcomposer.org/) installed and configured with the autoloader registering during bootstrap as described [here](http://ceeram.github.io/blog/2013/02/22/using-composer-with-cakephp-2-dot-x/). Make sure you have a composer.json and add the following to your required section.

```
composer require wyrihaximus/ratchet-model-push 
```

When you've set everything up, run `composer install`.

#### 3. Setup the plugin ####

Make sure you load `RatchetModelPush` and all the plugins it depends on and their dependecies in your bootstrap and set then up properly.

#### 4. Setting up the behavior ####

The following code example attaches the Pushable behavior to the `WyriProject` model. The events do the following in order:

1. Fires on creating on a new record and binds to `WyriProject.created`.
2. Fires on updating any record and binds to `WyriProject.updated`.
3. Fires on any updating record and binds to `WyriProject.updated.{id}` where `{id}` is the `id` for that record. So if `id` is `1` it binds to `WyriProject.updated.1`.

The data is passed into the event as a 1 dimensional array.

```php
<?php

class Model extends AppModel {
    public $actsAs = array(
        'RatchetModelPush.Pushable' => array(
            'events' => array(
                array(
                    'eventName' => 'WyriProject.created',
                    'created' => true,
                ),
                array(
                    'eventName' => 'WyriProject.updated',
                    'created' => false,
                ),
                array(
                    'eventName' => 'WyriProject.updated.{id}',
                    'created' => false,
                    'fields' => true,
                ),
            ),
        ), 
    );
}
```

#### 5. Client side ####

On the client side the only thing required is subscribing to the event:

```javascript
cakeWamp.subscribe('Rachet.WampServer.ModelUpdate.WyriProject.updated.1', function(topicUri, event) {
	// Do your stuff with the data
});
```

*Note: the actual event you're subscribing to is prefixed by *`Rachet.WampServer.ModelUpdate.`*.*

Make sure to include the JavaScript file and its dependencies in your view:
```php
echo $this->Html->script(array(
    'Ratchet.when/when',
    'Ratchet.autobahn/autobahn',
    'Ratchet.cake-wamp'
));
```

## Plugin License ##

(The MIT License)

Copyright © 2012 - 2013 Cees-Jan Kiewiet

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the ‘Software’), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED ‘AS IS’, WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.
