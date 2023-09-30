---
lab:
  title: Menjelajahi pengenalan karakter optik
---

# Menjelajahi pengenalan karakter optik

> **Catatan** Untuk menyelesaikan lab ini, Anda memerlukan [langganan Azure](https://azure.microsoft.com/free?azure-portal=true) dengan akses administrator.

Tantangan visi komputer yang umum adalah mendeteksi dan menafsirkan teks dalam gambar. Jenis pemrosesan ini sering disebut sebagai *pengenalan karakter optik* (OCR). Read API Microsoft menyediakan akses ke kemampuan OCR. 

Untuk menguji kemampuan Read API, kami akan menggunakan aplikasi command line sederhana yang berjalan di Cloud Shell. Prinsip dan fungsi yang sama berlaku dalam solusi dunia nyata, seperti situs web atau aplikasi ponsel.

## Menggunakan layanan Azure AI Vision untuk Membaca Teks dalam Gambar

Layanan **Azure AI Vision** menyediakan dukungan untuk tugas OCR, termasuk:

- **Read** API yang dioptimalkan untuk dokumen lebih besar. API ini digunakan secara asinkron, dan dapat digunakan untuk teks cetak dan tulis.

## Membuat sumber daya *layanan Azure AI*

Anda dapat menggunakan layanan Azure AI Vision dengan membuat sumber daya **Computer Vision** atau sumber daya **layanan Azure AI** .

Jika Anda belum melakukannya, buat sumber daya **layanan Azure AI** di langganan Azure Anda.

1. Di tab browser lain, buka portal Microsoft Azure di [https://portal.azure.com](https://portal.azure.com?azure-portal=true), masuk dengan akun Microsoft Anda.

1. Klik tombol **&#65291;Buat sumber daya** dan cari *layanan Azure AI*. Pilih **buat** paket **layanan Azure AI** . Anda akan dibawa ke halaman untuk membuat sumber daya layanan Azure AI. Konfigurasikan dengan pengaturan berikut:
    - **Langganan**: *Langganan Azure Anda*.
    - **Grup sumber daya**: *Pilih atau buat grup sumber daya dengan nama unik*.
    - **Wilayah**: *Pilih wilayah yang tersedia*.
    - **Nama**: *Masukkan nama unik*.
    - **Tingkat harga**: Standar S0
    - **Dengan mencentang kotak ini, saya menyatakan bahwa saya telah membaca dan memahami semua persyaratan di bawah**: Dipilih.

1. Tinjau dan buat sumber daya, dan tunggu hingga penyebaran selesai. Lalu pergi ke sumber daya yang disebarkan.

1. Lihat halaman **Kunci dan Titik Akhir** untuk sumber daya layanan Azure AI Anda. Anda akan membutuhkan titik akhir dan kunci untuk terhubung dari aplikasi klien.

## Menjalankan Cloud Shell

Untuk menguji kemampuan layanan Custom Vision, kami akan menggunakan aplikasi baris perintah sederhana yang berjalan di Cloud Shell di Azure.

1. Di portal Microsoft Azure, pilih tombol **[>_]** (*Cloud Shell*) di bagian atas halaman di sebelah kanan kotak pencarian. Tindakan ini akan membuka panel Cloud Shell di bagian bawah portal. 

    ![Mulai Cloud Shell dengan mengeklik ikon di sebelah kanan kotak pencarian di atas](media/read-text-computer-vision/powershell-portal-guide-1.png)

1. Saat pertama kali membuka Cloud Shell, Anda mungkin diminta untuk memilih jenis shell yang ingin digunakan (*Bash* atau *PowerShell*). Pilih **PowerShell**. Jika Anda tidak melihat opsi ini, lewati langkah ini.  

1. Jika Anda diminta membuat penyimpanan untuk Cloud Shell, pastikan langganan ditentukan dan pilih **Buat penyimpanan**. Kemudian tunggu sekitar satu menit hingga penyimpanan dibuat.

    ![Buat penyimpanan dengan mengklik konfirmasi.](media/read-text-computer-vision/powershell-portal-guide-2.png)

1. Pastikan jenis shell yang ditunjukkan di kiri atas panel Cloud Shell dialihkan ke *PowerShell*. Jika *Bash*, alihkan ke *PowerShell* dengan menggunakan menu drop-down.

    ![Cara menemukan menu drop down sebelah kiri untuk beralih ke PowerShell](media/read-text-computer-vision/powershell-portal-guide-3.png) 

1. Tunggu PowerShell untuk memulai. Anda akan melihat layar berikut di portal Microsoft Azure:  

    ![Tunggu PowerShell untuk memulai.](media/read-text-computer-vision/powershell-prompt.png) 

## Mengonfigurasi dan menjalankan aplikasi klien

Setelah memiliki model kustom, Anda dapat menjalankan aplikasi klien sederhana yang menggunakan layanan OCR.

1. Di shell perintah, masukkan perintah berikut untuk mengunduh contoh aplikasi dan menyimpannya ke folder yang disebut ai-900.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

    >**Tips** Jika Anda telah menggunakan perintah ini di lab lain untuk menggandakan penyimpanan *ai-900*, Anda dapat melewati langkah ini.

1. File diunduh ke folder bernama **ai-900**. Sekarang kami ingin melihat semua file di penyimpanan Cloud Shell Anda dan menggunakannya. Ketik perintah berikut ke dalam shell:

    ```PowerShell
    code .
    ```

    Perhatikan bagaimana perintah ini membuka penyunting seperti pada gambar di bawah ini: 

    ![Editor kode.](media/read-text-computer-vision/powershell-portal-guide-4.png)

1. Di panel **File** di sebelah kiri, perluas **ai-900** dan pilih **ocr.ps1**. File ini berisi beberapa kode yang menggunakan layanan Computer Vision untuk mendeteksi dan menganalisis teks dalam gambar, seperti yang ditunjukkan di sini:

    ![Editor berisi kode untuk menganalisis teks dalam gambar.](media/read-text-computer-vision/ocr-code.png)

1. Jangan terlalu khawatir tentang detail kode, yang penting adalah memerlukan URL titik akhir dan salah satu kunci untuk sumber daya layanan Azure AI Anda. Salin ini dari halaman **Kunci dan Titik Akhir** untuk sumber daya Anda dari portal Azure dan tempelkan ke dalam editor kode, dengan mengganti nilai tempat penampung **YOUR_KEY** dan **YOUR_ENDPOINT** masing-masing.

    > **Tips** Anda mungkin perlu menggunakan bilah pemisah untuk menyesuaikan area layar saat bekerja dengan panel **Tombol dan Titik Akhir** serta **Editor**.

    Setelah menempelkan nilai kunci dan titik akhir, dua baris pertama dari kode akan terlihat seperti ini:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. Di kanan atas panel editor, gunakan tombol **...** untuk membuka menu dan pilih **Simpan** untuk menyimpan perubahan Anda. Kemudian, buka menu lagi dan pilih **Tutup Editor**. Setelah menyiapkan kunci dan titik akhir, Anda dapat menggunakan sumber daya layanan Azure AI untuk mengekstrak teks dari gambar.

    Mari gunakan **Read** API. Dalam hal ini, Anda memiliki gambar iklan untuk perusahaan ritel Northwind Traders fiktif yang berisi beberapa teks.

    Contoh aplikasi klien akan menganalisis gambar berikut:

    ![Gambar iklan untuk toko kelontong Northwind Traders.](media/read-text-computer-vision/advert.jpg)

1. Di panel PowerShell, masukkan perintah berikut untuk menjalankan kode untuk membaca teks:

    ```PowerShell
    cd ai-900
    ./ocr.ps1 advert.jpg
    ```

1. Tinjau detail yang ditemukan dalam gambar. Teks yang ditemukan dalam gambar disusun dalam struktur hierarkis wilayah, garis, dan kata-kata, serta kode membacanya untuk mengambil hasilnya.

    Perhatikan bahwa lokasi teks ditunjukkan oleh koordinat kiri atas, dan lebar serta tinggi *kotak pembatas*, seperti yang ditunjukkan di sini:

    ![Gambar teks dalam gambar yang diuraikan](media/read-text-computer-vision/lab-05-bounding-boxes.png)

1. Sekarang mari kita coba gambar lain:

    ![Gambar dari huruf ketik.](media/read-text-computer-vision/letter.jpg)

    Untuk menganalisis gambar kedua, masukkan perintah berikut:

    ```PowerShell
    ./ocr.ps1 letter.jpg
    ```

1. Tinjau hasil analisis untuk gambar kedua. Ini juga akan menampilkan teks dan kotak pembatas teks.

## Pelajari lebih lanjut

Aplikasi sederhana ini hanya menunjukkan beberapa kemampuan OCR dari layanan Computer Vision. Untuk mempelajari lebih lanjut tindakan yang dapat Anda lakukan dengan layanan ini, lihat [Halaman OCR](https://docs.microsoft.com/azure/cognitive-services/computer-vision/overview-ocr).
