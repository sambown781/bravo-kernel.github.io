title: "How to prefix route a CAKEPHP 3 REST API"
date: 2015-04-15 09:47:38
categories:
- CakePHP
tags:
- cakephp
- cakephp3
- rest
- api
---
In this follow-up post to
[How to use a CakePHP 3 REST API](/2015/04/how-to-use-a-cakephp-3-rest-api/)
we will implement a
[Prefix Route](http://book.cakephp.org/3.0/en/development/routing.html#prefix-routing)
so we can benefit of clean controller logic separation and limit access
to our API resources through (only) ``/api`` prefixed URLs.

## Before We Begin

This is part three of the CakePHP 3 REST API tutorial series:

1. [How to build a CakePHP 3 REST API in minutes](/2015/04/how-to-build-a-cakephp-3-rest-api-in-minutes/)
2. [How to use a CAKEPHP 3 REST API](/2015/04/how-to-use-a-cakephp-3-rest-api/)
3. How to prefix route a CAKEPHP 3 REST API
4. How to add JWT Authentication to a CAKEPHP 3 REST API (**coming soon**)

Before starting this tutorial either:

+ complete the previous posts
+ start fresh by using these end-state [application sources](http://todo.com) and running the
database migration

## Creating the Prefix Route

### 1. Remove previous routing

Remove the general API routing created in the previous tutorial by removing these lines from your
``config/routes.php`` file:

```php
Router::extensions(['json', 'xml']);
```

```php
$routes->resources('Cocktails');
```

### 2. Create the route

Now create a new ``/api`` prefixed route we will use for all of our API resources by adding the
following code directly above the / scope definition in your ``config/routes.php`` file:

```php
Router::prefix('api', function ($routes) {
    $routes->extensions(['json', 'xml']);
    $routes->resources('Cocktails');
});
```

Please note that once again:

+ enabling extensions is optional
+ only explicitely specified controllers will be accessible through your API

### 3. Create the controller(s)

One of the great things about CakePHP 3 prefix routing is the automatic mapping to sub-namespaces 
which allows us to separate concern by creating dedicated controller files for (just) our API logic
inside the ``src/Controller/Api`` subdirectory.

Create a new controller file named ``src/Controller/Api/CocktailsController.php`` with the
same content we used in the previous tutorial but this time using a different namespace matching
our prefix route:

```php
<?php
namespace App\Controller\Api;

use App\Controller\AppController;

class CocktailsController extends AppController
{
    public $paginate = [
        'page' => 1,
        'limit' => 5,
        'maxLimit' => 15,
        'sortWhitelist' => [
            'id', 'name'
        ]
    ];
}
```

## Testing the Routes

### 1. Test the prefix

If things went well you should be able to query the prefix URL 
``http://cake3api.app/api/cocktails.json`` and be presented with the now familiar JSON output:

```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "name": "Cosmopolitan",
            "description": "Vodka based"
        },
        {
            "id": 2,
            "name": "Margarita",
            "description": "Tequila based"
        },
        {
            "id": 3,
            "name": "Mojito",
            "description": "Rum based"
        }
    ],
    "pagination": {
        "page_count": 5,
        "current_page": 1,
        "has_next_page": true,
        "has_prev_page": false,
        "count": 21,
        "limit": null
    }
}
```

<em>See the [previous tutorial](/2015/04/how-to-use-a-cakephp-3-rest-api/) for more usage examples.</em>

### 2. Test the default route

To verify your API resources are no longer accessible through the general routes
query ``http://cake3api.app/cocktails.json`` as used in the previous tutorial.
If things went well it should:

+ no longer produce a JSON/XML response
+ respond with an HTML ``MissingController`` exception instead



