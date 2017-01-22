---
layout: post
title: Mengenal Telegram Bot
tags: [bot, telegram, definisi]
comments: true
author: ali
category: pemrograman
summary: Telegram adalah aplikasi perpesanan instan berbasis awan (_cloud based_), yang dirancang hampir untuk semua jenis _platform_. Telegram di samping sebagai aplikasi perpesanan, ia juga memiliki fitur pembuatan akun _bot_ atau kependekan dari robot, yang mana fungsinya memberikan otomatisasi beberapa perintah ketika dipergunakan.<br/><br/>Artikel kali ini, penulis mencoba memberikan pembahasan mengenai Telegram _bot_ baik dari sisi pengertiannya, manfaat dan keugunaan, beserta cara pembuatannya dan dengan bahasa pemerograman apa saja yang dapat digunakan untuk membuat Telegram _bot_ tersebut.<br/><br/>Perlu diketahui bahwa hampir semua aplikasi perpesanan memiliki _bot_. Namun sayangnya fitur _bot_ dari aplikasi perpesanan lainnya umumnya berbayar. Beda dengan Telegram yang cendrung gratis, mudah dibuat, dan dokumentasinya lengkap. Jadi topik mengenai Telegram _bot_ adalah topik yang sangat menarik dan perlu di bahas.<br/><br/>
---

Telegram adalah aplikasi perpesanan instan berbasis awan (_cloud based_), yang dirancang hampir untuk semua jenis _platform_ seperti Android, iOS, Windows dan Web (melalui peramban). 

Salah satu keunggulan terbesar dari Telegram adalah pesan yang dikirim disimpan dalam awan. Dengan demikian, hal ini dapat bermanfaat dalam pengunaan secara _multi-device_ artinya kita tidak perlu khawatir kehilangan pesan pada saat berpindah-pindah _device_, baik di _desktop_, _smartphone_ atau _web_. Semua pesan yang telah kita kirim sama dan tersinkronisasi pada _device_ yang kita pergunakan pada saat ber-Telegram.

Telegram hadir diranah publik pada pertengahan tahun 2013. Awalnya Telegram sama seperti aplikasi perpesanan instan lainnya, yakni hanya untuk mengirim pesan teks, gambar, video, audio, dan pesan suara (_voice chat_) saja. 

Namun seiring perjalanan waktu, Telegram berkembang pesat seperti adanya _sticker_, uniknya _sticker_ tersebut diberikan secara cuma-cuma, baik dari pemakaian maupun dari sisi pembuatannya. 

