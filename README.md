# Lumen PHP Framework REST API

## Preparation

First, we create a new Lumen project using [Composer](https://getcomposer.org/).

The following command creates a runnable project with all dependencies in the Lumen Test folder.

```php composer.phar create-project --prefer-dist laravel/lumen Lumen-Test```

Locally you can now use the build-in server of PHP and start the following call.

```php -S localhost:8000 -t public```

If you now call localhost:8000 in the browser, you will see a page with the following output.
```Lumen () (Laravel Components)```

## Add route

We now start to add the first routes. To do this, we open the web.php file in the routes directory and add the following.

```
$router->group(['prefix'=>'api'], function() use($router){
    $router->get('/items', 'ItemController@all');
    $router->get('/items/{id}', 'ItemController@get');
});
```

The first route should return all items, the second route exactly one per id.


## Implement controller

Next, we implement the ItemContoller specified in the route with the required methods. For this we create a new file ItemController.php under app/Http/Controllers.
In the constructor, we build an array for the sample data that will be used in the all and get methods.

```<?php

namespace App\Http\Controllers;
use Illuminate\Http\Request;
 
class ItemController extends Controller
{
    private $items;
 
    public function __construct() {
        $this->items = array();
        for($i = 0; $i<10; $i++) {
            $item = array(
                'id' => $i,
                'name' => "item-" . $i
            );
            $this->items[] = $item;
        }
         
    }
 
    public function all()
    {
        return response()->json($this->items);
    }
     
    public function get($id)
    {
        $found_key = array_search($id, array_column($this->items, 'id'));
        return response()->json($this->items[$found_key]);
    }
}
```

## Calling the API

Tested with Postman! Can of course also be called via the browser.

`GET http://localhost:8000/api/items/`

![lumen-test](https://user-images.githubusercontent.com/70861205/118483446-4df50980-b716-11eb-950f-10afd271a1db.png)

