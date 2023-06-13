# Dumpdb

HeroicBit menyediakan fitur untuk dump database MySQL/MariaDB. Pada dasarnya kita dapat dump database langsung menggunakan mysqldump bawaan MySQL atau MariaDB. Tapi fitur Dumpdb ini dapat membackup semua database dan didaftarkan di cronjob agar proses backup database dilakukan secara berkala.

## Command

Jalankan perintah di bawah ini dari root folder heroicbit dan database pun akan didump ke dalam folder sites/[namasite]/resources/backup/[tanggal-backup]/namadb.sql.gz.

```
php heroic site/dumpdb -d namadomain.test
```

Untuk membackup semua database yang ada di MySQL atau MariaDB, gunakan perintah ini:

```
php heroic site/dumpdb/all -d namadomain.test
````

## Folder Backup

Secara default folder backup akan disimpan di sites/[namasite]/resources/backup. Untuk mengganti lokasi penyimpanan backup, kamu dapat mengaturnya di environment variable 'DBDUMP_PATH'.