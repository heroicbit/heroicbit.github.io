# Menggunakan Antrian

Adakalanya kita ingin mengeksekusi suatu task tapi tidak ingin membebani runtime sehingga membuat pengguna harus menunggu lama. Untuk itu kita bisa menggunakan sistem antrian (queue) yang disediakan oleh HeroicBit. Sistem ini adalah sistem antrian sederhana memanfaatkan queue engine bernama Beanstalk.

## Setup Queue

Untuk menggunakan fasilitas queue, kita menggunakan Beanstalk sebagai queue engine. Untuk itu install terlebih dahulu beanstalkd di server.

```
sudo apt install beanstalkd
```

Untuk memantau dan memastikan beanstalk berjalan dengan baik, kita akan perlu menginstal juga aplikasi untuk memantaunya. Unduh aplikasi beanstalk admin versi web di https://github.com/ptrofimov/beanstalk_console atau versi terminalnya di https://github.com/src-d/beanstool.

## Membuat Worker

Buat file PHP dengan nama [Nama]Worker.php di dalam folder application/cli/queueWorker/. Hanya satu method yang dibutuhkan untuk menjalankan job, yakni `run()`. Kamu boleh membuat method lain di dalamnya untuk mendukung proses run. Berikut ini contoh worker untuk menulis file ke dalam folder uploads/.

```php
<?php namespace App\cli\queueWorker;

class SampleWorker extends BaseWorker {

	// Required method, will be called by Queue class
	// This method will execute the job
	public function run()
	{
		$data = $this->jobData;

		$response = file_put_contents(SITEPATH.'resources/logs/'.$data['filename'], $data['content']."\n", FILE_APPEND);

		if($response){
			$output['status'] = "success";
			$output['message'] = "file created";
		} else {
			$output['status'] = "failed";
			$output['message'] = "file fail to create";
		}

        return json_encode($output);
	}
}
```

Pada worker di atas, kita menjalankan job untuk menulis file, dimana ia memerlukan 2 buah data, yakni filename dan content. Data ini akan didapatkan dari antrian job pada Beanstalk tube.

Perhatikan bahwa method `run()` wajib mengembalikan output dalam format json, dimana paling tidak ada satu index dengan nama `'status'` yang bernilai `'success'` atau `'failed'`. Data status ini diperlukan oleh queue untuk menentukan apakah proses eksekusi job berhasil atau gagal. **Kembalikan nilai status `'failed'` hanya apabila Kamu ingin mengulang eksekusi job!**.

## Mengeksekusi Data Antrian

Untuk mengeksekusi data antrian menggunakan worker yang sudah kita buat, kita terlebih dahulu mendaftarkan tube untuk menyimpan job untuk dieksekusi oleh worker. Buka file application/config/application.php dan scroll ke bagian Beanstalk Configurations. Tambahkan nama tube di bagian index tubes dan namespace class workernya di bagian value.

```php
$config['beanstalkd'] = [
    'host' => '127.0.0.1',
    'port' => '11300',
    'tubes' => [
        'sample' => 'App\cli\queueWorker\SampleWorker',
    ]
];
```

Pada konfigurasi di atas kita membuat nama tube `sample` dengan value `'App\cli\queueWorker\SampleWorker'`. Artinya setiap job yang masuk ke dalam tube `sample` akan dieksekusi oleh class SampleWorker.

Setelah itu kita bisa mengaktifkan worker dengan menjalankan command berikut ini dari terminal:

```
php cli command queue/consume/sample -d heroicbit.test
```

dengan segment ketiga adalah nama tube yang akan dipantau, dalam contoh kasus di atas adalah `sample`.

Command terminal ini akan terus mengeksekusi setiap kali ada job masuk. Untuk menjadikannya service di server kita akan perlu menginstall aplikasi seperti PM2 di Ubuntu.

## Menyimpan Data Antrian

Sekarang kita akan memasukkan antrian ke dalam tube `sample`. Kamu dapat mengisi antrian dari manapun di kodemu menggunakan snippet ini:

```php
$data = [
    'filename' => 'cobabeanstalk.txt', 
    'content' => $i . ' ' . date("Y-m-d H:i:s")
];

App\libraries\Beanstalk::produce('sample', $data);
```

Selama queue worker menyala, setiap kali ada job masuk ke dalam tube ia akan langsung dieksekusi.