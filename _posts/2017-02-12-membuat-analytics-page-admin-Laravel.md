---
layout: post
date: 2017-02-12 11:33:00 +0700
title: Membuat Analytics Page Admin Laravel
author: baddwin
category: programming
tags: [web,framework,php,laravel,api]
summary: "Analytics page merupakan fitur yang biasanya ditempatkan pada admin page. Gunanya adalah untuk memantau jumlah pengunjung website. Tulisan ini akan menerangkan sedikit bagaimana menambahkan fitur analytics pada admin page di Laravel dengan memanfaatkan Google Analytics API."
--- 

# Daftar Isi

* Daftar Isi
{:toc}

# Latar Belakang

[Laravel](http://laravel.com) merupakan framework PHP yang semakin populer sekarang ini. Karena kepopulerannya, penulis pun menjadi tertarik untuk mendalaminya. Semakin banyak tutorial tentang Laravel di luar sana, entah dalam blog, kursus daring, buku, maupun video. Pengembang Laravel sendiri sudah menyediakan platform belajar Laravel di [Laracasts](https://laracasts.com). Kali ini penulis ingin berbagi sedikit pengetahuan tentang Laravel.

# Membuat Analytics Page Admin

Analytics page merupakan fitur yang biasanya ditempatkan pada admin page. Gunanya adalah untuk memantau jumlah pengunjung website. Tulisan ini akan menerangkan sedikit bagaimana menambahkan fitur analytics pada admin page di Laravel dengan memanfaatkan Google Analytics API.

# Persyaratan

Untuk bisa mengikuti tutorial ini, pembaca diasumsikan sudah memenuhi persyaratan berikut:

1. Local development environment untuk Laravel, antara lain HTTPD *service* (Apache atau NGINX), PHP 5.6+, MySQL/MariaDB, Composer. Bagi pembaca yang menggunakan Windows bisa menggunakan [Laragon](https://laragon.org) yang sudah menyertakan Composer dan semua yang dibutuhkan untuk *development* Laravel.
2. Domain website yang sudah *up* dan sudah didaftarkan ke [Google Analytics](https://analytics.google.com/analytics/web/).

# Persiapan

## Instalasi Laravel

Tentu saja kita memerlukan Laravel sebelum membuat analytics page yang dimaksud. Buatlah satu project baru menggunakan [Composer](https://getcomposer.org). Perintahkan baris berikut di Terminal.

    composer create-project --prefer-dist laravel/laravel laravel-analytics

Maka akan dibuat satu folder `laravel-analytics` di Home atau lokasi di mana perintah itu dijalankan. Perintah itu akan mengunduh Laravel versi terbaru. Saat tulisan ini ditulis, versi terbaru adalah 5.4.11.

Jika memakai Laragon, silakan lihat tutorialnya di [Youtube ini](https://www.youtube.com/watch?v=pHQOdXY2AZU&list=PL0Nx259JjLcGqXg5kpTifqQ9GFoQ2qGPy).

## Membuat Google Cloud Platform Project

Silakan login ke Google dan daftarkan diri ke [*developer account*](https://developers.google.com/console/) jika belum. Kemudian buatlah satu project, jika belum punya.

![App Project](/img/2017/02/membuat-google-project.png)

## Mengaktifkan Google Analytics API

Setelah berhasil membuat sebuah project, pada dasbor *developer console*, klik pada Aktifkan API. Kemudian carilah Analytics API, lalu klik Aktifkan.

Setelah itu, kita ambil *credential* yang diperlukan untuk mengakses API Analytics. Klik Kredensial pada sidebar sebelah kiri. Klik tombol Buat Kredensial dan pilihlah Kunci Akun Layanan. 

![Memilih Kunci akun layanan](/img/2017/02/membuat-kunci-akun.png)

Kemudian pilih Akun Layanan yang sudah ada, atau buat baru jika belum ada. Dan pada pilihan Jenis Kunci, pilih yang JSON. Lalu klik Buat. Simpan file JSON yang diunduh. Nanti kita butuhkan untuk Laravel.

![Mengunduh kredensial](/img/2017/02/memilih-json-credential.png)

## Menambahkan pengguna Google Analytics

Sekarang kita ke laman Google Analytics. Pilih salah satu website yang akan kita kelola. Pada tab Admin, bagian Properti, klik Pengelolaan Pengguna. Di situ kita tambahkan izin untuk email di file kredensial yang diunduh tadi. Salin node **email** di dalam file JSON tadi ke bidang yang tampil, kemudian klik Tambah.

![Menambah pengguna](/img/2017/02/analytics-view-id.png)

Jika kita tidak menambahkan email ini, maka kita tidak punya izin mengambil data API Analytics.

![Error insufficient permission](/img/2017/02/error-laravel.png)

Tunggu dulu, jangan menutup laman Google Analytics ini. Nanti kita masih perlu sesuatu dari sini.

# Memulai *coding*

Baiklah, kita sudah mempersiapkan semua yang dibutuhkan. Sekarang kita mulai tutorial ini. Pertama, sesuaikan variabel *environment* sistem kita pada file `.env`, terutama profil koneksi database MySQL, yaitu nama database, username, dan password. Jika belum ada file `.env`, cukup salin file `.env.example` ke `.env`, kemudian jangan lupa jalankan perintah:

    php artisan key:generate

Jika sudah oke, jalankan *migration* untuk membuat tabel database yang dibutuhkan untuk autentikasi pengguna. Nanti kita membutuhkannya untuk login ke *admin page*. Di Terminal, perintahkan:

    php artisan migrate

Pastikan *successful*. Jika ada galat, terutama di Laravel 5.4, kemungkinan karena Laravel mengharuskan MySQL versi 5.7+. Atau jika kita masih tertahan pada MySQL 5.5 atau MariaDB, kita bisa menanggulanginya dengan mengedit file `app\Providers\AppServiceProvider.php` seperti ini:

```php
...
use Illuminate\Support\Facades\Schema;

...
      public function boot()
      {
         Schema::defaultStringLength(191);
      }
...

```

## Membuat scaffolding autentikasi

Laravel sudah menyediakan sistem login user. Untuk menggunakannya, kita cukup memerintahkan:

    php artisan make:auth

Maka akan dibuat *view* login, register, dan admin home. Coba periksa apakah benar sudah berfungsi. Perintahkan baris berikut untuk menjalankan web server lokal:

    php artisan serve

Buka laman `http://localhost:8000` pada browser. Kemudian coba klik login. Seharusnya tampil halaman login, dan kita bisa membuat info login dengan klik register.

## Mengunduh package composer

Selanjutnya kita akan mengunduh *package* PHP untuk mengambil data API Google Analytics. Ada beberapa *package* yang sudah tersedia di repositori Composer, saya memilihkan *package* `ozankurt/laravel-analytics`. Di Terminal, perintahkan:

    composer require ozankurt/laravel-analytics

Tunggu hingga selesai. Lalu tambahkan baris beriku pada file `config/app.php` di bagian `'providers' => []`

```php
Kurt\Google\Core\CoreServiceProvider::class,
```

Kemudian perintahkan

    php artisan vendor:publish

Maka seharusnya akan dibuat file `config/google.php`. Di file itu, kita sesuaikan konfigurasinya. Seperti berikut ini.

```
'applicationName' => env('GSUITE_PROJECT_NAME','MyProject'),
'jsonFilePath' => storage_path(env('GSUITE_JSON_FILE', '')),
'scopes' => [
        'https://www.googleapis.com/auth/analytics.readonly',
    ],
'analytics' => [
        'viewId' => env('GSUITE_VIEW_ID', ''),
    ],
```

Kredensial akan kita taruh di `.env` saja. Jadi, kita perlu mendefinisikan environment variabel yang dibutuhkan.

```
GSUITE_PROJECT_NAME=MyProject                    # nama project Google tadi
GSUITE_JSON_FILE=app/kredensial-analytics.json   # lokasi file json kredensial yang diunduh tadi, kita letakkan di folder storage/app 
GSUITE_VIEW_ID=ga:xxxxxx                         # penjelasan di bawah
```

Variabel terakhir itu adalah View ID akun Google Analytics kita. Kita ambil di laman Analytics, di tab Admin, bagian Tampilan, klik Setelah Tampilan. Di situ ada ID Tampilan. Salin kode tersebut ke file `.env` kita.

## Membuat *controller*

Selanjutnya kita buat satu controller untuk mengambil data API Analytics.

    php artisan make:controller AnalyticsController

Untuk mempersingkat tutorial ini, ubah file `AnalyticsController` itu seperti ini:

```php
<?php

namespace App\Http\Controllers;

use Illuminate\Http\Request;
use Kurt\Google\Analytics\Analytics as GoogleAnalytics;
use Kurt\Google\Analytics\Exceptions;

class AnalyticsController extends Controller
{
    private $ga;

    public function __construct(GoogleAnalytics $ga) {
        $this->ga = $ga;
        $this->middleware('auth');
    }

    public function index()
    {
        $results = $this->ga->getPageViewsByDate();

        return dd($results);
    }
}

```

Untuk mengaksesnya, kita buat satu *route*.

```
Route::get('/home/analytics', 'AnalyticsController@index');
```

Coba lihat pada `http://localhost:8000/home/analytics`. Seharusnya akan tampil response JSON seperti ini.

![Respons data API](/img/2017/02/json-response-google-analytics.png)

## Membuat Tampilan

Setelah kita mendapatkan respons yang diharapkan, mari membuat tampilan (view) yang lebih *friendly*. Buatlah satu *view* misalnya di dalam folder `resources/views/admin`, kita namakan `analytics.blade.php`. *View* ini akan memanfaatkan [Chart.js](http://chartjs.org) sebagai *library* untuk menggambar grafik analitik.

```blade
@extends('layouts.app')

@section('content')
<div class="container">
    <div class="row">
        <div class="col-md-8 col-md-offset-2">
            <div class="panel panel-default">
                <div class="panel-heading">Analytics</div>

                <div class="panel-body">
                    @if (is_object($analytics))
                        <h4>Jumlah pengunjung</h4>
                        <p>Total visitor: {{ $analytics->sum('ga:pageviews') }}</p>
                        <canvas id="stat" width="200" height="200"></canvas>
                    @else
                        <h4>Error!</h4>
                        <p>{{ $analytics }}</p>
                    @endif
                </div>
            </div>
        </div>
    </div>
</div>
@endsection

@section('script')
    @if (is_object($analytics) && $analytics->count() > 0)
    <script src="https://cdnjs.cloudflare.com/ajax/libs/Chart.js/2.4.0/Chart.bundle.min.js" charset="utf-8"></script>
    <script type="text/javascript">
        var ctx = document.getElementById("stat");
        var statistik = new Chart(ctx, {
            type: 'bar',
            data: {
                labels: {!! $analytics->pluck('ga:date')->toJson() !!},
                datasets: [
                    {
                    label: "Statistik Visitor Web",
                    data: {!! $analytics->pluck('ga:pageviews')->toJson() !!}
                    }
                 ]
            }
        });
        // console.log({!! $analytics->pluck('ga:date')->toJson() !!});
    </script>
    @endif
@endsection
```

Dan `AnalyticsController` kita ubah sedikit, menjadi:

```php
...
        $results = $this->ga->getPageViewsByDate();
        $data = collect($results['rows']);
        // return dd($results);
        return view('admin.analytics')->with('analytics', $data);
```

Dan file `resources/views/layouts/app.blade.php` kita tambahkan `@yield`:

```
...
<!-- Scripts -->
    <script src="/js/app.js"></script>
    @yield('script')
...
```

Sekarang coba refresh laman analytics tadi. Seharusnya sudah tampil laman admin yang menampilkan grafik batang. Tampilan grafik bisa kita ubah sesuai keinginan kita. Edit pada bagian Javascript di blade template analytics. Method dan konfigurasi Chart.js bisa dilihat pada [laman dokumentasinya](http://www.chartjs.org/docs/).

Kira-kira demikian tutorial ini. Walaupun singkat, semoga bermanfaat.

# Tambahan

Sebenarnya ada *package* Composer [spatie/laravel-analytics](https://github.com/spatie/laravel-analytics) yang lebih mudah, tetapi *package* ini membutuhkan PHP versi 7.0+, selain itu, jika kita menggunakannya pada *production*, kita diwajibkan mengirim  kartu pos ke pengembangnya di Bergia sana.

*Source code* untuk tutorial ini ada di [Github penulis](https://github.com/baddwin/sinaudev-laravel).