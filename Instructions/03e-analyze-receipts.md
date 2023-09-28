---
lab:
  title: Menjelajahi pengenalan formulir
---

# Menjelajahi pengenalan formulir

> **Catatan** Untuk menyelesaikan lab ini, Anda memerlukan [langganan Azure](https://azure.microsoft.com/free?azure-portal=true) dengan akses administrator.

Dalam bidang kecerdasan buatan (AI) visi komputer, pengenalan karakter optik (OCR) umumnya digunakan untuk membaca dokumen cetak atau tulisan tangan. Teks sering kali hanya diekstraksi dari dokumen ke dalam format yang dapat digunakan untuk pemrosesan atau analisis lebih lanjut.

Skenario OCR yang lebih canggih adalah ekstraksi informasi dari formulir, seperti pesanan pembelian atau faktur, dengan pemahaman semantik tentang apa yang diwakili oleh bidang dalam formulir. Layanan **Form Recognizer** dirancang khusus untuk masalah AI semacam ini.

Form Recognizer menggunakan model pembelajaran mesin yang dilatih untuk mengekstrak teks dari gambar faktur, tanda terima, dan sebagainya. Sementara model visi komputer lainnya dapat mengambil teks, Form Recognizer juga mengambil struktur teks, seperti pasangan kunci/nilai dan informasi dalam tabel. Dengan cara ini, sebagai alternatif mengetik entri secara manual dari formulir ke dalam database, Anda dapat secara otomatis mengambil hubungan antara teks dari file asli. 

Untuk menguji kemampuan layanan Form Recognizer, kita akan menggunakan aplikasi baris perintah sederhana yang dijalankan di Cloud Shell. Prinsip dan fungsi yang sama berlaku dalam solusi dunia nyata, seperti situs web atau aplikasi ponsel.

## Membuat sumber daya *layanan Azure AI*

Anda dapat menggunakan layanan Form Recognizer dengan membuat sumber daya **Form Recognizer** atau sumber daya **layanan Azure AI**.

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

Untuk menguji kemampuan layanan Form Recognizer, kita akan menggunakan aplikasi baris perintah sederhana yang dijalankan di Cloud Shell di Azure. 

1. Di portal Azure, pilih tombol **[>_]** (*Cloud Shell*) di bagian atas halaman di sebelah kanan kotak pencarian. Tindakan ini akan membuka panel Cloud Shell di bagian bawah portal. 

    ![Mulai Cloud Shell dengan mengeklik ikon di sebelah kanan kotak pencarian di atas](media/analyze-receipts/powershell-portal-guide-1.png)

1. Saat pertama kali membuka Cloud Shell, Anda mungkin diminta untuk memilih jenis shell yang ingin digunakan (*Bash* atau *PowerShell*). Pilih **PowerShell**. Jika Anda tidak melihat opsi ini, lewati langkah ini.  

1. Jika Anda diminta membuat penyimpanan untuk Cloud Shell, pastikan langganan ditentukan dan pilih **Buat penyimpanan**. Kemudian tunggu sekitar satu menit hingga penyimpanan dibuat.

    ![Buat penyimpanan dengan mengklik konfirmasi.](media/analyze-receipts/powershell-portal-guide-2.png)

1. Pastikan jenis shell yang ditunjukkan di kiri atas panel Cloud Shell dialihkan ke *PowerShell*. Jika *Bash*, alihkan ke *PowerShell* dengan menggunakan menu drop-down.

    ![Cara menemukan menu drop down sebelah kiri untuk beralih ke PowerShell](media/analyze-receipts/powershell-portal-guide-3.png) 

1. Tunggu PowerShell untuk memulai. Anda akan melihat layar berikut di portal Microsoft Azure:  

    ![Tunggu PowerShell untuk memulai.](media/analyze-receipts/powershell-prompt.png) 

## Mengonfigurasi dan menjalankan aplikasi klien

Setelah memiliki model kustom, Anda dapat menjalankan aplikasi klien sederhana yang menggunakan layanan Form Recognizer.

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

    ![Editor kode.](media/analyze-receipts/powershell-portal-guide-4.png)

1. Di panel **File** di sebelah kiri, perluas **ai-900** dan pilih **form-recognizer.ps1**. File ini berisi beberapa kode yang menggunakan layanan Form Recognizer untuk menganalisis bidang dalam tanda terima, seperti yang ditunjukkan di sini:

    ![Editor yang berisi kode untuk menganalisis bidang dalam tanda terima.](media/analyze-receipts/recognize-receipt-code.png)

1. Jangan terlalu khawatir tentang detail kode, yang penting adalah bahwa kode memerlukan URL titik akhir dan salah satu kunci untuk sumber daya layanan Azure AI Anda. Salin ini dari halaman **Kunci dan Titik Akhir** untuk sumber daya Anda dari portal Azure dan tempelkan ke dalam editor kode, dengan mengganti nilai tempat penampung **YOUR_KEY** dan **YOUR_ENDPOINT** masing-masing.

    > **Tips** Anda mungkin perlu menggunakan bilah pemisah untuk menyesuaikan area layar saat bekerja dengan panel **Tombol dan Titik Akhir** serta **Editor**.

    Setelah menempelkan nilai kunci dan titik akhir, dua baris pertama dari kode akan terlihat seperti ini:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. Di kanan atas panel editor, gunakan tombol **...** untuk membuka menu dan pilih **Simpan** untuk menyimpan perubahan Anda. Kemudian, buka menu lagi dan pilih **Tutup Editor**. Setelah menyiapkan kunci dan titik akhir, Anda dapat menggunakan sumber daya untuk menganalisis bidang dari tanda terima. Dalam hal ini, Anda akan menggunakan model bawaan Form Recognizer untuk menganalisis tanda terima untuk perusahaan ritel Northwind Traders fiksi.

    Contoh aplikasi klien akan menganalisis gambar berikut:

    ![Ini adalah gambar tanda terima.](media/analyze-receipts/receipt.jpg)

1. Di panel PowerShell, masukkan perintah berikut untuk menjalankan kode untuk membaca teks:

    ```PowerShell
    cd ai-900
    ./form-recognizer.ps1
    ```

1. Tinjau hasil yang ditampilkan. Perhatikan bahwa Form Recognizer dapat menafsirkan data dalam formulir, mengidentifikasi alamat pedagang dan nomor telepon dengan benar, serta tanggal dan waktu transaksi, serta item baris, subtotal, pajak, dan jumlah total.

## Pelajari lebih lanjut

Aplikasi sederhana ini hanya menunjukkan beberapa kemampuan Form Recognizer layanan Computer Vision. Untuk mempelajari lebih lanjut tindakan yang dapat Anda lakukan dengan layanan ini, lihat [halaman Form Recognizer](https://docs.microsoft.com/azure/applied-ai-services/form-recognizer/overview).
