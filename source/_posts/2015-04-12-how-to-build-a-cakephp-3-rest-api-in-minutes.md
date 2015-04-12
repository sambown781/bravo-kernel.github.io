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

## 1. Create The Application

For this tutorial (and the follow-up posts) we will create a fresh CakePHP 3 application for our API:

+ named ``cake3api.app``
+ connected to a database named ``cake3api_app``
+ exposed using virtualhost ``http://cake3api.app``

If you need help with the installation either:

+ follow the detailed installation instructions of [the CakePHP Blog Tutorial](http://book.cakephp.org/3.0/en/tutorials-and-examples/blog/blog.html#blog-tutorial)
+ install [cakebox](https://github.com/alt3/cakebox) and run ``cakebox application add cake3api.app`` to set up the application, database and virtual host

## 2. Update Composer

Use Composer to add the [CRUD plugin](https://github.com/FriendsOfCake/crud) to your application so your API will benefit of additional functionality like pagination, thin controllers and DRY best practices.

### Automatic Installation

To install the plugin automatically run the following command inside your application's root directory:

```bash
composer require friendsofcake/crud:dev-cake3
```

### Manual installation

To install the plugin manually, first extend your composer.json file with:

```json
{
        "require" : {
                "FriendsOfCake/crud": "dev-cake3"
        }
}
```

Then run the following command inside your application's root directory:

```bash
composer update
```

## 3. Enable the API

### a) Load the CRUD plugin

Make sure your application can use the CRUD plugin by adding the following line **directly below** ``Plugin::load('Migrations');`` in your ``config/bootstrap.php`` file:

```php
Plugin::load('Crud');
```

### b) Enable extensions

Allow access to your API resources using the .json and .xml extensions by adding the following line **directly above** the / scope definition in your ``config/routes.php`` file:

```php
Router::extensions(['json', 'xml']);

```

### c) Enable Request Headers

Allow access to your API resources using Request Headers by adding the following line as the **first line inside** the / scope definition in your ``config/routes.php`` file:

```php
$routes->resources('Cocktails');
```

> **TODO:** Cocktails needs to be replaced with a filesystem foreach loop going over all files in src/Controller here ... IMO also the big weak point. Feedback needed!

### d) Enable the API

Allow API access to all controllers in your application by replacing the content of your ``src/Controller/AppController.php`` file with:

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
        ],
        'Paginator' => [
            'page' => 1,
            'limit' => 10,
            'maxLimit' => 100,
            'sortWhitelist' => [
                'id', 'name', 'description'
            ]
        ]
    ];
}
```

## Stop The Clock!

That's all, you should now be able to browse to ``http://cake3api.app/index.json`` and be presented with your API's first JSON (error) response.

Quite impressive but in all fairness... even though you now have a (very) cool API that only took minutes to create it is still pretty useless without any meaninful data to serve so this might be a good moment to dive straight into this follow-up post:

> [How to use a CakePHP3 REST API](/2015/04/how-to-use-a-cakephp-3-rest-api/)

## Additional reading

+ [CRUD plugin documentation](http://crud.readthedocs.org)
+ [Securing your CakePHP 3 API using JWT authentication](https://github.com/ADmad/cakephp-jwt-auth)
+ [Limiting API access to an /api prefixed route](http://book.cakephp.org/3.0/en/development/routing.html#prefix-routing)
