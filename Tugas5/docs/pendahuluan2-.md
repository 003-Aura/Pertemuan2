# Pendahuluan P2 — Analisis JSONPlaceholder

## 1. Perbandingan Struktur Data `/posts/1` dan `/users/1`

Endpoint `/posts/1` digunakan untuk mengambil data postingan dengan ID 1. Data yang dikembalikan memiliki beberapa field, yaitu `userId`, `id`, `title`, dan `body`. `userId` menunjukkan ID pengguna yang membuat postingan, `id` merupakan ID postingan, `title` merupakan judul postingan, dan `body` merupakan isi postingan.

Sedangkan endpoint `/users/1` digunakan untuk mengambil data pengguna dengan ID 1. Data yang dikembalikan berisi informasi pengguna seperti `id`, `name`, `username`, `email`, `address`, `phone`, `website`, dan `company`.

Perbedaannya adalah `/posts/1` berfokus pada data postingan, sedangkan `/users/1` berfokus pada informasi pengguna.

---

## 2. Kelebihan dan Keterbatasan JSONPlaceholder

JSONPlaceholder memiliki kelebihan sebagai bahan latihan karena mudah digunakan, gratis, dan tidak memerlukan pembuatan server atau database sendiri. JSONPlaceholder juga menyediakan berbagai HTTP method seperti GET, POST, PUT, PATCH, dan DELETE sehingga dapat digunakan untuk memahami konsep REST API.

Namun, JSONPlaceholder memiliki keterbatasan karena merupakan API simulasi dengan data dummy. Oleh karena itu, JSONPlaceholder tidak ditujukan untuk digunakan sebagai API produksi atau sebagai database utama pada aplikasi nyata.

---

## 3. Hubungan URL, Method, dan Data yang Dikembalikan pada `/posts`

URL digunakan untuk menentukan resource atau data yang ingin diakses. Contohnya `/posts` digunakan untuk mengakses resource berupa postingan, sedangkan `/posts/1` digunakan untuk mengakses postingan dengan ID 1.

HTTP method menentukan operasi yang dilakukan terhadap resource tersebut. Contohnya GET digunakan untuk mengambil data, POST untuk membuat data baru, PUT dan PATCH untuk memperbarui data, sedangkan DELETE untuk menghapus data.

Setelah request dikirim, JSONPlaceholder memberikan response berupa data JSON dan HTTP status code yang menunjukkan hasil dari request tersebut.

---

## 4. Perbedaan `/posts/1` dan `?userId=1`

`/posts/1` digunakan untuk mengambil postingan berdasarkan ID postingan. Dengan demikian, `/posts/1` berarti mengambil postingan yang memiliki ID 1.

Sedangkan `/posts?userId=1` menggunakan query parameter untuk melakukan filter berdasarkan ID pengguna. Artinya, endpoint tersebut digunakan untuk mengambil postingan yang dibuat oleh pengguna dengan `userId` 1.

Perbedaannya adalah `/posts/1` menghasilkan data berdasarkan ID postingan, sedangkan `/posts?userId=1` menghasilkan data berdasarkan ID pengguna dan dapat mengembalikan beberapa postingan.