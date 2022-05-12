# Membuat Module

Membuat fitur baru di HeroicBit relatif mudah bila kamu sudah pernah menggunakan Framework CodeIgniter. HeroicBit menggunakan pendekatan Hierarchical MVC atau HMVC dalam mengatur fiturnya. Artinya setiap fitur disimpan di dalam sebuah module yang di dalamnya terdapat controllers, models dan views. Module disimpan di dalam folder modules/ dengan struktur seperti ini:

```folder
nama_module/
├── config/
├── controllers/
├── models/
├── views/
├── helpers/
├── libraries/
├── migrations/
├── seeds/
├── module.yml
├── Events.php
└── Shortcodes.php
```

Tidak semua folder di atas mesti ada di dalam sebuah module. Yang wajib ada adalah controllers/ dan file module.yml. Module dapat disimpan dimana saja, tapi umumnya sudah disediakan tempat diantaranya di dalam folder application/modules/ dan src/modules/.

Untuk dapat menggunakan module dalam aplikasi, kita perlu mendaftarkan dahulu path dari module tersebut di dalam file application/config/application.php

```php
//------------------------------------------------------------------------------
// Module Paths
//------------------------------------------------------------------------------
// Register modules used in your application here
//
$config['module_paths'] = [
    'page'         => [APPPATH.'modules/page/', '../modules/page/'],
    'post'         => [APPPATH.'modules/post/', '../modules/post/'],
    'user'         => [APPPATH.'modules/user/', '../modules/user/'],
    'files'        => [APPPATH.'modules/files/', '../modules/files/'],
    'entry'        => [APPPATH.'modules/entry/', '../modules/entry/'],
    'setting'      => [APPPATH.'modules/setting/', '../modules/setting/'],
    'variable'     => [APPPATH.'modules/variable/', '../modules/variable/'],
    'dashboard'    => [SITEPATH.'modules/dashboard/', '../modules/dashboard/'],
    'development'  => [APPPATH.'modules/development/', '../modules/development/'],
];

```

Struktur array untuk mendaftarkan konfigurasi path module seperti ini:

`'nama_module'  => ['absolute/path/ke/module/', 'relative/path/dari/folder/config'],`

Misalkan, kalau folder modulenya disimpan di dalam **application/modules/nama_module/** maka kita daftarkan seperti ini:

`'nama_module'  => [APPPATH.'modules/nama_module/', '../modules/nama_module/'],`

dan bila disimpan di dalam folder **src/modules/nama_module/** maka daftarkan seperti ini:

`'nama_module'  => [SITEPATH.'modules/nama_module/', '../../src/modules/nama_module/'],`

Gunakan konstanta `APPPATH` untuk merujuk ke path folder application/ dan `SITEPATH` untuk merujuk ke path folder src/.



## Tutorial Singkat

Supaya lebih kebayang, kita akan belajar membuat satu module sederhana untuk menampilkan tulisan "Hello world" yah.

### Setup Dasar

Pertama-tama buat sebuah folder dengan nama **hello_world/** di dalam folder src/modules/.

Sebelum melanjutkan menuliskan program, daftarkan terlebih dahulu module ini ke dalam konfigurasi di dalam file application/config/application.php

```php
$config['module_paths'] = [
    ...
    'hello_world' => [SITEPATH.'modules/hello_world/', '../../src/modules/hello_world/'],
 ];
```

Kemudian buat file di dalam folder module dengan nama **module.yml** dan isi seperti ini:

```yaml
name: Hello World
icon: heart
description: Module sederhana menampilkan tulisan Hello World
author: Nama Kamu
```

Buat satu folder dengan nama **controllers/** dan buat file Hello_world.php di dalam folder controllers/. Ini dengan kode berikut:

```php
<?php

class Hello_world extends Frontend_Controller 
{
	public function index()
	{
		echo "Hello World!";
	}
}
```

Dari sini kita sudah bisa melihat hasilnya dengan membuka di browser dengan URL http://vissi.test/hello_world.

### Menggunakan Template Views

Buat folder dengan nama **views/** dan buat satu file dengan nama index.php di dalamnya. Ini dengan kode berikut:

```php
<h1>Hello world again</h1>
```

Perbaharui method `index()` di dalam controller menjadi seperti ini:

```php
public function index()
{
	$this->load->render('index');
}
```

Buka lagi di browser http://vissi.test/hello_world dan sekarang tulisan "Hello world"-nya sudah pakai yang dari view.

Disini kita membuat controller dengan extends dari class `Frontend_Controller`. Khusus untuk frontend kita menggunakan template engine Latte di dalam viewnya dan menggunakan ekstensi file .html untuk file viewnya. Kita juga menggunakan method `$this->load->render()` untuk memuat file view.

Kita akan mencoba passing data untuk dirender di dalam view. Edit kode pada method index() di controller seperti ini:

```php
public function index()
{
    $data['name'] = "Toni Haryanto";
	$this->load->render('index', $data);
}
```

Lalu edit file viewnya:

```php
<h1>Hello world again, {$name}!</h1>
```

Begitu caranya untuk passing data ke view.

Latte merupakan template engine yang terbilang sangat mudah untuk digunakan. Sederhananya seolah-olah kita hanya mengubah kode buka-tutup PHP dengan tanda kurung kurawal, `<?= $name; ?>` menjadi `{$name}`. Lebih lengkap tentang Latte template engine bisa dilihat di halaman dokumentasinya disini: https://latte.nette.org/en/

### Membuat Halaman Admin

Setiap module boleh membuat halaman adminnya sendiri. HeroicBit sudah menyediakan halaman dashboard admin yang terintegrasi dengan sistem dan fitur-fitur dasar sehingga kita tidak perlu lagi membuat halaman admin sendiri.

Pada module hello_world/ buat folder baru di dalam folder controller/ dengan nama admin/. Kemudian buat file controller di dalamnya dengan nama Hello_world.php. Isi dengan kode berikut:

```php
<?php

class Hello_world extends Backend_Controller 
{
	public function index()
	{
        $data['name'] = "Toni Haryanto";
		$this->view('admin/index', $data);
	}
}
```

Halaman admin menggunakan controller di dalam folder admin/ dengan extends ke class Backend_Controller. Load view pun menggunakan method `$this->view()` dan file viewnya masih menggunakan ekstensi .php.

Buat file **index.php** di dalam folder views/admin/ lalu isi dengan kode berikut:

```php
<div class="mb-5">
    <div class="row">
        <div class="col-12">
            <h2>Hello World</h2>
        </div>
	</div>
</div>

<div class="card">
	<div class="card-block">
		<p class="lead">Hello world from dashboard, <?= $name; ?></p>
	</div>
</div>
```

Ada dua elemen utama di view admin ini, bagian pertama untuk header halaman dan bagian kedua untuk konten halaman.