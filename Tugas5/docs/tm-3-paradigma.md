# TM-3 — Perbandingan 4 Paradigma API

## 1. Kasus Dashboard
Kasus yang digunakan adalah dashboard yang membutuhkan tiga jenis data:

- Total jadwal
- Daftar jadwal
- Data tagihan

Empat paradigma yang dibandingkan adalah REST, GraphQL, gRPC, dan WebSocket/Polling.
---

## 2. REST
Pada pendekatan REST, setiap kebutuhan data dapat diperoleh melalui endpoint yang berbeda.
Contoh request:

```text
GET /api/v1/jadwal/jumlah-jadwal
GET /api/v1/jadwal
GET /api/v1/tagihan
```

- **Request:** sekitar 3 request
- **Request 1:** mengambil total jadwal.
- **Request 2:** mengambil daftar jadwal.
- **Request 3:** mengambil data tagihan.

### Kelebihan
- Sederhana dan mudah dipahami.
- Cocok untuk operasi CRUD.
- Menggunakan HTTP method yang umum digunakan.
- Mudah diuji menggunakan Postman atau curl.

### Kekurangan
- Membutuhkan beberapa request untuk mendapatkan data yang berbeda.
- Jumlah request dapat bertambah jika kebutuhan dashboard semakin banyak.
---

## 3. GraphQL
GraphQL memungkinkan client meminta beberapa jenis data melalui satu request.
Pada kasus dashboard digunakan konsep query `dashboardTrend`:

```graphql
query {
  dashboardTrend {
    totalJadwal
    daftarJadwal
    tagihan
  }
}
```

Dengan query tersebut, dashboard dapat meminta total jadwal, daftar jadwal, dan data tagihan melalui **1 request**.
- **Request:** 1 request
- **Data yang diperoleh:** total jadwal, daftar jadwal, dan tagihan.
- **Query:** `dashboardTrend`

### Kelebihan
- Beberapa jenis data dapat diminta dalam satu request.
- Client dapat menentukan data yang dibutuhkan.
- Cocok untuk dashboard yang membutuhkan banyak jenis data.

### Kekurangan
- Membutuhkan server GraphQL.
- Pengelolaan schema dan query lebih kompleks dibandingkan REST.

> **Catatan:** `dashboardTrend` merupakan skenario yang digunakan untuk perbandingan pada tugas ini, bukan klaim bahwa endpoint tersebut tersedia pada sistem SILAB.
---

## 4. gRPC
gRPC digunakan untuk komunikasi antar-service. Pada kasus ini penggunaannya bersifat hipotetis.
Contoh arsitektur:

```text
Dashboard Service
       |
       +----> Jadwal Service
       |
       +----> Tagihan Service
```

Dashboard Service dapat berkomunikasi dengan Jadwal Service dan Tagihan Service untuk memperoleh data yang diperlukan.
- **Jenis komunikasi:** antar-service
- **Contoh:** Dashboard Service berkomunikasi dengan Jadwal Service dan Tagihan Service.
- **Sifat contoh:** hipotetis sesuai kebutuhan perbandingan pada tugas.

### Kelebihan
- Cocok untuk komunikasi antar-service.
- Memiliki performa yang baik untuk komunikasi internal.
- Sesuai untuk sistem yang menggunakan beberapa service.

### Kekurangan
- Implementasinya lebih kompleks dibandingkan REST.
- Lebih cocok untuk komunikasi internal dibandingkan kebutuhan API web sederhana.
---

## 5. WebSocket / Polling
WebSocket dan polling digunakan apabila dashboard membutuhkan pembaruan data secara berkala atau secara real-time.

### WebSocket
WebSocket mempertahankan koneksi antara client dan server sehingga server dapat mengirimkan perubahan data kepada dashboard.

Contoh:

```text
Server
   |
   | Data berubah
   v
Dashboard
   |
   | Angka diperbarui
   v
User
```

Contohnya, ketika jumlah jadwal berubah, angka pada dashboard dapat diperbarui secara langsung tanpa menunggu request baru dari client.

### Polling
Polling dilakukan dengan cara client mengirim request secara berkala untuk memeriksa data terbaru.
Contoh:

```text
Dashboard → Request → Server
Dashboard ← Response ← Server

       Tunggu beberapa saat

Dashboard → Request → Server
Dashboard ← Response ← Server
```

Polling dapat digunakan jika data tidak harus benar-benar real-time tetapi tetap perlu diperbarui secara berkala.

### Kelebihan
- WebSocket cocok untuk kebutuhan data real-time.
- Server dapat mengirimkan update secara langsung melalui WebSocket.
- Polling relatif sederhana untuk kebutuhan update berkala.

