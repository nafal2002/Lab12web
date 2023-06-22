# Tugas Pemograman Web 2
## Tugas Praktikum 12 


| #               | Biodata           |
| --------------- | ----------------- |
| **Nama**        | Nafal Mumtaz fuadi|
| **NIM**         | 312110457         |
| **Kelas**       | TI.21.A.2         |
| **Mata Kuliah** | Pemrograman Web 2 |

<br>

## Apa itu REST API?
<p align="center">
  <img src="https://www.rtdip.io/api/images/rest-api-logo.png">
</p>

<p>Representational State Transfer (REST) adalah salah satu desain arsitektur Application
Programming Interface (API). API sendiri merupakan interface yang menjadi perantara
yang menghubungkan satu aplikasi dengan aplikasi lainnya.
REST API berisi aturan untuk membuat web service dengan membatasi hak akses client
yang mengakses API. Kenapa harus demikian?
Jika dianalogikan sebagai restoran, REST API adalah daftar menu. Pelanggan hanya bisa
memesan sesuai daftar menu meskipun si koki (server) bisa membuatkan pesanan
tersebut.</p>
<p>REST API bisa diakses atau dihubungkan dengan aplikasi lain. Oleh sebab itu, pembatasan
dilakukan untuk melindungi database/resource yang ada di server.</p>

## Cara kerja REST API menggunakan prinsip REST Server dan REST Client.
REST Server bertindak sebagai penyedia data/resource, sedangkan REST Client akan
membuat HTTP request pada server dengan URI atau global ID. Nantinya, server akan
memberikan response dengan mengirim kembali HTTP request yang diminta client.
Nah, data yang dikirim maupun diterima ini biasanya berformat JSON. Itulah kenapa REST
API mudah diintegrasikan dengan berbagai platform dengan bahasa pemrograman
ataupun framework yang berbeda.</p>
<p>Misalnya, Anda membuat backend project menggunakan REST API dengan bahasa
pemrograman PHP. Nantinya, REST API tersebut bisa dihubungkan dengan frontend yang
menggunakan Vue js.<p>

