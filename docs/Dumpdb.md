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

## Konfigurasi

Konfigurasi Dumbdb diatur di dalam file env site, diantaranya:

'DBDUMP_PATH': Lokasi penyimpanan backup, defaultnya disimpan di sites/[namasite]/resources/backup. 

'DBDUMP_AMOUNT': Jumlah backup yang dipertahankan. File backup yang lebih lama dari jumlah yang ditentukan akan otomatis dihapus untuk menjaga efisiensi storage.

## Backup ke Server Git

Server git seperti Github dan Gitlab dapat dijadikan opsi penyimpanan backup data. Dumpdb telah menyediakan kemampuan untuk push file backup database ke server git. Ada beberapa hal yang harus diatur:

1. Pastikan kamu sudah menginstall git di server
2. Jalankan perintah berikut agar www-data juga dapat menjalankan perintah git dan push ke server
   ```
   sudo -u www-data git config --global user.name "Toni Haryanto"
   sudo -u www-data git config --global user.email "toha.samba@gmail.com"
   ``` 
3. Setelah mengatur folder tempat menyimpan backup, pastikan folder tersebut dapat ditulisi oleh user `www-data` karena user inilah yang akan menjalankan perintah PHP.
   ```
   sudo chown :www-data ~/backupdb/ -R
   sudo chmod 775 ~/backupdb/ -R
   ```
4. Daftarkan ssh pubkey atas nama user www-data ke server git
   ```
   ssh-keygen -t ed25519 -C "toha.samba@gmail.com"
   sudo -u www-data cat /var/www/.ssh/id_ed25519.pub
   ```
5. Daftarkan perintah berikut ini untuk menjalankan proses backup secara berkala menggunakan cron. Pada contoh di bawah ini command dijalankan setiap 2 jam sekali
   ```
   sudo -u www-data crontab -e

   * */2 * * * cd /var/www/persis67benda.com && php heroic site/dumpdb -d www.persis67benda.com && php heroic site/dumpdb/pushToGitServer -d www.persis67benda.com >/dev/null 2>&1
   ```