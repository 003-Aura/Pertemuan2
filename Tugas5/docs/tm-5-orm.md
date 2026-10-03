# TM-5 — Connector vs ORM

## 1. Contoh `create.js`

Karena materi Pertemuan 2 tidak menyediakan file `totalJadwal.js` atau `create.js`, digunakan contoh sederhana untuk menggambarkan proses membuat data jadwal.

```js
const createJadwal = async (db, data) => {
  const [result] = await db.query(
    'INSERT INTO jadwal (nama, tanggal) VALUES (?, ?)',
    [data.nama, data.tanggal]
  );

  return result;
};

module.exports = createJadwal;
```js

Kode tersebut digunakan untuk menambahkan data jadwal ke dalam database menggunakan query SQL.

## 2. Versi Raw `mysql2`

Dengan `mysql2`, query SQL ditulis secara langsung.

```js
const mysql = require('mysql2/promise');

const db = mysql.createPool({
  host: 'localhost',
  user: 'root',
  password: '',
  database: 'silab'
});

const createJadwal = async (data) => {
  const [result] = await db.query(
    'INSERT INTO jadwal (nama, tanggal) VALUES (?, ?)',
    [data.nama, data.tanggal]
  );

  return result;
};

createJadwal({
  nama: 'Jadwal Praktikum',
  tanggal: '2026-10-03'
}).then(result => {
  console.log('Data berhasil dibuat:', result);
});


## 3. Versi Prisma

Dengan Prisma, proses database dilakukan menggunakan model dan method ORM.

### `findMany`

Untuk mengambil daftar data jadwal:

```js
const jadwal = await prisma.jadwal.findMany();

- create
Untuk menambahkan data jadwal:
```js
const jadwal = await prisma.jadwal.create({
  data: {
    nama: 'Jadwal Praktikum',
    tanggal: new Date('2026-10-03')
  }
});

create() digunakan untuk membuat data baru melalui model Prisma.


 5. Risiko dan Cara Mengurangi SQL Injection
SQL Injection dapat terjadi apabila input pengguna langsung digabungkan ke dalam string SQL.

Contoh raw SQL yang berisiko:

```js
const nama = req.body.nama;

const sql = `INSERT INTO jadwal (nama) VALUES ('${nama}')`;

const [result] = await db.query(sql);
```js

Pada contoh tersebut, input pengguna dimasukkan langsung ke dalam query SQL. Jika input tidak ditangani dengan benar, pengguna dapat menyisipkan perintah SQL yang tidak seharusnya dijalankan.


6. Cara Mengurangi Risiko SQL Injection

Pada mysql2, risiko SQL Injection dapat dikurangi menggunakan parameterized query.

const [result] = await db.query(
  'INSERT INTO jadwal (nama) VALUES (?)',
  [nama]
);

Nilai input dikirim sebagai parameter terpisah dari struktur query SQL.

Pada Prisma, data diberikan melalui object data sehingga nilai input tidak digabungkan langsung ke dalam string SQL.

const jadwal = await prisma.jadwal.create({
  data: {
    nama: nama
  }
});

Sequelize dan Prisma menyediakan mekanisme parameterisasi yang membantu memisahkan data pengguna dari perintah SQL.


7. Kesimpulan

Database connector seperti mysql2 memberikan kontrol langsung karena developer dapat menulis query SQL sendiri. Prisma sebagai ORM menyediakan abstraksi melalui model dan method seperti findMany() dan create().

Raw SQL tetap dapat digunakan, tetapi input pengguna harus dipisahkan dari query menggunakan parameterized query untuk mengurangi risiko SQL Injection.

Prisma dan Sequelize membantu mengurangi risiko tersebut melalui mekanisme parameterisasi dan abstraksi ORM.