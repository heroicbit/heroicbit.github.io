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

'DUMPDB_ENABLE': true atau false, defaultnya false. Set ke true agar Dumpdb dapat dijalankan.

'DUMPDB_PATH': Lokasi penyimpanan backup, defaultnya disimpan di sites/[namasite]/resources/backup. 

'DUMPDB_AMOUNT': Jumlah backup yang dipertahankan. File backup yang lebih lama dari jumlah yang ditentukan akan otomatis dihapus untuk menjaga efisiensi storage.

## Backup ke Server Git

Server git seperti Github dan Gitlab dapat dijadikan opsi penyimpanan backup data. Dumpdb telah menyediakan kemampuan untuk push file backup database ke server git. Ada beberapa hal yang harus diatur:

1. Pastikan kamu sudah menginstall git di server
2. Pastikan konfigurasi Dumpdb sudah diset di environment variable.
3. Jalankan perintah berikut agar www-data juga dapat menjalankan perintah git dan push ke server
   ```
   sudo -u www-data git config --global user.name "John Doe"
   sudo -u www-data git config --global user.email "johndoe@gmail.com"
   ``` 
   Pastikan folder /var/www chmod 775 dan chown :www-data
4. Jalankan command untuk backup pertama kali menggunakan terminal dari folder heroicbit:
   ```
   php heroic site/dumpdb -d namadomain.com
   ```
5. Pastikan folder penyimpanan backup dapat ditulisi oleh user `www-data` karena user inilah yang akan menjalankan perintah PHP.
   ```
   sudo chown :www-data ~/backupdb/ -R
   sudo chmod 775 ~/backupdb/ -R
   ```
6. Daftarkan ssh pubkey atas nama user www-data ke server git
   ```
   sudo -u www-data ssh-keygen -t ed25519 -C "johndoe@gmail.com"
   sudo -u www-data cat /var/www/.ssh/id_ed25519.pub
   ```
7. Daftarkan perintah berikut ini untuk menjalankan proses backup secara berkala menggunakan cron. Pada contoh di bawah ini command dijalankan setiap 2 jam sekali
   ```
   sudo -u www-data crontab -e

   0 */2 * * * cd /var/www/heroicbit && php heroic site/dumpdb -d namadomain.com && php heroic site/dumpdb/pushToGitServer -d namadomain.com >/dev/null 2>&1
   ```