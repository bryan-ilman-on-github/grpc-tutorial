# Refleksi

1. **Unary RPC**: Metode ini melibatkan panggilan one-to-one antara klien dan server. Klien mengirim permintaan dan menunggu respons dari server. Cocok untuk operasi sederhana seperti membaca data. **Server Streaming**: Klien mengirim permintaan, dan server mengirim respons berupa aliran data. Cocok untuk situasi di mana server perlu mengirim banyak data ke klien, seperti streaming video. **Bi-Directional Streaming RPC**: Klien dan server dapat mengirim data secara bersamaan. Cocok untuk aplikasi chat atau sinkronisasi data secara real-time.
2. Beberapa pertimbangan keamanan saat mengimplementasikan layanan gRPC di Rust:
   - **Otentikasi**: Pastikan hanya pengguna yang sah yang dapat mengakses layanan dengan menggunakan mekanisme otentikasi seperti token atau sertifikat.
   - **Otorisasi**: Tentukan hak akses berdasarkan peran pengguna untuk mengontrol akses ke fitur tertentu.
   - **Enkripsi Data**: Gunakan protokol aman seperti TLS untuk mengenkripsi data yang dikirim melalui jaringan.
3. Tantangan dalam mengelola bidirectional streaming di gRPC Rust, terutama dalam aplikasi chat, meliputi:

   - **Sinkronisasi**: Memastikan pesan tiba dan dikelola dengan benar di kedua sisi.
   - **Kinerja**: Memastikan efisiensi dan responsivitas dalam mengirim dan menerima data secara real-time.
   - **Penanganan Kesalahan**: Mengatasi situasi ketika koneksi terputus atau ada kesalahan dalam aliran data.

4. Keuntungan menggunakan `tokio_stream::wrappers::ReceiverStream` adalah kemudahan dalam mengintegrasikan Rust gRPC dengan sistem streaming. Namun, kelemahannya adalah ketergantungan pada tokio runtime yang dapat membatasi fleksibilitas dalam penggunaannya.
5. Untuk meningkatkan code maintainability, langkah-langkah berikut bisa dilakukan:

   - **Pembagian Fungsionalitas**: Pisahkan fungsionalitas ke dalam modul-modul yang terpisah berdasarkan tanggung jawab masing-masing, seperti otentikasi, otorisasi, penanganan kesalahan, dan logika bisnis.
   - **Abstraksi**: Gunakan abstraksi untuk memisahkan implementasi dari interface. Ini dapat berupa penggunaan trait untuk mendefinisikan perilaku umum yang dapat diimplementasikan oleh berbagai komponen.
   - **Pengelolaan dependencies**: Gunakan manajemen dependensi yang baik untuk mengelola ketergantungan antar-modul dengan benar. Ini dapat dilakukan dengan memanfaatkan sistem paket seperti Cargo untuk mengelola dependensi eksternal.
   - **Unit Testing**: Tulis unit test yang komprehensif untuk memastikan bahwa setiap komponen berfungsi seperti yang diharapkan. Ini membantu memastikan bahwa perubahan pada satu bagian tidak memengaruhi yang lain.

6. Untuk menangani logika pemrosesan pembayaran yang lebih kompleks, beberapa langkah tambahan yang mungkin diperlukan adalah:

   - **Validasi Input**: Periksa keabsahan data masukan, seperti memeriksa apakah jumlah pembayaran valid dan apakah informasi kartu kredit valid.

   - **Penanganan Kesalahan**: Tangani kasus-kasus kesalahan yang mungkin terjadi selama pemrosesan, seperti pembayaran ditolak atau gangguan jaringan.

   - **Logging**: Catat kegiatan dan kejadian penting selama pemrosesan pembayaran untuk tujuan audit dan pemecahan masalah.

   - **Optimasi Kinerja**: Optimalkan kinerja kode, terutama jika ada operasi yang memerlukan waktu lama atau membutuhkan sumber daya yang signifikan.

   - **Testing**: Uji coba secara menyeluruh untuk memastikan bahwa semua kasus penggunaan, termasuk skenario ekstrem, ditangani dengan benar.

7. Penggunaan gRPC sebagai protokol komunikasi dalam sistem terdistribusi memiliki dampak yang signifikan pada arsitektur dan desain keseluruhan. Berikut adalah kelebihannya:

   - **Efisiensi Komunikasi**: gRPC menggunakan Protokol Buffer Google (protobuf) untuk serialisasi data, yang menghasilkan payload yang lebih kecil dan efisien dibandingkan dengan format lain seperti JSON.

   Namun, penggunaan gRPC juga memiliki beberapa keterbatasan:

   - **Keterbatasan Interoperabilitas**: Meskipun gRPC mendukung berbagai bahasa pemrograman, integrasi dengan teknologi dan platform yang tidak mendukung gRPC dapat menjadi tantangan. Hal ini dapat mempersulit integrasi dengan sistem yang menggunakan protokol komunikasi yang berbeda.

   - **Ketergantungan pada Protokol Buffer**: Meskipun protobuf adalah format serialisasi yang efisien, beberapa organisasi mungkin memiliki preferensi atau kebutuhan untuk menggunakan format lain seperti JSON atau XML. Ini memerlukan transformasi data tambahan yang dapat menambah kompleksitas dan overhead.

8. - kelebihan HTTP/2:

     - **Prioritasi Stream**: Memungkinkan server untuk memberi prioritas pada stream data yang lebih penting.
     - **Multiplexing**: HTTP/2 memungkinkan banyak permintaan dan respons berjalan secara bersamaan pada satu koneksi, mengatasi masalah “head-of-line blocking” di HTTP/1.1.
     - **Kompressi Header**: HTTP/2 mengurangi overhead dengan mengompresi header permintaan dan respons.

   - Kekurangan HTTP/2:
     - **Keterbatasan Browser**: Implementasi HTTP/2 di browser masih terbatas, terutama untuk JavaScript.

9. - REST API dengan model request-response:

     - **Komunikasi Sekuensial**: Klien membuat permintaan dan server memberikan respons. Komunikasi berlangsung secara sekuensial, di mana klien harus menunggu hingga respons diterima sebelum membuat permintaan berikutnya.
     - **Tidak Real-time**: REST API cenderung kurang responsif dalam komunikasi real-time karena klien harus secara aktif membuat permintaan untuk mendapatkan pembaruan dari server.

   - gRPC dengan streaming dua arah:

     - **Komunikasi Real-time**: gRPC mendukung streaming dua arah, di mana klien dan server dapat saling bertukar pesan secara simultan dan independen. Hal ini memungkinkan komunikasi real-time yang responsif dan interaktif antara klien dan server.
     - **Responsif**: Dengan streaming dua arah, gRPC memungkinkan responsifitas yang tinggi di mana server dapat langsung mengirim pembaruan atau tanggapan kepada klien tanpa menunggu permintaan klien.

10. gRPC menggunakan pendekatan schema-based dengan Protocol Buffers, sementara REST API menggunakan JSON yang lebih fleksibel dan tanpa skema. Berikut adalah perbandingannya:

- **gRPC**:
  - Keuntungan: Protocol Buffers dapat memberikan ukuran payload yang lebih kecil, Protocol Buffers melakukan validasi otomatis terhadap pesan berdasarkan skema sehingga menghindari kesalahan format data selama komunikasi.
  - Kekurangan: Protocol Buffers membutuhkan definisi skema yang ketat sebelum penggunaan.
- **REST API**:
  - Keuntungan: Fleksibilitas dan kemudahan penggunaan.
  - Kekurangan: Tidak ada skema baku.
