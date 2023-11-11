---
lab:
  title: Menjelajahi layanan Azure AI
---

> **Penting**
> **lab Detektor Anomali telah ditolak dan digantikan oleh pembaruan di bawah ini.**

Layanan Azure AI membantu pengguna membuat aplikasi AI dengan API dan model yang siap pakai dan dapat disesuaikan sebelumnya. Dalam latihan ini Anda akan melihat salah satu layanan, Azure AI Content Brankas ty, di Content Brankas ty Studio. 

Content Brankas ty Studio memungkinkan Anda menjelajahi bagaimana konten teks dan gambar dapat dimoderasi. Anda dapat menjalankan pengujian pada sampel teks atau gambar dan mendapatkan skor tingkat keparahan mulai dari yang aman hingga tinggi untuk setiap kategori. Dalam latihan lab ini Anda akan membuat sumber daya layanan tunggal di Content Brankas ty Studio dan menguji fungsionalitasnya. 

> **Catatan** Tujuan dari latihan ini adalah untuk mendapatkan rasa umum tentang bagaimana layanan Azure AI disediakan dan digunakan. Content Brankas ty digunakan sebagai contoh, tetapi Anda tidak diharapkan untuk mendapatkan pengetahuan komprehensif tentang keamanan konten dalam latihan ini!

## Menavigasi Content Brankas ty Studio 

![Cuplikan layar halaman arahan studio keamanan konten.](./media/content-safety/content-safety-getting-started.png)


