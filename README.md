# Ringkasan Materi Python OOP

## 1. Konsep Dasar
**OOP (Object-Oriented Programming)** adalah paradigma pemrograman yang mengorganisasi kode ke dalam **objek**, yaitu gabungan data (atribut) dan perilaku (method).

- **Class**: cetak biru/template untuk membuat objek.
- **Object (instance)**: hasil nyata dari sebuah class.
- **Atribut**: variabel milik objek.
- **Method**: fungsi milik objek.

```python
class Mahasiswa:
    kampus = "Universitas X"          # atribut class

    def __init__(self, nama, nim):    # constructor
        self.nama = nama              # atribut instance
        self.nim = nim

    def perkenalan(self):             # method
        return f"Halo, saya {self.nama} ({self.nim})"

m1 = Mahasiswa("Budi", "123")
print(m1.perkenalan())
```

`self` merujuk ke objek itu sendiri, dan `__init__` dipanggil otomatis saat objek dibuat.

## 2. Empat Pilar OOP

### a. Encapsulation (Enkapsulasi)
Membungkus data dan membatasi akses langsung ke dalamnya.

- `public` → `self.nama`
- `protected` (konvensi) → `self._nama`
- `private` (name mangling) → `self.__nama`

```python
class Rekening:
    def __init__(self, saldo):
        self.__saldo = saldo

    def get_saldo(self):
        return self.__saldo

    def setor(self, jumlah):
        if jumlah > 0:
            self.__saldo += jumlah
```

Bisa juga memakai `@property` sebagai getter/setter:

```python
class Produk:
    def __init__(self, harga):
        self._harga = harga

    @property
    def harga(self):
        return self._harga

    @harga.setter
    def harga(self, nilai):
        if nilai < 0:
            raise ValueError("Harga tidak boleh negatif")
        self._harga = nilai
```

### b. Inheritance (Pewarisan)
Class anak mewarisi atribut dan method dari class induk.

```python
class Hewan:
    def __init__(self, nama):
        self.nama = nama

    def suara(self):
        return "..."

class Kucing(Hewan):
    def suara(self):
        return "Meong"

class Anjing(Hewan):
    def __init__(self, nama, ras):
        super().__init__(nama)   # memanggil constructor induk
        self.ras = ras
```

- `super()` dipakai untuk mengakses class induk.
- Python mendukung **multiple inheritance**: `class C(A, B)`. Urutan pencarian method mengikuti **MRO** (`C.__mro__`).

### c. Polymorphism (Polimorfisme)
Method dengan nama yang sama berperilaku berbeda tergantung objeknya.

```python
for h in [Kucing("Tom"), Anjing("Rex", "Husky")]:
    print(h.suara())   # Meong, lalu suara Anjing/induk
```

Python juga mendukung **duck typing**: yang penting objek punya method yang dipanggil, tanpa peduli tipenya.

### d. Abstraction (Abstraksi)
Menyembunyikan detail implementasi dan hanya menampilkan hal yang penting. Gunakan modul `abc`.

```python
from abc import ABC, abstractmethod

class Bentuk(ABC):
    @abstractmethod
    def luas(self):
        pass

class Persegi(Bentuk):
    def __init__(self, s):
        self.s = s

    def luas(self):
        return self.s ** 2
```

Class abstrak tidak bisa diinstansiasi langsung, dan subclass wajib mengimplementasikan semua abstract method.

## 3. Jenis Method

| Jenis | Dekorator | Parameter pertama | Fungsi |
|---|---|---|---|
| Instance method | (tanpa) | `self` | Mengakses/mengubah data objek |
| Class method | `@classmethod` | `cls` | Mengakses data class |
| Static method | `@staticmethod` | (tidak ada) | Fungsi utilitas biasa dalam class |

```python
class Util:
    jumlah = 0

    @classmethod
    def tambah(cls):
        cls.jumlah += 1

    @staticmethod
    def kuadrat(x):
        return x * x
```

## 4. Magic / Dunder Methods

| Method | Fungsi |
|---|---|
| `__init__` | Constructor |
| `__str__` | Representasi string (untuk `print`) |
| `__repr__` | Representasi untuk developer/debug |
| `__len__` | Dipanggil oleh `len(obj)` |
| `__add__` | Operator `+` |
| `__eq__` | Operator `==` |
| `__del__` | Destructor |

```python
class Vektor:
    def __init__(self, x, y):
        self.x, self.y = x, y

    def __add__(self, other):
        return Vektor(self.x + other.x, self.y + other.y)

    def __str__(self):
        return f"({self.x}, {self.y})"
```

## 5. Atribut Class vs Instance
- **Atribut class**: dibagi ke semua objek (didefinisikan di luar `__init__`).
- **Atribut instance**: unik per objek (didefinisikan di `__init__` dengan `self`).

## 6. Composition vs Inheritance
- **Inheritance** = hubungan "is-a" (Kucing *adalah* Hewan).
- **Composition** = hubungan "has-a" (Mobil *punya* Mesin).

```python
class Mesin:
    def nyalakan(self):
        return "Mesin menyala"

class Mobil:
    def __init__(self):
        self.mesin = Mesin()
```

Prinsip umum: utamakan composition bila hubungannya bukan "is-a" yang jelas.

## 7. Ringkasan Cepat
- **Class** = template, **object** = instance dari class.
- `__init__` = constructor, `self` = referensi ke objek.
- **4 pilar**: Encapsulation, Inheritance, Polymorphism, Abstraction.
- `super()` memanggil class induk.
- `@property` untuk getter/setter yang rapi.
- `ABC` + `@abstractmethod` untuk class abstrak.
- `@classmethod` (`cls`) dan `@staticmethod` (tanpa `self`/`cls`).
- Dunder method memungkinkan operator overloading dan integrasi dengan fungsi bawaan Python.

Kalau mau, saya bisa tambahkan soal latihan, contoh studi kasus, atau versi yang lebih singkat untuk bahan ujian
