# Puller

HeroicBit hadir dengan satu tool bernama Puller, tool sederhana tapi sangat bermanfaat untuk proses deployment aplikasi ke server. Bila kamu mengembangkan aplikasi HeroicBit dengan memanfaatkan version control Git dan Github, kamu dapat melakukan proses deploy dengan mudah setiap kali melakukan push ke Github.

## Instalasi

File buat pullnya adalah Puller.sh yang ada di dalam folder application/cli/. File ini nanti didaftarkan di cron. Contoh kode cronnya:

```
* * * * * cd /var/www/heroicbit/application/modules/puller && ./Puller.sh
```

## Cara Pakai

Di halaman admin Settings > Puller aktifkan fitur ini. Kemudian pada kolom Repository to Pull, daftarkan repositori yang ingin di-autopull. Format penulisannya adalah:

`namaproject /absolute/path/ke/reponya branch`

Misalnya:

```
self /var/www/heroicbit main
news /var/www/heroicbit/sites/news main
sekolah /var/www/mysekolah.com main
```

Kamu dapat mendaftarkan lebih dari satu repository.
**Catatan:** Fitur puller ini hanya tersedia di site default saja.

Untuk menjalankan pull, tinggal panggil endpoint seperti ini:

websitesaya.com/puller/run/self atau websitesaya.com/puller/run/news

Setiap kali endpoint ini dipanggil, HeroicBit akan membuat file dengan nama proectnya di dalam folder sites/default/resources/pullthis/ yang akan menjadi penanda supaya si puller ngepull repo tersebut. Si puller hanya ngepull branch yang sudah didaftarkan di settingan list repository tadi.

Endpoint di atas juga bisa kamu daftarkan di Github webhook supaya setiap kali kita ngepush ke repo, Github akan otomatis me-request endpoint tersebut.

## Menggunakan Signature di Github Webhook

Biar lebih aman lagi, kita bisa mendaftarkan secret pada saat membuat webhook di Github.

Tinggal masukkan kode secret yang kamu inginkan di kolom Secret pada saat membuat webhook di Github. Kemudian di halaman admin Setting Puller isikan juga kode secret yang sama di kolom secret. Ini tujuannya supaya si puller memverifikasi terlebih dahulu apakah request untuk ngepull ini memang datang dari Github. Kalo signaturenya beda, tandanya endpoint dipanggil bukan oleh Github webhook.