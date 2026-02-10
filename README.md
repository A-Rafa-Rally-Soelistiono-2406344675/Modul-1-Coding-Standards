# Eshop Reflection
## Module 1 - Coding Standards
## Reflection 1

---

### Clean Code Principles Applied

1. **Separation of Concerns**, saya menerapkannya dengan memisahkan controller, service, dan repository ke dalam package yang berbeda (`controller`, `service`, `repository`). Pemisahan ini akan membuat setiap lapisan memiliki tanggung jawab yang jelas dan mencegah pencampuran logika antar layer, sehingga kode lebih mudah dipahami.

2. **Single Responsibility Principle**, saya memastikan setiap kelas hanya memiliki satu tanggung jawab utama. Controller hanya bertugas menangani request dan response, service menangani logika kode, dan repository menangani penyimpanan serta pengambilan data.

3. **Meaningful Naming**, saya melakukan penamaan kelas dan metode seperti `create`, `findAll`, `findById`, `update`, dan `deleteById`. Penamaan ini bersifat deskriptif sehingga tujuan dan fungsi kode dapat dipahami tanpa perlu membaca implementasinya secara mendetail.

---

### Secure Coding Practices Applied

1. **Avoiding SQL Injection Risks** , Pada tahap ini, repository masih menggunakan in-memory storage sehingga tidak ada interaksi langsung dengan database dan risiko SQL injection dapat dihindari.

2. **Minimalisir Data Exposure** menerapkan view hanya menampilkan data yang benar-benar dibutuhkan oleh interface. Hal ini membantu mengurangi risiko kebocoran data yang tidak perlu.

3. **Post/Redirect/Get Pattern** digunakan setelah operasi yang mengubah data, seperti create, update, dan delete. Dengan melakukan redirect setelah POST, aplikasi dapat mencegah pengiriman ulang data yang sama ketika pengguna melakukan refresh halaman.

---

### Mistakes / Gaps Found and Improvements

1. **Penghapusan data menggunakan metode GET tidak aman** karena dapat memicu penghapusan tidak disengaja atau disalahgunakan. Perbaikan yang disarankan adalah menggunakan metode POST atau DELETE dengan perlindungan CSRF melalui Spring Security.

2. **Validasi input belum diterapkan** pada field seperti `productName` dan `productQuantity`. Perbaikan dapat dilakukan dengan menambahkan anotasi validasi seperti `@NotBlank` dan `@Min(0)` serta menangani error validasi di controller.

3. **Penanganan error masih minim** karena ketika data produk tidak ditemukan, aplikasi hanya melakukan redirect tanpa memberikan informasi kepada pengguna. Perbaikannya adalah dengan menampilkan pesan error atau halaman khusus agar pengguna memahami apa yang terjadi.

4. **Manajemen ID produk masih lemah** karena ID dihasilkan di service tanpa pengecekan keunikan jika ID diberikan secara manual. Perbaikannya adalah dengan selalu menghasilkan ID di sisi server atau melakukan validasi keunikan sebelum menyimpan data.

5. **Tipe input quantity kurang tepat** karena masih menggunakan `type="text"` yang memungkinkan input tidak valid. Perbaikan dapat dilakukan dengan mengganti menjadi `type="number"` dan tetap menambahkan validasi di server side.

---

## Reflection 2

---

### Unit Test, Code Coverage, dan Kualitas

Setelah menulis unit test, saya merasa lebih percaya diri karena kelas sudah terdokumentasi dan dapat diverifikasi secara otomatis melalui unit test. Jumlah unit test dalam sebuah kelas sebaiknya beberapa **fungsi dan logika** yang perlu diverifikasi. Prinsip yang saya gunakan adalah satu test = satu skenario, ini memungkinkan bahwa semua edge mampu terverifikasi dengan baik. Selain itu, fokus pada public function suatu code, bukan pada detail implementasi.

Untuk memastikan test cukup, saya mengombinasikan:

- semua public function dan kondisi penting sudah diuji.
- melihat seberapa besar **Code coverage** yang ada.
- memastikan **Mutation testing / negative testing**.

**code coverage**, metrik ini berguna untuk mengetahui seberapa banyak baris kode yang dieksekusi oleh test. Namun **100% coverage tidak berarti bebas bug**. Dengan begitu,

- Test bisa mengeksekusi kode tanpa melakukan assert yang kuat.
- Bug logika bisa hiding pada kombinasi input yang belum diuji.
- Coverage hanya mengukur eksekusi, bukan kebenaran code.


### Refleksi Clean Code pada Suite Uji Fungsional Baru

Jika saya membuat suite uji fungsional baru yang sangat mirip dengan `CreateProductFunctionalTest.java`, lalu menyalin prosedur setup dan variabel instance yang sama, maka clean code bisa menurun. hal ini dikarenakan:

- **Duplicate code**, codingan yang sama muncul di banyak class test, membuat perubahan kecil menjadi ribet karena harus diedit di banyak tempat.
- **Inisialisasi berulang**, variabel instance yang identik di beberapa class risiko inkonsistensi.
- **Sulit dirawat**, saat konfigurasi test berubah, semua class harus diperbarui manual.

Perbaikan yang bisa dilakukan:

- **Membuat base test class**: misalnya `BaseFunctionalTest` yang berisi client side dan data awal.
- **Menggunakan @BeforeEach atau @BeforeAll** secara konsisten di base class.
- **Membuat helper atau test utility** untuk data produk atau request, sehingga detail repetitif dipusatkan.
- **Test parameterization** jika skenario uji berbeda hanya pada input dan expected output.

