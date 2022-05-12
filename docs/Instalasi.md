# Instalasi

- Clone project
- Jalankan perintah `composer install` di root project
- Pointing vhost ke folder public/
- Salin file \_domain/domain.name ke file baru dengan nama \_domain/namadomainkamu.test
- Buat database di MySQL
- Set konfigurasi di dalam file \_domain/namadomainkamu.test
- Jalankan perintah `php cli install -d namadomainkamu.test` untuk menginstal site dan migrasi database
- Buka web http://namadomainkamu.test/admin dengan akun email admin@admin.com dan password 12345
- Set folder sites/namasite/resources agar dapat ditulisi (writable)
- Set folder public/uploads/namasite/sources dan public/uploads/namasite/thumbs agar dapat ditulisi (writable). Buat dulu foldernya bila belum ada
- Set symlink dari folder sites/namasite/theme ke public/views/namasite untuk meng-override tampilan
