# Eshop Reflection

## Clean Code Principles Applied

1. **Separation of Concerns**, saya menerapkannya dengan memisahkan controller, service, dan repository ke dalam package yang berbeda (`controller`, `service`, `repository`). Pemisahan ini akan membuat setiap lapisan memiliki tanggung jawab yang jelas dan mencegah pencampuran logika antar layer, sehingga kode lebih mudah dipahami.

2. **Single Responsibility Principle**, saya memastikan setiap kelas hanya memiliki satu tanggung jawab utama. Controller hanya bertugas menangani request dan response, service menangani logika kode, dan repository menangani penyimpanan serta pengambilan data.

3. **Meaningful Naming**, saya melakukan penamaan kelas dan metode seperti `create`, `findAll`, `findById`, `update`, dan `deleteById`. Penamaan ini bersifat deskriptif sehingga tujuan dan fungsi kode dapat dipahami tanpa perlu membaca implementasinya secara mendetail.

---

## Secure Coding Practices Applied

1. **Avoiding SQL Injection Risks** , Pada tahap ini, repository masih menggunakan in-memory storage sehingga tidak ada interaksi langsung dengan database dan risiko SQL injection dapat dihindari.

2. **Minimalisir Data Exposure** menerapkan view hanya menampilkan data yang benar-benar dibutuhkan oleh interface. Hal ini membantu mengurangi risiko kebocoran data yang tidak perlu.

3. **Post/Redirect/Get Pattern** digunakan setelah operasi yang mengubah data, seperti create, update, dan delete. Dengan melakukan redirect setelah POST, aplikasi dapat mencegah pengiriman ulang data yang sama ketika pengguna melakukan refresh halaman.

---

## Mistakes / Gaps Found and Improvements

1. **Penghapusan data menggunakan metode GET tidak aman** karena dapat memicu penghapusan tidak disengaja atau disalahgunakan. Perbaikan yang disarankan adalah menggunakan metode POST atau DELETE dengan perlindungan CSRF melalui Spring Security.

2. **Validasi input belum diterapkan** pada field seperti `productName` dan `productQuantity`. Perbaikan dapat dilakukan dengan menambahkan anotasi validasi seperti `@NotBlank` dan `@Min(0)` serta menangani error validasi di controller.

3. **Penanganan error masih minim** karena ketika data produk tidak ditemukan, aplikasi hanya melakukan redirect tanpa memberikan informasi kepada pengguna. Perbaikannya adalah dengan menampilkan pesan error atau halaman khusus agar pengguna memahami apa yang terjadi.

4. **Manajemen ID produk masih lemah** karena ID dihasilkan di service tanpa pengecekan keunikan jika ID diberikan secara manual. Perbaikannya adalah dengan selalu menghasilkan ID di sisi server atau melakukan validasi keunikan sebelum menyimpan data.

5. **Tipe input quantity kurang tepat** karena masih menggunakan `type="text"` yang memungkinkan input tidak valid. Perbaikan dapat dilakukan dengan mengganti menjadi `type="number"` dan tetap menambahkan validasi di server side.