1. [Buka Content Brankas ty Studio](https://contentsafety.cognitive.azure.com?azure-portal=true). Jika Anda tidak masuk, Anda harus masuk. Pilih **Masuk** di kanan atas layar. Gunakan email dan kata sandi yang terkait dengan langganan Azure Anda untuk masuk. 

1. Content Brankas ty Studio disiapkan seperti banyak studio lain untuk layanan Azure AI. Pada menu di bagian atas layar, klik ikon di sebelah kiri *Azure AI*. Anda akan melihat daftar drop-down studio lain yang dirancang untuk pengembangan dengan layanan Azure AI. Anda dapat mengklik ikon lagi untuk menyembunyikan daftar.

![Cuplikan layar menu Content Brankas ty Studio dengan pilihan pengalih terbuka untuk beralih ke studio lain.](./media/content-safety/studio-toggle-icon.png)  

## Mengaitkan sumber daya dengan studio 

Sebelum menggunakan studio, Anda perlu mengaitkan sumber daya layanan Azure AI dengan studio. Bergantung pada studio, Anda mungkin menemukan bahwa Anda memerlukan sumber daya layanan tunggal tertentu, atau dapat menggunakan sumber daya multi-layanan umum. Dalam kasus Content Brankas ty Studio, Anda dapat menggunakan layanan dengan membuat sumber daya Content Brankas ty* layanan *tunggal atau *sumber daya multi-layanan umum layanan* Azure AI. Dalam langkah-langkah di bawah ini, kami akan membuat sumber daya Content Brankas ty layanan tunggal. 

1. Di kanan atas layar, klik **ikon Pengaturan**. 

![Cuplikan layar ikon pengaturan di kanan atas layar, di samping bel, tanda tanya, dan ikon senyum.](./media/content-safety/settings-toggle.png)

1. Pada halaman **Pengaturan**, Anda akan melihat *tab Direktori* dan *tab Sumber Daya*. Pada tab *Sumber Daya*, pilih **Buat sumber daya** baru. Ini akan membawa Anda ke halaman untuk membuat sumber daya di Portal Microsoft Azure.

> **Catatan** Tab *Direktori* memungkinkan pengguna untuk memilih direktori yang berbeda untuk membuat sumber daya. Anda tidak perlu mengubah pengaturannya kecuali Anda ingin menggunakan direktori yang berbeda. 

![Cuplikan layar tempat memilih buat sumber daya baru dari halaman pengaturan Content Brankas ty Studio.](./media/content-safety/create-new-resource-from-studio.png)

1. Pada halaman *Buat Konten Brankas ty* di [Portal](https://portal.azure.com?auzre-portal=true) Microsoft Azure, Anda perlu mengonfigurasi beberapa detail untuk membuat sumber daya Anda. Konfigurasikan dengan pengaturan berikut:
    - **Langganan**: *Langganan Azure Anda*.
    - **Grup sumber daya**: *Pilih atau buat grup sumber daya dengan nama unik*.
    - **Wilayah**: *Pilih wilayah yang tersedia*.
    - **Nama**: *Masukkan nama unik*.
    - **Tingkat harga**: Gratis F0

1. Pilih **Tinjau + Buat** dan tinjau konfigurasi. Lalu pilih **Buat**. Layar akan menunjukkan kapan penyebaran selesai. 

*Congrats! Anda baru saja membuat, atau menyediakan sumber daya layanan Azure AI. Yang Anda sediakan khususnya adalah sumber daya layanan Content Brankas ty layanan tunggal.*

1. Setelah penyebaran selesai, buka tab baru dan kembali ke [Content Brankas ty Studio](https://contentsafety.cognitive.azure.com?azure-portal=true). 

1. **Pilih ikon Pengaturan** di kanan atas layar lagi. Kali ini Anda akan melihat bahwa sumber daya yang baru dibuat telah ditambahkan ke daftar.  

1. Pada halaman Pengaturan Content Brankas ty Studio, pilih sumber daya layanan Azure AI yang baru saja Anda buat dan klik **Gunakan sumber daya** di bagian bawah layar. Anda akan dibawa kembali ke halaman beranda studio. Sekarang Anda dapat mulai menggunakan studio dengan sumber daya yang baru dibuat.

## Mencoba moderasi teks di Content Brankas ty Studio

1. Pada beranda Content Brankas ty Studio, di bawah *Jalankan pengujian* moderasi, navigasikan ke kotak **Konten** teks sedang dan klik **Coba**.
1. Di bawah jalankan pengujian sederhana, klik **konten Brankas**. Perhatikan bahwa teks ditampilkan dalam kotak di bawah ini. 
1. Klik **Jalankan pengujian**. Menjalankan pengujian memanggil model pembelajaran mendalam Content Brankas ty Service. Model pembelajaran mendalam telah dilatih untuk mengenali konten yang tidak aman.
1. Di panel *Hasil* , periksa hasilnya. Ada empat tingkat keparahan dari aman ke tinggi, dan empat jenis konten berbahaya. Apakah layanan AI Content Brankas ty menganggap sampel ini dapat diterima atau tidak? Yang penting untuk dicatat adalah bahwa hasilnya berada dalam interval keyakinan. Model yang terlatih dengan baik, seperti salah satu model out-of-the-box Azure AI, dapat mengembalikan hasil yang memiliki probabilitas tinggi untuk mencocokkan apa yang akan dilabeli manusia hasilnya. Setiap kali Anda menjalankan pengujian, Anda memanggil model lagi. 
1. Sekarang coba sampel lain. Pilih teks di bawah Konten kekerasan dengan salah eja. Periksa apakah konten ditampilkan dalam kotak di bawah ini.
1. Klik **Jalankan pengujian** dan periksa hasilnya di panel Hasil lagi. 

Anda dapat menjalankan pengujian pada semua sampel yang disediakan, lalu memeriksa hasilnya.

## Lihat kunci dan titik akhir

Kemampuan yang Anda uji ini dapat diprogram ke dalam semua jenis aplikasi. Kunci dan titik akhir yang digunakan untuk pengembangan aplikasi dapat ditemukan baik di Content Brankas ty Studio maupun Portal Microsoft Azure. 

1. Di Content Brankas ty Studio, navigasi kembali ke **halaman Pengaturan**, dengan tab *Sumber Daya* dipilih. Cari sumber daya yang Anda gunakan. Gulir ke seluruh untuk melihat titik akhir dan kunci untuk sumber daya Anda. 
1. Di Portal Microsoft Azure, Anda akan melihat bahwa ini adalah *titik akhir yang sama* dan *kunci yang berbeda* untuk sumber daya Anda. Untuk memeriksanya, buka [Portal](https://portal.azure.com?auzre-portal=true) Microsoft Azure. Cari *Content Brankas ty* di bilah pencarian atas. Temukan sumber daya Anda dan klik sumber daya tersebut. Di menu sebelah kiri, lihat di bawah *Manajemen* Sumber Daya untuk *Kunci dan Titik* Akhir. Pilih **Kunci dan Titik** Akhir untuk melihat titik akhir dan kunci untuk sumber daya Anda. 

Setelah selesai, Anda dapat menghapus sumber daya Content Brankas ty dari Portal Microsoft Azure. Menghapus sumber daya adalah cara untuk mengurangi biaya yang bertambah ketika sumber daya ada dalam langganan. Untuk melakukan ini, navigasikan **ke halaman Gambaran Umum** sumber daya Content Brankas ty Anda. Pilih **Hapus** di bagian atas layar. 
