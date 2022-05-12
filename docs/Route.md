# HeroicBit Routing

HeroicBit menggunakan library routing bawaan Bonfire sehingga mekanisme routing di CodeIgniter menjadi lebih canggih daripada bawaannya. Dokumentasi terkait penggunaan custom routing dapat dilihat di dokumentasi [Bonfire Route](http://cibonfire.com/docs/developer/routes).

Konfigurasi route untuk sistem core HeroicBit ditulis di dalam file application/config/routes.php seperti bawaan CodeIgniter. Untuk menulis route baru, sebaiknya lakukan di dalam file `application/routes/` atau `sites/[namasite]/routes/`.

Di dalam dua folder tersebut terdapat dua buah file, yakni `web.php` untuk route website dan `api.php` untuk route [API](REST-API.md).