## Langkah-langkah Praktikum dan Persiapan
Periapan awal adalah mengunduh aplikasi REST Client, ada banyak aplikasi yang dapat digunakan untuk keperluan tersebut. Salah satunya adalah Postman. [Postman](https://www.postman.com/) merupakan aplikasi yang berfungsi sebagai REST Client, digunakan untuk testing REST API. Unduh apliasi Postman [Disini.](https://www.postman.com/downloads/)
1. Unduh aplikasi REST Client, misalnya [Postman](https://www.postman.com/), yang digunakan untuk testing REST API.
2. Membuat Model.
3. Membuat REST Controller.
4. Membuat Routing REST API.
5. Testing REST API CodeIgniter.


## Membuat Model
Pada modul sebelumnya sudah dibuat ArtikelModel, pada modul ini kita akan memanfaatkan model
tersebut agar dapat diakses melalui API.

## Membuat REST Controller
Pada tahap ini, kita akan membuat file REST Controller yang berisi fungsi untuk menampilkan,
menambah, mengubah dan menghapus data. Masuklah ke direktori app\Controllers dan buatlah file
baru bernama Post.php. Kemudian, salin kode di bawah ini ke dalam file tersebut:

```php
<?php

namespace App\Controllers;

use CodeIgniter\RESTful\ResourceController;
use CodeIgniter\API\ResponseTrait;
use App\Models\ArtikelModel;

class Post extends ResourceController
{
  use ResponseTrait;
  // all users
  public function index()
  {
    $model = new ArtikelModel();
    $data['artikel'] = $model->orderBy('id', 'DESC')->findAll();
    return $this->respond($data);
  }
  // create
  public function create()
  {
    $model = new ArtikelModel();
    $data = [
      'judul' => $this->request->getVar('judul'),
      'isi' => $this->request->getVar('isi'),
    ];
    $model->insert($data);
    $response = [
      'status' => 201,
      'error' => null,
      'messages' => [
        'success' => 'Data artikel berhasil ditambahkan.'
      ]
    ];
    return $this->respondCreated($response);
  }
  // single user
  public function show($id = null)
  {

    $model = new ArtikelModel();
    $data = $model->where('id', $id)->first();
    if ($data) {
      return $this->respond($data);
    } else {
      return $this->failNotFound('Data tidak ditemukan.');
    }
  }
  // update
  public function update($id = null)
  {
    $model = new ArtikelModel();
    $id = $this->request->getVar('id');
    $data = [
      'judul' => $this->request->getVar('judul'),
      'isi' => $this->request->getVar('isi'),
    ];
    $model->update($id, $data);
    $response = [
      'status' => 200,
      'error' => null,
      'messages' => [
        'success' => 'Data artikel berhasil diubah.'
      ]
    ];
    return $this->respond($response);
  }
  // delete
  public function delete($id = null)
  {
    $model = new ArtikelModel();
    $data = $model->where('id', $id)->delete($id);
    if ($data) {
      $model->delete($id);
      $response = [
        'status' => 200,
        'error' => null,
        'messages' => [
          'success' => 'Data artikel berhasil dihapus.'
        ]
      ];
      return $this->respondDeleted($response);
    } else {
      return $this->failNotFound('Data tidak ditemukan.');
    }
  }
}
```

<p>Kode diatas berisi 5 method, yaitu:</p>

- index() – Berfungsi untuk menampilkan seluruh data pada database.
- create() – Berfungsi untuk menambahkan data baru ke database.
- show() – Berfungsi untuk menampilkan suatu data spesifik dari database.
- update() – Berfungsi untuk mengubah suatu data pada database.
- delete() – Berfungsi untuk menghapus data dari database.

<p>
<img src="https://images.squarespace-cdn.com/content/v1/59051f5dbebafb1fcb3f32ec/1583599848894-NQK12W7YU5BFP9IJ0461/Rest%2BAPI-8.png" style="width:500px; height:260px;">
</p>

## Membuat Routing REST API
- Untuk mengakses REST API CodeIgniter, kita perlu mendefinisikan route-nya terlebih dulu.
Caranya, masuklah ke direktori app/Config dan bukalah file Routes.php. Tambahkan kode
di bawah ini:</p>

```php
$routes->resource('post');
```

- Untuk mengecek route nya jalankan perintah berikut:

```bash
php spark routes
```

- Selanjutnya akan muncul daftar route yang telah dibuat.

![Routes](img/routes.png)

- Seperti yang terlihat, satu baris kode routes yang di tambahkan akan menghasilkan banyak
Endpoint.

- Selanjutnya melakukan uji coba terhadap REST API CodeIgniter.

## Testing REST API CodeIgniter
- Buka aplikasi postman dan pilih create new → HTTP Request

![Postman](img/postman.png)

### Menampilkan Semua Data 
- Pilih method GET dan masukkan URL berikut: http://localhost:8080/post
- Lalu, klik Send. Jika hasil test menampilkan semua data artikel dari database, maka pengujian
berhasil.

![Menampilkan Semua Data di Postman](img/menampilkan_semua_data_postman.png)

### Menampilkan Data Spesifik
- Masih menggunakan method GET, hanya perlu menambahkan ID artikel di belakang URL
seperti ini: http://localhost:8080/post/5

- Selanjutnya, klik Send. Request tersebut akan menampilkan data artikel yang memiliki ID
nomor 5 di database.

![Menampilkan Data Spesifik di Postman](img/menampilkan_data_spesifik_postman.png)

### Mengubah Data
- Untuk mengubah data, silakan ganti method menjadi PUT. Kemudian, masukkan URL artikel
yang ingin diubah. Misalnya, ingin mengubah data artikel dengan ID nomor 5, maka masukkan
URL berikut: http://localhost:8080/post/5
- Selanjutnya, Masukkan nama atribut tabel pada kolom KEY dan nilai data yang baru pada kolom VALUE. Kalau sudah,
klik Send.

![Mengubah Data di Postman](img/mengubah_data_postman.png)

- Maka hasilnya akan seperti ini.

![Hasil Ubah Data](img/hasil_ubah_data.png)

### Menghapus Data
- Pilih method DELETE untuk menghapus data. Lalu, masukkan URL spesifik data mana yang
ingin di hapus. Misalnya, ingin menghapus data nomor 16, maka URL-nya seperti ini: http://localhost:8080/post/16
- Langsung saja klik Send, maka akan mendapatkan pesan bahwa data telah berhasil dihapus dari
database.

![Menghapus Data di Postman](img/menghapus_data_postman.png)

## Pertanyaan dan Tugas
<p>Selesaikan programnya sesuai Langkah-langkah yang ada. Anda boleh melakukan improvisasi.</p>

## Laporan Praktikum
1. Melanjutkan praktikum sebelumnya pada repository dengan nama Lab11Web.
2. Kerjakan semua latihan yang diberikan sesuai urutannya.
3. Screenshot setiap perubahannya.
4. Update file README.md dan tuliskan penjelasan dari setiap langkah praktikum beserta screenshotnya.
5. Commit hasilnya pada repository masing-masing.
6. Kirim URL repository pada e-learning ecampus.

## Terima Kasih!
