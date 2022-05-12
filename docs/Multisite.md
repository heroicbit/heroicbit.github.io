# Multisite

HeroicBit hadir dengan fitur multisite, dimana dalam satu instalasi sistem kita dapat membuat lebih dari satu website. Ada 2 folder utama yang berperan dalam multisite ini, diantaranya folder `_domain/` dan `sites/`.

Saat pertama menginstall, sistem akan menggunakan site default yang mana konfigurasinya ada di dalam folder `sites/default/`. Berikut ini struktur lengkap dari isi folder site:

```
sites/
└── default/
    ├── configs/
    ├── entries/
    ├── modules/
    ├── resources/
    ├── routes/
    ├── settings/
    ├── shortcodes/
    └── theme/
```
**Penjelasan**:
- Folder `configs/` berisi file konfigurasi di level site
- Folder `entries/` berisi file konfigurasi untuk membuat CRUD menggunakan [Entry](Membuat-Entry.md)
- Folder `modules/` berisi [module](Membuat-Modul.md) yang hanya digunakan oleh site tersebut
- Folder `resources/` berisi beragam file yang diperlukan oleh module dan site tersebut 
- Folder `routes/` berisi routing khusus site dan juga untuk meng-override [route](Route.md) di level core
- Folder `settings/` berisi file konfigurasi [setting](Setting.md) khusus untuk site tersebut
- Folder `shortcode/` berisi file konfigurasi [shortcode](Shortcode.md) khusus untuk site tersebut
- Folder `theme/` berisi file theme untuk [meng-override theme](Membuat-Tema.md) utama