Kemudian adanya `supergroup`, ia merupakan grup tingkat lanjut, denganya kita dapat menampung banyak pengguna, bahkan hingga **5000 pengguna**. Juga fitur _public group_ dari _supergroup_ ini, salah satu contohnya seperti [Sinaudev](https://t.me/sinaudev). Fitur lain dari supergroup yakni dapat mengahapus dan menyunting pesan, _pin_ suatu pesan, dan memblokir pengguna. 

Dan seterusnya, masih banyak lagi beberapa fitur yang tidak penulis sebutkan di artikel ini. Anda bisa membacanya sendiri di situs resmi Telegram.

Bicara keunggulan di atas, alih-alih itu semua keunggulan yang terlihat dan dapat dirasakan bagi para pengguna "biasa" (_non developer_). Namun, bagi Anda seorang _developer_ ada keunggulan lain yang bisa diperoleh dari penggunaan Telegram ini yakni: (1) adanya format `monospace`. Ini sangat berguna ketika Anda sedang berbicara sebuah baris kode dalam percakapan di suatu grup; (2) adanya _bot_, yakni suatu akun yang dirancang khusus untuk otomatisasi suatu pesan.

Pembahasan kali ini, penulis berfokus pada pembahasan seputar Telegram _bot_ baik dari sisi pengertiannya, manfaat dan keugunaan, beserta cara pembuatannya dan dengan bahasa pemerograman apa saja yang dapat digunakan untuk membuat Telegram _bot_ tersebut.

Tulisan ini dibuat semata-mata untuk dijadikan sebagai salah satu referensi gambaran mengenai Telegram _bot_, yang mana untuk pengertian _bot_-nya itu sendiri saja dikalangan beberapa pengguna ataupun _developer_ masih terbilang cukup kabur. Karenanya, penulis mencoba memberikan sedikit pemahaman dengan bahasa sehari-hari yang cukup ringan.

Sebelum beralih kepada pembahasan _bot_, perlu diketahui bahwa hampir semua aplikasi perpesanan memiliki _bot_. Namun sayangnya fitur _bot_ dari aplikasi perpesanan lainnya umumnya berbayar. Beda dengan Telegram yang cendrung gratis, mudah dibuat, dan dokumentasinya lengkap. Jadi topik mengenai Telegram _bot_ adalah topik yang sangat menarik dan perlu di bahas.

# Pengertian _Bot_

_Bot_ adalah kependekan dari robot merupakan suatu aplikasi yang diprogram dan dirancang khusus untuk melakukan tugas secara otomatis. _Bot_ dikenal juga sebagai _software agent_. Karena fungsinya yang mirip dengan cara kerja manusia, bedanya hanya pada sisi otomatisasinya saja.

Dalam kehidupan sehari-hari sebetulnya kita sudah sering dihadapkan dengan _bot_, salah satu contohnya pada PABX ketika kita menelepon suatu instansi, perusahaan, atau layanan seluler, kita sering mendengar kalimat berikut: "_Selamat Datang di XYZ. Untuk pesan berbahasa Indonesia tekan satu. Untuk berbahasa Inggris tekan dua_".

_Nah_, itulah suatu gambaran dari salah satu bentuk _bot_. Ia telah diprogram untuk memberikan respon pesan secara otomatis kepada para penelepon.

Begitu pula dengan _bot_ di Telegram. Ia merupakan suatu akun yang diprogram dan dirancang khusus untuk melakukan tugas secara otomatis, yang mana tugas tersebut hampir mustahil dapat dilakukan oleh manusia karena ia dipergunakan secara terus menerus.

# Apa Manfaatnya dan Mengapa Perlu Adanya _Bot_?

Sesuai dengan pengertian dari _bot_ itu sendiri yakni untuk melaksanakan tugas secara otomatis. Otomatisasi itulah yang menjadi manfaat dari adanya Telegram _bot_ tersebut. Salah satu contoh seperti _bot_ [@bing](https://t.me/bing) yakni untuk pencarian gambar. Adanya bot tersebut mempermudah kita memberikan referensi gambar tanpa harus berselancar (_browsing_) terlebih dahulu pada peramban (_browser_) lalu memberikan tautan (_link_) tersebut pada suatu percakapan. Hal ini, berimplikasi pada dua kali kerja, oleh karena itu adanya _bot_ [@bing](https://t.me/bing) mempersingkat, sekali kerja, cukup masukan nama gambar yang kita ingin cari, dan _viola!_ gambar langsung dapat ditampilkan.

Lihat di bawah ini cara kerja _bot_ [@bing](https://t.me/bing):
<video controls><source src="/img/2017/01/20/bing-bot.mp4" type="video/mp4">  Your browser does not support HTML5 video.</video>

Contoh lainnya seperti _bot_ [@kbbibot](https://t.me/kbbibot). Merupakan _bot_ pencarian lema dari Kamus Besar Bahasa Indonesia (KBBI) yang bersumber dari Kementerian Pendidikan dan Kebudayaan (Kemendikbud). Dengannya, kita tidak perlu lagi buka kamus atau membukanya melalui peramban, cukup gunakan _bot_ tersebut dan masukan lema yang kita ingin cari.

Lihat di bawah ini cara kerja _bot_ [@kbbibot](https://t.me/kbbibot)
<video controls><source src="/img/2017/01/20/kbbi-bot.mp4" type="video/mp4">  Your browser does not support HTML5 video.</video>

_Nah_, dengan demikian sudah jelas bukan manfaat adanya Telegram _bot_? Itu baru dua contoh _bot_ belum _bot_ - _bot_ lainnya yang memiliki beberapa fitur canggih, seperti fitur AI (_artificial intelligence_), _group management_, dan lain sebagainya.

# Bagaimana Cara Membuatnya?

Cara membuat _bot_ di Telegram cukup mudah. Pertama-tama yang dilakukan adalah memanggil [@BotFather](https://t.me/BotFather) lalu kemudian masukan perintah `/newbot` dan berikan nama _bot_ yang Anda inginkan. Penamaan harus diakhir kata `bot`, Contohnya [@kbbibot](https://t.me/kbbibot), [@situsalibot](https://t.me/situsalibot), [@sinaudevbot](https://t.me/sinaudevbot).

Simak video di bawah ini tentang bagaimana cara membuat akun _bot_ dari [@BotFather](https://t.me/BotFather):
<video controls><source src="/img/2017/01/20/create-bot.mp4" type="video/mp4">  Your browser does not support HTML5 video.</video>

Sangat mudah bukan?

# Langkah Selanjutnya Setelah Membuat _Bot_

Perlu diketahui, jangan salah kaprah mengenai [@BotFather](https://t.me/BotFather)! Ia hanyalah sebuat _agent_ untuk **pembuatan akun _bot_ saja, dan _bot_ secara bawaan tidak akan berjalan**. Jadi Langkah selanjutnya kita perlu menyisipkan baris kode bahasa pemerograman agar _bot_ tersebut dapat berjalan dengan sebagaimana mestnya. Tanpa baris kode, _bot_ yang telah dibuat tidaklah akan bermakna apa-apa. Oleh karenanya, menjadi syarat mutlak bagi Anda untuk memahami paling tidak satu bahasa pemerograman, **jika tidak paham sama sekali tinggalkan tulisan sub-bab di bawah ini** pelajari dahulu bahasa pemerograman.

Meski demikian jangan berkecil hati, Anda masih tetap dapat membuat _bot_ tanpa harus _coding_ yakni dengan [Manybot](https://manybot.io/). Sesuai _tagline_-nya "_Create a Telegram bot without coding._". Akan tetapi, jangan berharap lebih menggunakan [Manybot](https://manybot.io/) bisa bermacam-macam sesuai keinginan Anda. Ia hanya bisa sesuai dengan fitur yang diberikan.

# Bahasa Pemerograman Apa Saja Yang Didukung?

Hampir semua bahasa pemerograman bisa. Bahkan bahasa skrip (_scripting language_) pun seperti batch (pada Windows) dan Bash (pada GNU/Linux) dapat melakukan itu. Yang penting adalah bisa mengirim (_post_) dan mem- _parsing_ JSON. Karena Telegram _bot_ menggunakan JSON dalam hal pertukaran data dari sisi _server_ ke Anda sebagai _client_.

Dalam pembuatan _bot_, Telegram mendukung dua model metode. Pertama menggunakan metode `long-polling`; kemudian yang kedua menggunakan metode `webhook`.

# Apa itu `Long-Polling`

Long Polling adalah suatu teknik proses di mana komputer atau controling device menunggu rikues dari perangkat atau device eksternal dengan mengecek secara terus-menerus.


 Keduanya memiliki kekurangan dan kelebihan masing-masing. Adapun kekurangan dan kelebihannya menurut rangkuman saya:
Kelebihan dan Kekurangan Long-Polling

Kelebihan:

    Long-Polling merupakan metode default Telegram.
    Portable artinya dengan metode ini Anda bisa menjalankannya dimanapun baik di komputer, di HP, di raspi, atau mungkin di dalam router, yang penting ada aplikasi yang menjalankannya. Jadi Anda cukup mengeksekusi aplikasi Anda untuk menjalankan bot ini.
    Tidak diwajibkan menggunakan hosting. Ini cocok bagi Anda yang ingin mencoba-coba bot jika Anda belum memahami hosting atau sekadar untuk coba-coba bot sebelum ditanam di hosting Anda.

Kekurangan:

    Proses membacanya chat cukup lama.
    Anda harus selalu meng-getUpdates untuk mendapatkan informasi.
    Tidak bisa 24 Jam Online, kecuali memang Anda menam di komputer yang terus menerus hidup dan terhubung internet semacam hosting. Tapi metode ini tidak disarankan, lebih baik menggunakan webhook.

Kelebihan dan Kekurangan Webhook

Kelebihan:

    Proses membaca chat lebih cepat dibandingan long-polling.
    Bisa Online 24 Jam. Dikarenakan webhook ditanam dalam hosting.

Kekurangan:

    Wajib memiliki hosting. Babas mau shared hosting atau VPS. Tapi disarankan menggunakan VPS agar tidak mengganggu bandwidth hosting Anda.
    Wajib menggunakan https. Untuk port bebas boleh 80, 88, 8443 atau port default SSL nya yakni 443.



