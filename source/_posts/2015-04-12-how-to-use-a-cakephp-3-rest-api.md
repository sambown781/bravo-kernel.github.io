title: "How to use a CakePHP 3 REST API"
date: 2015-04-12 13:10:24
categories:
- CakePHP
tags:
- cakephp
- cakephp3
- rest
- api
---
In this follow-up post to
[How to build a CakePHP 3 REST API in minutes](/2015/04/how-to-build-a-cakephp-3-rest-api-in-minutes/)
we explore the inner workings of our new API by:

1. Adding some tasty cocktail data to our application
(already listening at ``http://cake3api.app``)
2. Accessing the cocktail resources through our API using the:
    + index action (GET)
	+ view action (GET)
	+ add action (POST)
	+ edit/update action (PUT)
	+ delete action (DELETE)
3. Configuring Pagination
4. Testing Querystring Parameters

<em>Application sources [available here](https://github.com/bravo-kernel/examples/tree/master/blog-how-to-use-a-cakephp3-rest-api).</em>

## 1. Adding The Cocktails

### Populate the database

Download this CakePHP
[database migration file](https://github.com/bravo-kernel/examples/blob/master/blog-how-to-use-a-cakephp3-rest-api/config/Migrations/20150411161731_initial.php)
to your ``config/Migrations`` directory.

Now run the following command inside your application's root directory to
create and popuplate the ``cocktails`` table:

```bash
bin/cake migrations migrate
```

### Create the controller

Create a new file named ``src/Controller/CocktailsController.php`` with the
following content:

```php
<?php
namespace App\Controller;

use App\Controller\AppController;

class CocktailsController extends AppController
{
    public $paginate = [
        'page' => 1,
        'limit' => 10,
        'maxLimit' => 100,
        'fields' => [
            'id', 'name', 'description'
        ],
        'sortWhitelist' => [
            'id', 'name', 'description'
        ]
    ];
}
```

### Create the entity and table

Generate the required entity and model by running the following command inside
your application's root directory:

```bash
bin/cake bake model Cocktails
```

### Optionally create html views

Even though **your API does not use views** to output JSON and XML you might
want to generate some html views for easy browsing the cocktails by running
the following command inside your application's root directory:

```bash
bin/cake bake template Cocktails
```

If things went well browsing to ``http://cake3api.app/cocktails`` should
display something similar to:
<br />

{% asset_img browser-cocktails-index.png 'Cocktails index' %}

## 2. Accessing API Resources

Your API supports accessing resources with or without extensions and we will
test both where applicable.

If you don't have a client capable of testing Request Headers you might
consider using:

+ the [RESTClient](https://addons.mozilla.org/nl/firefox/addon/restclient/) plugin for Firefox
+ the [Postman](https://chrome.google.com/webstore/detail/postman-rest-client/fdmmgilgnpjigdojojpjoooidkmcomcm) plugin for Chrome

> **Note:** for brevity we will only test JSON responses.

### Index action (GET)

Retrieve the first JSON page with cocktails from your API by either:
+ retrieving directly from ``http://cake3api.app/cocktails.json``
+ querying ``http://cake3api.app/cocktails`` using:
    - **HTTP Method** ``GET``
    - **Accept Header** ``application/json``

{% asset_img api-request-headers-index.png 'API Request Headers for index action' %}

Should return Status Code 200 (Success) with a JSON response body similar to:

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


### View action (GET)

Specify a record id to retrieve JSON details for a single cocktail by either:

+ retrieving directly from e.g. ``http://cake3api.app/cocktails/5.json``
+ querying ``http://cake3api.app/cocktails/5`` using:
    - **HTTP Method** ``GET``
    - **Accept Header** ``application/json``

{% asset_img api-request-headers-view.png 'API Request Headers for view action' %}

Should return Status Code 200 (Success) with a JSON response body similar to:

```json
{
    "success": true,
    "data": {
        "id": 5,
        "name": "Caipirinha",
        "description": "Rum based",
        "created": "2015-04-11T09:33:37+0000",
        "modified": null
    }
}
```

### Add action (POST)

Create a new cocktail record by posting JSON data to your API using:

+ The index URL ``http://cake3api.app/cocktails``
+ **HTTP Method** ``POST``
+ **Accept Header** ``application/json``
+ **Content-Type Header** ``application/json``
+ Body data in (absolutely) correct JSON format

{% asset_img api-request-headers-add.png 'API Request Headers for add action' %}

Should return Status Code 201 (Created) with a JSON response body containing
the **id of the created cocktail** similar to:

```json
{
    "success": true,
    "data": {
        "id": 22
    }
}
```

### Edit/Update action (PUT)

Change an existing cocktail by posting JSON data to our API using:

+ The view URL for the specific recipe, e.g. ``http://cake3api.app/cocktails/5``
+ **HTTP Method** ``PUT``
+ **Accept Header** ``application/json``
+ **Content-Type Header** ``application/json``
+ Partial or full body data in (absolutely) correct JSON format

{% asset_img api-request-headers-edit.png 'API Request Headers for edit/update action' %}

Should return Status Code 200 (Success) with a JSON response body similar to:

```json
{
    "success": true,
    "data": []
}
```

### Delete action (DELETE)

Delete an existing cocktail through your API by using:

+ The view URL for the specific cocktail, e.g. ``http://cake3api.app/cocktails/5``
+ **HTTP Method** ``DELETE``
+ **Accept Header** ``application/json``

{% asset_img api-request-headers-delete.png 'API Request Headers for delete action' %}

Should return Status Code 200 (Success) on success with a JSON response body similar to:

```json
{
    "success": true,
    "data": []
}
```

## 3. Configuring Pagination

The Paginator settings in ``src/Controller/CocktailsController.php`` are used
by your API to format the outputted JSON/XML. Let's change some of the settings
below in preparation for querystring parameter testing.

```php
    public $paginate = [
        'page' => 1,
        'limit' => 10,
        'maxLimit' => 100,
        'fields' => [
            'id', 'name', 'description'
        ],
        'sortWhitelist' => [
            'id', 'name', 'description'
        ]
    ];
```

### Show all cocktail details/fields

To show all record details **remove** the ``fields`` array (used earlier to
keep the JSON output examples readable).

Querying ``http://cake3api.app/cocktails.json?limit=2`` should now produce
more detailed JSON similar to:

```json
{
    "success": true,
    "data": [
        {
            "id": 1,
            "name": "Cosmopolitan",
            "description": "Vodka based",
            "created": "2015-04-10T15:56:23+0000",
            "modified": null
        },
        {
            "id": 2,
            "name": "Margarita",
            "description": "Tequila based",
            "created": "2015-04-10T15:59:39+0000",
            "modified": null
        }
    ],
    "pagination": {
        "page_count": 11,
        "current_page": 1,
        "has_next_page": true,
        "has_prev_page": false,
        "count": 21,
        "limit": 2
    }
}
```

### Show less cocktails per page

To show only show 5 cocktails per page change ``limit => 10`` to ``limit => 5``.

### Set maximum number of cocktails per page

To never show more than 15 cocktails change ``'maxLimit' => 100`` to
``'maxLimit' => 15``.

### Prevent sorting by description

To prevent users from sorting by the description field remove ``description``
from the ``sortWhitelist`` array.

### Default page

Do NOT change the default ``page`` used for the index resultset to prevent
frustrating further tests.

## 4. Testing Querystring Parameters

Your API comes with support for querystring parameters that clients can use
to manipulate the output produced by your API.

### Limiting results

Clients can use the ``limit`` parameter to manipulate the number of records
returned by your API.

To verify the parameter works as expected query ``http://cake3api.app/cocktails.json?limit=3``.

The result should list three cocktails instead of (the now default) five.

### Rate limiting

To verify your API is respecting the ``maxLimit`` Paginator setting
query ``http://cake3api.app/cocktails.json?limit=20``.

The result should only list 15 cocktails.

### Sorting by field

Clients can use the ``sort`` parameter to specify which field should be used
to sort the results produced by your API.

To verify the parameter is working as expected query
``http://cake3api.app/cocktails.json?sort=name``.

The results should be sorted by cocktail name.

### Sorting ascending/descending

Clients can use the ``direction`` parameter **in combination** with the ``sort``
parameter to specify the direction in which results are sorted by your API
(either ``asc`` or ``desc``).

To verify the parameter is working as expected query
``http://cake3api.app/cocktails.json?sort=name&direction=desc``.

The results should be reverse-sorted by cocktail name.

### Sort whitelist

To verify your API is respecting the ``sortWhitelist`` Paginator setting
query ``http://cake3api.app/cocktails.json?sort=description``.

The result should show the default (non-sorted) index results since we
removed ``description`` from the sortWhitelist array.

## Additional reading

+ [CakePHP 3 Pagination](http://book.cakephp.org/3.0/en/controllers/components/pagination.html)
+ [Securing your API using JWT authentication](https://github.com/ADmad/cakephp-jwt-auth)

<em>Hat tip to CakePHP Core Developers [jose_zap](https://github.com/lorenzo) and [ADmad](https://github.com/admad) for helping create this post.</em>
