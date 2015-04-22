title: "How to build a CakePHP 3 REST API in minutes"
date: 2015-04-12 11:22:21
categories:
- CakePHP
tags:
- cakephp
- cakephp3
- rest
- api
---
Step-by-step instructions for creating a full featured RESTful API using CakePHP 3.

## Before We Begin

This is part one of the CakePHP 3 REST API tutorial series:

1. How to build a CakePHP 3 REST API in minutes
2. [How to use a CakePHP 3 REST API](/2015/04/how-to-use-a-cakephp-3-rest-api/)
3. [How to prefix route a CakePHP 3 REST API](/2015/04/how-to-prefix-route-a-cakephp-3-rest-api/)
4. How to add JWT Authentication to a CakePHP 3 REST API (**coming soon**)


## 1. Create The Application

For this tutorial (and the follow-up posts) we will create a fresh CakePHP 3
application for our API:

+ named ``cake3api.app``
+ connected to a database named ``cake3api_app``
+ exposed using virtualhost ``http://cake3api.app``

If you need help with the installation either:

+ follow the detailed installation instructions of
[the CakePHP Blog Tutorial](http://book.cakephp.org/3.0/en/tutorials-and-examples/blog/blog.html#blog-tutorial)
+ install [cakebox](https://github.com/alt3/cakebox) and run
``cakebox application add cake3api.app`` to set up the application, database
and virtual host

## 2. Add the CRUD plugin

Add the [CRUD plugin](https://github.com/FriendsOfCake/crud) to your
application so your API will benefit of additional functionality like
pagination, thin controllers and DRY best practices.

Run the following command inside your application's root directory to
composer install the plugin:

```bash
composer require friendsofcake/crud:dev-cake3
```

Now run the following command to make your application use the plugin:

```php
bin/cake plugin load Crud
```

## 3. Enable the API

### a) Expose one or more controllers

Only controllers **explicitly enabled** for API use will be accessible
through your API.

To prepare for the follow-up tutorial we will enable the ``CocktailsController`` by using the
``resources()`` method inside the / scope definition of your ``config/routes.php`` file like this:

```php
Router::scope('/', function ($routes) {
	$routes->resources('Cocktails');
...
}
```

### b) Configure the API

Make the default API configuration available to all controllers in your
application by replacing the content of your ``src/Controller/AppController.php``
file with:

```php
<?php
namespace App\Controller;

use Cake\Controller\Controller;

class AppController extends Controller {

    use \Crud\Controller\ControllerTrait;

    public $components = [
        'RequestHandler',
        'Crud.Crud' => [
            'actions' => [
                'Crud.Index',
                'Crud.View',
                'Crud.Add',
                'Crud.Edit',
                'Crud.Delete'
            ],
            'listeners' => [
                'Crud.Api',
                'Crud.ApiPagination',
                'Crud.ApiQueryLog'
            ]
        ]
    ];
}
```

### c) Optionally enable extensions

Exposing your API resources **requires no additional configuration**, they are
already fully accessible using Request Headers.

However... we will assume you want to additionally allow access to your API resources using
the [.json and .xml extensions](http://book.cakephp.org/3.0/en/development/routing.html#routing-file-extensions)
so open your ``config/routes.php`` file and add the following line directly above
the / scope definition:

```php
Router::extensions(['json', 'xml']);
```

## Stop The Clock!

That's all, you should now be able to browse to ``http://cake3api.app/index.json``
and be presented with your API's first JSON (error) response looking like this:

```json
{
    "message": "Controller class Index could not be found.",
    "url": "\/index.json",
    "code": 404
}
```

Quite impressive but in all fairness... even though you now have a (very) cool
API that only took minutes to create it is still pretty useless without any
meaningful data to serve so this might be a good moment to dive straight into
the follow-up tutorial:

> [How to use a CakePHP3 REST API](/2015/04/how-to-use-a-cakephp-3-rest-api/)

## Additional reading

+ Follow-up tutorial "[How to use a CakePHP3 REST API](/2015/04/how-to-use-a-cakephp-3-rest-api/)"
+ [Tutorial sources](https://github.com/bravo-kernel/application-examples/tree/master/blog-how-to-build-a-cakephp3-rest-api)
+ [FriendsOfCake CRUD plugin documentation](http://crud.readthedocs.org)

<em>Hat tip to CakePHP Core Developers [jose_zap](https://github.com/lorenzo)
and [ADmad](https://github.com/admad) for helping create this post.</em>
