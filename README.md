# Lumen PHP Framework REST API

## Vorbereitung

Zunächst erstellen wir ein neues Lumen-Projekt mittels Composer.

Folgender Befehl erstellt ein lauffähiges Projekt mit allen Abhängigkeiten im Ordner lumen-api.

```php composer.phar create-project --prefer-dist laravel/lumen Lumen-Test```

Lokal kann man jetzt den Build-In Server von PHP nutzen und folgen Aufruf starten.

```php -S localhost:8000 -t public```

Ruft man nun localhost:8000 im Browser auf, sieht man eine Seite mit folgender Ausgabe.

```Lumen () (Laravel Components)```

## Route hinzufügen

Wir beginnen nun, erste Routen hinzuzufügen. Dazu öffnen wir die Datei web.php im Verzeichnis routes und fügen folgendes hinzu.

```
$router->group(['prefix'=>'api'], function() use($router){
    $router->get('/items', 'ItemController@all');
    $router->get('/items/{id}', 'ItemController@get');
});
```

Die erste Route soll alle Items zurückgeben, die zweite Route genau eins per Id.

## Controller implementieren

Als nächstes implementieren wir den in der Route angegebenen ItemContoller mit den benötigten Methoden. Dazu erstellen wir eine neue Datei ItemController.php unter app/Http/Controllers.
Im Konstruktor bauen wir ein Array für die Beispieldaten, die in den Methoden all und get verwendet werden.
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

## Aufruf der API

Voraussetzung mit Linux! Installiere bitte den Postman!
`GET http://localhost:8000/api/items/`

![lumen-test](https://user-images.githubusercontent.com/70861205/118483446-4df50980-b716-11eb-950f-10afd271a1db.png)

