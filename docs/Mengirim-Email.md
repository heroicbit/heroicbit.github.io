# Mengirim Email

Anda dapat mengirim email menggunakan library email bawaan CodeIgniter. Namun, bila ingin mendapatkan manfaat kemudahan pengiriman email yang sudah disediakan heroicBit, Anda dapat memanfaatkan cara berikut ini.

```php
$this->load->helper('email');
sendEmail(
	['toha.samba@gmail.com', 'Toni Haryanto'],
	"Tiket Event Pembinaan",
	[
		'title' => 'Pelatihan Kepenulisan Bersama Pakar Penulis Nasional',
		'name' => 'Toni Haryanto',
		'ticketcode' => '233391TEST345',
		'start' => '2022-05-15 12:00:00',
		'event_type' => 'online',
		'location' => 'Daring',
	],
	'sample'
);
```

Cukup dengan memanggil fungsi sendEmail() yang telah ditingkatkan oleh HeroicBit, dengan format:

```
sendEmail(array $to, string $subject, array $data, string $template);
```

Parameter `$to` boleh berisi string email tujuan saja.
Parameter `$subject` adalah string untuk judul email.
Parameter `$data` adalah array data yang ingin diparsing ke dalam template email. 
Parameter `$template` adalah string nama template email yang akan digunakan. Template email disimpan di dalam folder `SITEPATH.'resources/email_templates/[nama_template].php'`.

## Pengaturan SMTP

Anda dapat mengatur konfigurasi pengiriman email di halaman admin Settings > tab Emailer.
Pastikan opsi _Use Mailcatcher for sandboxing in local_ diset ke No untuk mengirim dengan SMTP asli.

## Mengirim Email Menggunakan Queue

Bila Anda telah menginstall [sistem Queue](Queue.md) dari HeroicBit, Anda dapat menyerahkan tugas pengiriman email ke sistem queue sehingga user tidak perlu menunggu email selesai terkirim. Anda cukup mengaktifkan opsi _Send Email by Queue_ di halaman Settings > Emailer.

## Sandboxing

Seringkali kita perlu mengetes pengiriman email misalnya untuk penyesuaian template, memastikan email tampil dan terkirim dengan baik. Anda dapat mengaktifkan opsi Setting _Use Mailcatcher for sandboxing in local_ ke Yes agar email dikirim ke SMTP di komputer lokalmu.

Untuk menangkap email di komputer lokal, Anda dapat menggunakan aplikasi seperti [MailCatcher](https://mailcatcher.me/) atau [MailHog](https://github.com/mailhog/MailHog).

### Install MailHog

Saya menyarankan untuk menggunakan MailHog karena proses installnya lebih mudah.

Untuk Windows, Anda tinggal [mengunduh](https://github.com/mailhog/MailHog/releases) dan menginstall file .exe dari MailHog dan ia langsung aktif.

Untuk Ubuntu, jalankan perintah berikut untuk proses install:

```
sudo apt install golang-go -y
go get github.com/mailhog/MailHog
sudo ln -s ~/gocode/bin/MailHog /usr/local/bin/mailhog
```

Lalu jalankan dengan memanggil command:
```
mailhog
```