### Kekurangan
- WebSocket membutuhkan pengelolaan koneksi yang lebih kompleks.
- Polling dapat menghasilkan banyak request jika interval terlalu pendek.
- Polling tidak benar-benar real-time karena bergantung pada interval request.
---

## 6. Perbandingan
| Paradigma | Request | Penggunaan | Kelebihan | Kekurangan |
|---|---:|---|---|---|
| REST | ±3 request | Resource dan CRUD | Sederhana dan mudah dipahami | Membutuhkan beberapa request |
| GraphQL | 1 request | Mengambil beberapa data sekaligus | Fleksibel dan mengurangi jumlah request | Lebih kompleks |
| gRPC | Antar-service | Komunikasi internal service | Performa baik | Implementasi lebih kompleks |
| WebSocket | Koneksi berkelanjutan | Data real-time | Update dapat diterima langsung | Pengelolaan koneksi lebih kompleks |
| Polling | Request berulang | Update berkala | Relatif sederhana | Dapat menghasilkan banyak request |

---

## 7. Analisis Jumlah Request
Jika dashboard menggunakan REST, kebutuhan tiga jenis data dapat dilakukan dengan tiga request:

```text
Request 1 → Total Jadwal
Request 2 → Daftar Jadwal
Request 3 → Data Tagihan
```

Sehingga:
- **REST:** N ≈ 3 request
- **GraphQL:** 1 request menggunakan `dashboardTrend`
- **gRPC:** komunikasi dilakukan antar-service dan jumlah request bergantung pada rancangan service.
- **WebSocket:** menggunakan koneksi yang dapat dipertahankan untuk menerima update.
- **Polling:** melakukan request secara berulang sesuai interval yang ditentukan.

---

## 8. Pilihan Paradigma
Untuk kasus dashboard yang membutuhkan total jadwal, daftar jadwal, dan data tagihan, paradigma yang dipilih adalah **GraphQL**.

### Alasan
GraphQL dipilih karena dashboard membutuhkan beberapa jenis data sekaligus. Dengan query `dashboardTrend`, ketiga jenis data tersebut dapat diminta melalui **1 request**.

Dibandingkan REST yang membutuhkan beberapa request ke endpoint yang berbeda, GraphQL lebih sesuai untuk kasus dashboard yang membutuhkan beberapa sumber data dalam satu halaman.

Pilihan ini berdasarkan kebutuhan kasus yang diberikan, bukan berarti GraphQL selalu lebih sesuai untuk semua jenis aplikasi.
---

## 9. Kapan Pilihan Diganti?
Pilihan GraphQL dapat diganti apabila kebutuhan sistem berubah.

### REST
REST dapat digunakan apabila sistem hanya membutuhkan resource dan operasi CRUD sederhana.

Contohnya:

```text
GET    /api/v1/jadwal
POST   /api/v1/jadwal
PUT    /api/v1/jadwal/:id
DELETE /api/v1/jadwal/:id
```

### gRPC
gRPC dapat digunakan apabila sistem berkembang menjadi beberapa service dan membutuhkan komunikasi antar-service.

Contohnya:

```text
Dashboard Service
       |
       +----> Jadwal Service
       |
       +----> Tagihan Service
```

### WebSocket
WebSocket dapat digunakan apabila dashboard membutuhkan pembaruan data secara real-time, misalnya jumlah jadwal harus langsung berubah ketika terdapat perubahan pada server.

### Polling
Polling dapat digunakan apabila data hanya perlu diperbarui secara berkala dan tidak membutuhkan pembaruan secara real-time.

---

## 10. Kesimpulan
Keempat paradigma memiliki karakteristik penggunaan yang berbeda.

- **REST** sesuai untuk resource dan operasi CRUD.
- **GraphQL** sesuai untuk mengambil beberapa jenis data melalui satu request.
- **gRPC** sesuai untuk komunikasi antar-service.
- **WebSocket** sesuai untuk pembaruan data secara real-time.
- **Polling** sesuai untuk pembaruan data secara berkala.

Pada kasus dashboard yang membutuhkan **total jadwal + daftar jadwal + tagihan**, **GraphQL dipilih** karena ketiga data tersebut dapat diminta melalui **1 request** menggunakan konsep `dashboardTrend`.

Jika kebutuhan sistem berubah menjadi CRUD sederhana, REST dapat digunakan. Jika sistem membutuhkan komunikasi antar-service, gRPC dapat dipertimbangkan. Jika membutuhkan pembaruan angka secara real-time, WebSocket dapat digunakan, sedangkan polling dapat digunakan untuk pembaruan berkala.