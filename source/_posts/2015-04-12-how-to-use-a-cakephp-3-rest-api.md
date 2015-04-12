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
In this follow-up post to [How to build a CakePHP 3 REST API in minutes](/2015/04/how-to-build-a-cakephp-3-rest-api-in-minutes/) we explore the inner workings of our new API by:

1. Adding some tasty cocktail data to our application (already listening at ``http://cake3api.app``)
2. Learning how to access our API resources using:
    + index action (GET)
	+ view action (GET)
	+ add action (POST)
	+ edit/update action (PUT)
	+ delete action (DELETE)
3. Configuring Pagination
4. Testing Querystring Parameters

<em>Application sources available [here](https://github.com/bravo-kernel/examples/tree/master/blog-how-to-use-a-cakephp3-rest-api).</em>

## 1. Adding The Cocktails

### Populate the database

Download this CakePHP [database migration file](https://github.com/bravo-kernel/examples/blob/master/blog-how-to-use-a-cakephp3-rest-api/config/Migrations/20150411161731_initial.php) to your ``config/Migrations`` directory.

Create and popuplate the ``cocktails`` table by running the following command inside your application's root directory:

```bash
bin/cake migrations migrate
```

### Create the controller

Create new file ``src/Controller/cocktailsController.php`` with the following content:

```php
<?php
namespace App\Controller;

use App\Controller\AppController;

class CocktailsController extends AppController
{

}
```

### Create models and views

Generate some basic models and views by running the following commands inside your application's root directory:

```bash
bin/cake bake model Cocktails
bin/cake bake template Cocktails
```

### Confirm

You are ready to start testing your API if browsing to ``http://cake3api.app/cocktails`` looks similar to:
<br />

{% asset_img browser-cocktails-index.png 'Cocktails index' %}

## 2. Accessing API Resources

Your API supports accessing resources with or without the .json/.xml extensions and we will test both where applicable.

If you don't have a client capable of testing Request Headers you might consider using:

+ the [RESTClient](https://addons.mozilla.org/nl/firefox/addon/restclient/) plugin for Firefox
+ the [Postman](https://chrome.google.com/webstore/detail/postman-rest-client/fdmmgilgnpjigdojojpjoooidkmcomcm) plugin for Chrome

> **Note:** we will only be testing JSON for brevity.

### Index action (GET)

Retrieve a JSON list of cocktails from your API by either:
+ retrieving directly from ``http://cake3api.app/cocktails.json``
+ querying ``http://cake3api.app/cocktails`` using:
    - **HTTP Method** ``GET``
    - **Accept-Header** ``application/json``

Will return Status Code 200 (Success) on success.

<br />

{% asset_img api-request-headers-index.png 'API Request Headers for index action' %}

### View action (GET)

Retrieve JSON details for a single cocktail from your API by either:

+ retrieving directly from ``http://cake3api.app/cocktails/5.json``
+ querying ``http://cake3api.app/cocktails/5`` using:
    - **HTTP Method** ``GET``
    - **Accept-Header** ``application/json``

Will return Status Code 200 (Success) on success.

<br />

{% asset_img api-request-headers-view.png 'API Request Headers for view action' %}

### Add action (POST)

Add a new cocktail by posting JSON data to your API using:

+ The index URL ``http://cake3api.app/cocktails``
+ **HTTP Method** ``POST``
+ **Accept-Header** ``application/json``
+ **Content-Type** ``application/json``
+ Body data in (absolutely) correct JSON format

Will return Status Code 201 (Created) on success as well as the **id of the created cocktail** in the response body.

<br />

{% asset_img api-request-headers-add.png 'API Request Headers for add action' %}

### Edit/Update action (PUT)

Change an existing cocktail by posting JSON data to our API using:

+ The view URL for the specific recipe ``http://cake3api.app/cocktails/5``
+ **HTTP Method** ``PUT``
+ **Accept-Header** ``application/json``
+ **Content-Type** ``application/json``
+ Partial or full body data in (absolutely) correct JSON format

Will return Status Code 200 (Success) on success.

<br />

{% asset_img api-request-headers-edit.png 'API Request Headers for edit/update action' %}

### Delete action (DELETE)

Delete an existing cocktail using your API by using:

+ The view URL for the specific cocktail, e.g. ``http://cake3api.app/cocktails/5``
+ **HTTP Method** ``DELETE``
+ **Accept-Header** ``application/json``

Will return Status Code 200 (Success) on success.

<br />

{% asset_img api-request-headers-delete.png 'API Request Headers for delete action' %}

## 3. Configuring Pagination

Let's change some of the PaginatorComponent settings in ``src/Controller/AppController.php`` used by your API in preparation for querystring parameter testing.

```php
        'Paginator' => [
            'page' => 1,
            'limit' => 10,
            'maxLimit' => 100,
            'sortWhitelist' => [
                'id', 'name', 'description'
            ]
        ]
```

### Show less cocktails per page

Show only show 5 cocktails per page by changing ``limit => 10`` to ``limit => 5``.

### Set maximum number of cocktails per page

Never show more than 15 cocktails by changing ``'maxLimit' => 100`` to ``'maxLimit' => 15``.

### Prevent sorting by description

Prevent users from sorting by the description field by removing ``description`` from the ``sortWhitelist`` array.

### Default page

Do NOT change the default ``page`` used for the index resultset to prevent frustrating further tests.

## 4. Testing Querystring Parameters

Your API comes with support for querystring parameters that clients can use to manipulate the output produced by your API.

### Limiting results

Clients can use the ``limit`` parameter to manipulate the number of records returned by your API.

To verify the parameter works as expected query ``http://cake3api.app/cocktails.json?limit=3``.

The result should list three cocktails instead of (the now default) five.

### Rate limiting

To verify your API is respecting the ``maxLimit`` Paginator setting query ``http://cake3api.app/cocktails.json?limit=20``.

The result should only list 15 cocktails.

### Sorting by field

Clients can use the ``sort`` parameter to specify which field should be used to sort the results produced by your API. 

To verify the parameter is working as expected query ``http://cake3api.app/cocktails.json?sort=name``.

The results should be sorted by cocktail name.

### Sorting ascending/descending

Clients can use the ``direction`` parameter **in combination** with the ``sort`` parameter to specify the direction in which results are sorted by your API (either ``asc`` or ``desc``). 

To verify the parameter is working as expected query ``http://cake3api.app/cocktails.json?sort=name&direction=desc``.

The results should be reverse-sorted by cocktail name.

### Sort whitelist

To verify your API is respecting the ``sortWhitelist`` Paginator setting query ``http://cake3api.app/cocktails.json?sort=description``.

The result should show the default (non-sorted) index results since we removed ``description`` from the sortWhitelist array.

## Additional reading

+ [CakePHP 3 Pagination](http://book.cakephp.org/3.0/en/controllers/components/pagination.html)
+ [Securing your API using JWT authentication](https://github.com/ADmad/cakephp-jwt-auth)
