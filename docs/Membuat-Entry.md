# Membuat Entry

Entry adalah salah satu fitur andalan dari HeroicBit yang memungkinkan Anda untuk membuat CRUD dengan cepat. Fitur Entry menggunakan format yaml untuk mendefinisikan CRUD yang ingin dibuat secara otomatis. Contoh penulisannya seperti ini:

```yaml
name: Mahasiswa
description: Contoh CRUD Data Mahasiswa
icon: users
table: mahasiswa
show_admin_menu: true
parent_menu: "20:Contents:file"
menu_position: 10
soft_deletes: true
show_on_table: [nim,nama]
fields:
  nim:
    field: nim
    label: Nomor Induk
    form: text
    rules: required
  nama:
    field: nama
    label: Nama Lengkap
    form: text
    rules: required
  gender:
    field: gender
    label: Jenis Kelamin
    form: radio
    options:
      l: Laki-laki
      p: Perempuan
```

## Cara Membuat Entry

Setiap entry yang dibuat mewakili CRUD dari satu table di database. Entry dapat membuatkan table tersebut setelah file yaml didefinisikan, atau Anda dapat membuat entry dari table yang sudah ada di database.

File konfigurasi entry dapat ditulis di beberapa tempat di dalam project HeroicBit, diantaranya:

- `application/entries/[namaentry]/`

- `application/modules/[namamodule]/entries/[namaentry]/`

- `sites/[namasite]/entries/[namaentry]/`

- `sites/[namasite]/[namamodule]/entries/[namaentry]/`

Di dalam file entry kita dapat membuat 2 file konfigurasi, diantaranya `schema.yml` dan `Action.yml`. File `schema.yml` digunakan untuk mendefinisikan dokumen yaml dari entry, dan file `Action.php` digunakan untuk menuliskan kode PHP yang menunjang aksi tambahan dari entry.

## Generate Table Database

Entry yang sudah dibuat tapi belum memiliki table di database, dapat digenerate melalui halaman admin Configurations > Entries. Klik tombol Build Table pada baris entry yang ingin dibuatkan tablenya berdasarkan struktur field yang sudah ditulis pada file yaml.

Termasuk bila ada penambahan field baru di yaml, kita dapat mengupdate struktur table dengan mengklik tombol Sync Table. Aksi ini hanya menambahkan field yang belum ada di table dan tidak menghapus field yang dihilangkan di yaml.

## Properti Entry

Di bawah ini daftar setiap property yaml entry beserta penjelasannya:

| Properti                     | Status   | Opsi            | Default                                              | Penjelasan                                                                                                                                                                                                            |
| ---------------------------- | -------- | --------------- | ---------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`                       | wajib    | string          | -                                                    | Nama nama entry                                                                                                                                                                                                       |
| `table`                      | wajib    | string          | -                                                    | Nama table di database                                                                                                                                                                                                |
| `fields`                     | wajib    | array asosiatif | -                                                    | Struktur field yang digunakan pada table database. Detail tipe-tipe field dijelaskan di bagian fields.                                                                                                                |
| `description`                | opsional | string          | -                                                    | Penjelasan entry                                                                                                                                                                                                      |
| `icon`                       | opsional | string          | 'list'                                               | Nama icon untuk menu. Cek list icon [disini](https://modularcode.io/modular-admin-html/icons.html)                                                                                                                    |
| `soft_deletes`               | opsional | boolean         | false                                                | Apakah penghapusan data permanen atau by status. Set `true` akan membuat data diset terhapus dengan mengisi kolom deleted_at pada table. **Pastikan table memiliki kolom deleted_at**.                                |
| `show_admin_menu`            | opsional | boolean         | false                                                | Menampilkan atau tidak entry di menu sidebar                                                                                                                                                                          |
| `parent_menu`                | opsional | string          | null                                                 | Nama dari parent menu tempat menu entry disimpan, dalam format "[PosisiParentMenu]:[LabelParentMenu]:[IconParentMenu]". Misalnya "20:Contents:file". Bila tidak didefinisikan, entry akan disimpan sejajar menu utama |
| `menu_position`              | opsional | number          | 100                                                  | Urutan penempatan di menu sidebar                                                                                                                                                                                     |
| `sorting`                    | opsional | boolean         | true                                                 | Menampilkan form untuk memodifikasi urutan data                                                                                                                                                                       |
| `perpaging`                  | opsional | boolean         | true                                                 | Menampilkan form untuk memodifikasi jumlah baris per halaman                                                                                                                                                          |
| `filtering`                  | opsional | boolean         | true                                                 | Menampilkan form untuk memfilter data                                                                                                                                                                                 |
| `row_per_page`               | opsional | number          | 10                                                   | Mengatur jumlah default baris per halaman                                                                                                                                                                             |
| `hide_edit`                  | opsional | boolean         | false                                                | Menyembunyikan tombol aksi edit data                                                                                                                                                                                  |
| `disable_delete`             | opsional | boolean         | false                                                | Menonaktifkan klik tombol hapus data                                                                                                                                                                                  |
| `show_on_table`              | opsional | array           | field yang didefinisikan di properti `fields`        | Daftar field yang ingin ditampilkan di tabel data                                                                                                                                                                     |
| `fullwidth_table`            | opsional | boolean         | true                                                 | Mengatur apakah tabel data ditampilkan layar penuh atau tidak                                                                                                                                                         |
| `small_table`                | opsional | boolean         | true                                                 | Mengatur apakah tabel data ditampilkan dengan spasi kecil atau tidak                                                                                                                                                  |
| `export_csv`                 | opsional | boolean         | false                                                | Menampilkan atau tidak tombol export ke CSV                                                                                                                                                                           |
| `show_on_export`             | opsional | array           | field yang didefinisikan di properti `show_on_table` | Daftar field yang ingin ditampilkan pada hasil export csv                                                                                                                                                             |
| `show_on_api`                | opsional | array           | semua field di table                                 | Daftar field yang ingin ditampilkan pada response api                                                                                                                                                                 |
| `parent_module`              |          |                 |                                                      |                                                                                                                                                                                                                       |
| `parent_module_filter_field` |          |                 |                                                      |                                                                                                                                                                                                                       |
| `view_fields`                |          |                 |                                                      |                                                                                                                                                                                                                       |
| `view_query`                 |          |                 |                                                      |                                                                                                                                                                                                                       |
| `view_table`                 |          |                 |                                                      |                                                                                                                                                                                                                       |
| `action_buttons`             |          |                 |                                                      |                                                                                                                                                                                                                       |
