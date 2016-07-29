---
layout: post
title: Pengenalan DBeaver (DDL & DML)Part 2
author: aries
category: linux
tags: [sql client]
---
Pada tulisan sebelumnya saya hanya membahas mengenai apa itu Dbeaver dan bagaimana menghubungkan database dengan aplikasi tersebut. Pada tulisan kali ini saya akan mencoba untuk membahas mengenai fitur yang lebih dekat dengan kegiatan secara umum saat kita menggunakan database. Atau istilahnya yaitu DDL dan DML.

Sebagai pengingat, yang dimaksud DDL ( Data Definition Language ) digunakan untuk mendefinisikan struktur, contoh perintah sqlnya seperti : `create table`, `alter table`, dan lain sebagainya. Sedangkan DML ( Data Manipulation Language ) digunakan untuk memanipulasi data, contoh perintah sqlnya seperti : `insert`, `update`, `delete`.

Lalu bagaimana pemanfaatan DDL dan DML di Dbeaver itu sendiri ?  Yang pertama adalah memilih database aktif. Pada tulisan saya yang pertama disebutkan untuk mengisi informasi sambungan ke database dibutuhkan informasi _server host, port, database, username, dan password_. Apakah dengan begitu kita hanya mampu mengakses database yang ditulis tadi ? Tentu tidak, kita masih bisa memilih database lainnya. Caranya mudah yaitu **klik kanan pada database yang dipilih -> set active** , jika dituliskan perintah sqlnya adalah `use namadatabase`.

![Set Aktif Database](/img/db_set_active.png)


### Membuat Tabel

Untuk membuat tabel baru di Dbeaver juga mudah sekali, setelah memilih database mana yang aktif, di kolom utama cukup **klik kanan->Create New Table...** maka halaman membuat tabel akan muncul seperti di bawah ini

![Tabel baru](/img/create_tabel_info.png)


Di sana terdapat beberapa sub menu sesuai fungsinya masing-masing, setidaknya ada 5 yang harus diketahui

1. Information : Berisi informasi umum tentang tabel dari nama tabel, _engine_, _charset_ , dan sebagainya.

2. Columns : Fungsinya untuk menambah/mengubah/menghapus kolom atau field dari tabel yang bersangkutan.

3. Constraints : Berfungsi untuk set sebuah field sesuai constraint yang hendak dipilih misalnya sebagai _primary key_ atau _unique key_.

4. Foreign Keys : Berfungsi untuk menghubungkan relasi antara dua tabel.

5. Triggers : Sesuai namanya ini berfungsi untuk membantu kita dalam pembuatan trigger dalam satu tabel. Untuk yang satu ini dibahas terpisah.


![Foreign Key](/img/create_tabel_forgn.png)



Pada gambar di atas memperlihatkan saya sedang memilih tabel kelas sebagai acuan dan kolom yang saya beri nama "child" adalah _foreign key_ dari tabel yang saya buat. 

Cara yang dilakukan di atas tadi jika di tuliskan kurang lebih seperti ini :

```sql
CREATE TABLE siswa (
  id int(11) NOT NULL AUTO_INCREMENT,
  child int(11) NOT NULL,
  PRIMARY KEY (id),
  KEY tabel_kelas_FK (child),
  CONSTRAINT tabel_kelas_FK FOREIGN KEY (child) REFERENCES kelas (child)
) ENGINE=InnoDB DEFAULT CHARSET=utf8

```

### Select, Insert, Update, dan Delete

**Select**

Select merupakan perintah untuk memuculkan data yang ada pada tabel tertentu. Di aplikasi ini untuk memunculkan data cukup dengan klik dua kali tabel yang akan dimunculkan datanya. Lalu setelah informasi tabel muncul di kolom utama aplikasi, pilih tab "Data" untuk memperlihatkan data yang ada.

Jika dituliskan dalam perintah sql ini sama dengan :

```sql

select * from namatable

```

**Insert**

Pilih tabel yang akan ditambah datanya, lalu pilih simbol "+" yang berada di bawah maka satu kolom akan berubah berwana hijau yang menandakan kolom sudah bisa diisi. Untuk menyimpannya pilih simbol "✔". Jelasnya lihat gambar di bawah ini.


![insert](/img/insert.png)

Jika dituliskan dalam perintah sql ini sama dengan :

```sql

INSERT INTO namatable
(id_kelas, nama, created_at, updated_at)
VALUES(1, 'Kelas Satu', STR_TO_DATE('2016-07-29 17:39:33','%Y-%m-%d %H:%i:%s'), NULL);


```


**Update**

Untuk update jauh lebih mudah lagi, cukup klik dua kali pada kolom yang hendak diubah, dan jika kolom tersebut berubah menjadi textbox maka kolom tersebut siap untuk diubah.


![update](/img/update.png)


Jika dituliskan dalam perintah sql ini sama dengan :

```sql

UPDATE namatable
SET nama= "kelas_satu dua"
WHERE id_kelas=1;


```

**Delete**

Untuk delete ( hapus ), caranya sorot terlebih dahulu baris yang akan dihapus lalu pilih icon "-" yang berada di bawah sejajar dengan simbol "+". Setelah diklik maka baris tersebut akan berubah warna menjadi merah atau oranye. Jika yakin akan dihapus pilih simbol "✔" jika tidak pilih simbol "X".

![hapus](/img/hapus.png)


Jika dituliskan dalam perintah sql ini sama dengan :

```sql

DELETE FROM namatable
WHERE id_kelas=1;


```

**Diagram**

Saya seringkali di kantor disuruh untuk print diagram database yang sedang digunakan. Kadang diminta diagram keseluruhan database kadang cukup diagram dari tabel yang spesifik. Di aplikasi ini kemampuan memunculkan diagram cukup bagus. Bisa untuk keseluruhan database bisa juga cukup tabel yang dipilih.
Dan satu lagi diagram yang dihasilkan dapat langsung disimpan sebagai gambar.

![diagram](/img/diagram.png)

Untuk part 2 mengenai Dbeaver cukup sampai di sini, semoga tulisan ini cukup membantu. Jika ada masukan, saran, kritik, atau apapun dengan senang hati saya menerimanya.


Referensi :

* http://stackoverflow.com/questions/2578194/what-is-ddl-and-dml