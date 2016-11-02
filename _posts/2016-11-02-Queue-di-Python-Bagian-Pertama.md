---
layout: post
title: Queue di Python Bagian Pertama
author: rezha
category: pemrograman
tags: [python, queue]
---

## Cara terbaik untuk implementasi queue sederhana

`list` sederhana dapat digunakan dengan mudah untuk digunakan dan diimplementasikan sebagai struktur data queue. Queue ini akan memiliki prinsip `first-in, first-out`.

Akan tetapi, pendekatan ini merupakan langkah yang tidak efisien karena memasukkan dan mengambil data dari index pertama `list` python cenderung "lambat" karena semua elemen perlu bergeser satu index.

Direkomendasikan untuk mengimplementasikan queue menggunakan modul `collections.deque`, karena modul ini telah didisain agar menambahkan dan mengambil data dengan cepat baik dari index pertama maupun terakhir.

```python
from collections import deque
queue = deque(["a", "b", "c"])
queue.append("d")
queue.append("e")
queue.popleft()
queue.popleft()
print(queue)
# keluarannya adalah: deque(['c', 'd', 'e'])
```

Queue balikannya dapat dimplementasikan dengan menggunakan metode `appendleft` daripada `append` dan metode `pop` daripada `popleft`

Disadur dari [sini](https://rezhajulio.id/Queue-in-python-part-1/)
