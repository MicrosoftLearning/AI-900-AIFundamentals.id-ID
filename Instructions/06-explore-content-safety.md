Content Safety Studio memungkinkan Anda menjelajahi bagaimana konten teks dan gambar dapat dimoderasi. Anda dapat menjalankan pengujian pada sampel teks atau gambar dan mendapatkan skor tingkat keparahan mulai dari aman hingga tinggi untuk setiap kategori. Anda dapat dengan cepat mengetahui cara kerja layanan AI Keamanan Konten, dan kemampuannya. 

Dalam latihan lab ini Anda akan membuat sumber daya Azure AI Services multi-layanan di portal Azure dan memeriksa titik akhir dan kuncinya. Anda kemudian akan menggunakan Content Safety Studio untuk menjelajahi fungsionalitas layanan Content Safety AI. 

## Membuat sumber daya AI Services

1.  Di tab browser lain, buka portal Microsoft Azure di [https://portal.azure.com](https://portal.azure.com?azure-portal=true), masuk dengan akun Microsoft Anda.
1.  Klik tombol **&#65291;Buat sumber daya** dan cari *layanan Azure AI*. Pilih **buat** paket **layanan Azure AI** . Anda akan dibawa ke halaman untuk membuat sumber daya layanan Azure AI. Konfigurasikan dengan pengaturan berikut:
- **Langganan**: Langganan Azure Anda.
- **Grup sumber daya**: Pilih atau buat grup sumber daya yang sesuai.
- **Wilayah**: Pilih wilayah yang tersedia.
- **Nama**: Masukkan nama yang unik.
- **Tingkatan harga**: F0 
2.  Tinjau dan buat sumber daya dan tunggu hingga penyebaran selesai. 

## Mengaitkan sumber daya ke Content Safety Studio 
Di tab browser terpisah, buka Content Safety Studio dan masuk. Layar Memulai ditampilkan.

1.  Klik cog Pengaturan di menu kanan atas.
2.  Pilih sumber daya layanan Azure AI yang baru saja Anda buat, lalu klik Gunakan sumber daya.
3.  Lihat titik akhir dan kunci untuk sumber daya Anda, menggulir jika perlu. Jika Anda memeriksa di Portal Microsoft Azure, Anda akan melihat bahwa ini adalah titik akhir dan kunci untuk sumber daya Anda yang menunjukkan bahwa Anda telah berhasil mengaitkan sumber daya Anda dengan studio Keamanan Konten.
4.  Klik Content Safety Studio untuk kembali ke halaman beranda. Di bawah Konten teks moderasi, klik Cobalah.
5.  Di bawah jalankan pengujian sederhana, klik Konten Aman. Perhatikan bahwa teks ditampilkan dalam kotak di bawah ini. 
6.  Klik Jalankan pengujian. 
7.  Di panel Hasil, periksa hasilnya. Ada empat tingkat keparahan dari aman ke tinggi, dan empat jenis konten berbahaya. Apakah layanan Content Safety AI menganggap sampel ini dapat diterima atau tidak? 
8.  Sekarang coba sampel lain. Pilih teks di bawah Konten kekerasan dengan kesalahan ejaan. Periksa apakah konten ditampilkan dalam kotak di bawah ini.
9.  Klik Jalankan pengujian dan periksa hasilnya di panel Hasil di bawah ini. Apakah sampel ini dapat diterima? Jika tidak, mengapa tidak?

Anda dapat menjalankan pengujian pada semua sampel yang disediakan, lalu memeriksa hasilnya.

Setelah selesai, hapus sumber daya layanan Azure AI di Portal Microsoft Azure. 
