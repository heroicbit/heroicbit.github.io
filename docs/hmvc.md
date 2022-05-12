# HMVC

HMVC singkatan dari Hierarchical Model View Controller. Bila pada CodeIgniter 3 kita menuliskan kode controller di folder controllers, dan begitu juga dengan model dan view, pada HMVC kita menghimpun folder controllers, models dan views dalam satu folder yang dinamakan module. Boleh dibilang module adalah pengemasan MVC berdasarkan fitur.

```
modules/
└── mymodule/
    ├── configs/
    ├── controllers/
    ├── models/
    ├── views/
    .
    .
```

DI HeroicBit, folder module disimpan di beberapa tempat, diantaranya di `application/modules/` dan di `sites/[namasite]/modules/`.

Namun demikian, HeroicBit tidak membaca otomatis module yang ada di folder tersebut sebagaimana bawaan dari library HMVC punya WireDesign. HeroicBit mengharuskan kita untuk mendaftarkan path dari setiap module yang akan digunakan ke dalam file `application/config/application.php`.

```php
$config['module_paths'] = [
    // Core modules
    'post'         => [APPPATH.'modules/post/', '../modules/post/', true],
    'user'         => [APPPATH.'modules/user/', '../modules/user/', true],
    'media'        => [APPPATH.'modules/media/', '../modules/media/'],
    ...
];
```

Dengan mendaftarkan path module seperti ini, module kita tidak akan otomatis terdeteksi sistem, tapi dengan begitu sistem menjadi lebih cepat karena tidak mesti men-scan folder module seperti sebelumnya.

Anda juga dapat menambah atau meng-override penggunaan module di level site dengan mendaftarkan module path pada file `sites/[namasite]/configs/application.php`.