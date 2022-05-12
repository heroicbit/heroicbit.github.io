# Pendahuluan

HeroicBit menggunakan CodeIgniter Framework versi 3.1.x. Bagi Anda pecinta CodeIgniter akan merasa seperti di rumah sendiri, tentunya dengan fasilitas yang lebih lengkap. Ada penambahan dan modifikasi beberapa bagian, diantaranya:

- CodeIgniter system via Composer package
- HMVC (Hierarchical MVC) dengan modifikasi https://bitbucket.org/wiredesignz/codeigniter-modular-extensions-hmvc
- Latte Template Engine https://latte.nette.org/
- Base Model dengan modifikasi https://github.com/avenirer/CodeIgniter-MY_Model
- Template Modular Admin dengan modifikasi https://github.com/modularcode/modular-admin-html
- PHPASS untuk enkripsi password https://asecuritysite.com/encryption/phpass
- Pheanstalk untuk library queue dengan beanstalkd https://github.com/pheanstalk/pheanstalk
- PHPMailer untuk library email https://github.com/PHPMailer/PHPMailer
- Modifikasi cart menggunakan library https://github.com/seikan/Cart
- Implementasi namespace dengan PSR-4 autoload

## Fitur

Beberapa fitur utama yang sudah tersedia di halaman admin HeroicBit diantaranya:

- Page management
- Post management
- User and role permission management
- Site settings
- File manager
- Entry CRUD admin
- Dynamic variables
- Course management
- Payment management

## Struktur Folder

Berikut ini struktur folder dari HeroicBit:

```folder
project/
├── _domain/
├── application/
│   ├── cli/
│   ├── config/
│   ├── controllers/
│   ├── core/
│   ├── entries/
│   ├── helpers/
│   ├── hooks/
│   ├── language/
│   ├── libraries/
│   ├── modules/
│   ├── routes/
│   ├── settings/
│   ├── shortcodes/
│   ├── tests/
│   └── third_party/
├── docs/
├── public/
|   ├── filemanager/
|   ├── uploads/
|   ├── views/
|   └── index.php
├── sites/
|   └── default/
|       ├── configs/
|       ├── entries/
|       ├── modules/
|       ├── resources/
|       ├── routes/
|       ├── settings/
|       ├── shortcodes/
|   	└── theme/
├── vendor/
└── composer.json
```