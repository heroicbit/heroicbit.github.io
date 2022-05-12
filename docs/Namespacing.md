# Namespacing

Meskipun CodeIgniter 3 belum mendukung namespace, tapi meinCMS sudah menambahkan kode spl_autoload_register() pada file application/config/application.php sehingga kamu dapat menggunakan namespace pada kode programmu. Keuntungan menggunakan namespace adalah setidaknya kita tidak perlu pusing lagi mencari nama class yang unik karena di CodeIgniter 3 kita tidak dapat menggunakan senama class yang sama lebih dari sekali.

Ada dua scope namespace yang sudah diatur, yakni `App\` dan `Site\`. 

`App\` merujuk pada folder application/. Misalnya untuk mengakses file library dari dalam folder application/libraries, kamu dapat menggunakan namespace `App\libraries\NamaLibrary`.

Adapun `Site\` digunakan untuk mengakses folder site (yakni folder public/default/ atau public/namasite/). Misalnya bila kamu membuat module dengan nama book dan disimpan di dalam folder public/default/modules/book/, maka kamu dapat mengakses file di dalamnya dengan namespace `Site\modules\book\models\BookModel` atau `Site\modules\book\apapun\NamaFileClass`.

## Contoh Implementasi

Misalkan kita membuat modul book di dalam sites, dengan struktur seperti ini:

```
public/default/modules
└── book
	└── controllers
	│	└── Book.php
	└── models
		└── BookModel.php
```

Maka kita dapat menulis kode seperti ini:

**book/models/BookModel.php**

```php
<?php namespace Site\modules\book\models;

class BookModel extends \MY_Model {

	public $table = 'books';

}
```

**book/controllers/Book.php**

```php
<?php

use Site\modules\book\models\BookModel;

class Book extends MY_Controller {

	function __construct()
	{
		parent::__construct();
		$this->BookModel = new BookModel();
	}

	function index()
	{
		$books = $this->BookModel->get_all();
		print_r($books);
	}

}
```