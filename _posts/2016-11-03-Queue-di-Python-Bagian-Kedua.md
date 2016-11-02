---
layout: post
title: Queue di Python Bagian Kedua
author: rezha
category: pemrograman
tags: [python, queue]
---

## Double ended queue mengunakan deque

Class `deque` di modul `collections` dapat mempermudah kita membuat `double ended queue` di Python.
`Double ended queue` adalah queue yang memungkinkan kita untuk menambah atau mengurangi elemen dari kanan ataupun dari kiri, dan class `deque` memungkinkan kita melakukannya dengan efisien.

Mari kita import modulnya terlebih dahulu

```python
from collections import deque
```

Instansiasi kelas `deque`

```python
d = deque()
```

Menambahkan elemen dari kanan dan kiri
```python
d.append("b")
d.appendleft("a")
print(d)
# keluarannya adalah: deque(['a', 'b'])
```

Dengan cara yang sama, elemen dapat dihapus dari kanan dan kiri

d.pop()
d.popleft()
print(d)
# keluaran: deque([])


Sejak Python 2.6, kamu dapat membatasi jumlah elemen dalam `deque` dengan menggunakan argumen `maxlen` saat instansiasi. Jika limitnya telah terlewati, elemen dari sisi lain akan dihapus ketika menambahkan elemen baru.

```python
d = deque(maxlen=3)
deque([], maxlen=3)
for i in range(4):
    d.append(i)
    print(d)
...
# keluaran:
deque([0], maxlen=3)
deque([0, 1], maxlen=3)
deque([0, 1, 2], maxlen=3)
deque([1, 2, 3], maxlen=3) # elemen '3' masuk dari sisi kanan, elemen paling kiri yakni '0' akan terhapus
```

Disadur dari [sini](https://rezhajulio.id/Queue-in-python-part-2/)

