---
lab:
  title: Menjelajahi pengenalan wajah
---

# Menjelajahi pengenalan wajah

> **Catatan** Untuk menyelesaikan lab ini, Anda memerlukan [langganan Azure](https://azure.microsoft.com/free?azure-portal=true) dengan akses administrator.

Solusi visi komputer seringkali membutuhkan solusi kecerdasan buatan (AI) untuk mendeteksi wajah manusia. Misalnya, perusahaan ritel Northwind Traders ingin menemukan pelanggan yang ada di toko untuk membantu pelanggan dengan baik. Salah satu cara untuk mencapainya adalah dengan menentukan apakah ada wajah dalam gambar, dan jika demikian, lalu mengidentifikasi koordinat kotak pembatas di sekitar wajah.

Untuk menguji kemampuan layanan Face, kita akan menggunakan aplikasi baris perintah sederhana yang dijalankan di Cloud Shell. Prinsip dan fungsi yang sama berlaku dalam solusi dunia nyata, seperti situs web atau aplikasi ponsel.

## Membuat sumber daya *Face API*

Anda dapat menggunakan layanan Face dengan membuat sumber daya **Face** .

Jika Anda belum melakukannya, buat sumber daya **Face API** di langganan Azure Anda.

1. Di tab browser lain, buka portal Microsoft Azure di [https://portal.azure.com](https://portal.azure.com?azure-portal=true), masuk dengan akun Microsoft Anda.

1. Klik tombol **&#65291;Buat sumber daya** , cari *Face*, dan buat sumber daya **Face** dengan pengaturan berikut:
    - **Langganan**: *Langganan Azure Anda*.
    - **Grup sumber daya**: *Pilih atau buat grup sumber daya dengan nama unik*.
    - **Wilayah**: *Pilih wilayah yang tersedia*.
    - **Nama**: *Masukkan nama unik*.
    - **Tingkat harga**: Gratis F0

1. Tinjau dan buat sumber daya, dan tunggu hingga penyebaran selesai. Lalu pergi ke sumber daya yang disebarkan.

1. Lihat halaman **Kunci dan Titik Akhir** untuk sumber daya Wajah Anda. Anda akan membutuhkan titik akhir dan kunci untuk terhubung dari aplikasi klien.

## Menjalankan Cloud Shell

Untuk menguji kemampuan layanan Face, kita akan menggunakan aplikasi baris perintah sederhana yang dijalankan di Cloud Shell di Azure. 

1. Di portal Azure, pilih tombol **[>_]** (*Cloud Shell*) di bagian atas halaman di sebelah kanan kotak pencarian. Tindakan ini akan membuka panel Cloud Shell di bagian bawah portal. 

    ![Mulai Cloud Shell dengan mengeklik ikon di sebelah kanan kotak pencarian di atas](media/create-face-solutions/powershell-portal-guide-1.png)

1. Saat pertama kali membuka Cloud Shell, Anda mungkin diminta untuk memilih jenis shell yang ingin digunakan (*Bash* atau *PowerShell*). Pilih **PowerShell**. Jika Anda tidak melihat opsi ini, lewati langkah ini.  

1. Jika Anda diminta membuat penyimpanan untuk Cloud Shell, pastikan langganan ditentukan dan pilih **Buat penyimpanan**. Kemudian tunggu sekitar satu menit hingga penyimpanan dibuat.

    ![Buat penyimpanan dengan mengklik konfirmasi.](media/create-face-solutions/powershell-portal-guide-2.png)       

1. Pastikan jenis shell yang ditunjukkan di kiri atas panel Cloud Shell dialihkan ke *PowerShell*. Jika *Bash*, alihkan ke *PowerShell* dengan menggunakan menu drop-down.

    ![Cara menemukan menu drop down sebelah kiri untuk beralih ke PowerShell](media/create-face-solutions/powershell-portal-guide-3.png) 

1. Tunggu PowerShell untuk memulai. Anda akan melihat layar berikut di portal Microsoft Azure:  

    ![Tunggu PowerShell untuk memulai.](media/create-face-solutions/powershell-prompt.png)

## Mengonfigurasi dan menjalankan aplikasi klien

Setelah memiliki model kustom, Anda dapat menjalankan aplikasi klien sederhana yang menggunakan layanan Face.

1. Di shell perintah, masukkan perintah berikut untuk mengunduh contoh aplikasi dan menyimpannya ke folder yang disebut ai-900.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

    > **Tips** Jika Anda telah menggunakan perintah ini di lab lain untuk menggandakan penyimpanan *ai-900*, Anda dapat melewati langkah ini.

1. File diunduh ke folder bernama **ai-900**. Sekarang kami ingin melihat semua file di penyimpanan Cloud Shell Anda dan menggunakannya. Ketik perintah berikut ke dalam shell:

     ```PowerShell
    code .
    ```

    Perhatikan bagaimana perintah ini membuka penyunting seperti pada gambar di bawah ini: 

    ![Editor kode.](media/create-face-solutions/powershell-portal-guide-4.png) 

1. Di panel **File** di sebelah kiri, perluas **ai-900** dan pilih **find-faces.ps1**. File ini berisi beberapa kode yang menggunakan layanan Face untuk mendeteksi dan menganalisis wajah dalam gambar, seperti yang ditunjukkan di sini:

    ![Editor yang berisi kode untuk mendeteksi wajah dalam gambar](media/create-face-solutions/find-faces-code.png)

1. Jangan terlalu khawatir tentang detail kode, yang penting adalah bahwa ia membutuhkan URL titik akhir dan salah satu kunci untuk sumber daya Face Anda. Salin ini dari halaman **Kunci dan Titik Akhir** untuk sumber daya Anda dari portal Azure dan tempelkan ke dalam editor kode, dengan mengganti nilai tempat penampung **YOUR_KEY** dan **YOUR_ENDPOINT** masing-masing.

    > **Tips** Anda mungkin perlu menggunakan bilah pemisah untuk menyesuaikan area layar saat bekerja dengan panel **Tombol dan Titik Akhir** serta **Editor**.

    Setelah menempelkan nilai kunci dan titik akhir, dua baris pertama dari kode akan terlihat seperti ini:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. Di kanan atas panel editor, gunakan tombol **...** untuk membuka menu dan pilih **Simpan** untuk menyimpan perubahan Anda. Kemudian, buka menu lagi dan pilih **Tutup Editor**.

    Contoh aplikasi klien akan menggunakan layanan Face Anda untuk menganalisis gambar berikut, yang diambil dari kamera di toko Northwind Traders:

    ![Gambar orang tua yang menggunakan kamera ponsel untuk mengambil gambar anak di toko](media/create-face-solutions/store-camera-1.jpg)

1. Di panel PowerShell, masukkan perintah berikut untuk menjalankan kode:

    ```PowerShell
    cd ai-900
    ./find-faces.ps1 store-camera-1.jpg
    ```

1. Meninjau informasi yang ditampilkan, yang mencakup lokasi wajah dalam gambar. Lokasi wajah ditunjukkan oleh koordinat kiri atas, serta lebar dan tinggi *kotak pembatas*, seperti yang ditunjukkan di sini:

    ![Gambar seseorang dengan skema wajah](media/create-face-solutions/store-camera-1-face.jpg)

    >**Catatan** Kemampuan layanan wajah yang menampilkan fitur pengenal pribadi dibatasi. Lihat https://azure.microsoft.com/blog/responsible-ai-investments-and-safeguards-for-facial-recognition/ untuk detailnya.

1. Sekarang mari kita coba gambar lain:

    ![Gambar orang dengan keranjang belanja](media/create-face-solutions/store-camera-2.jpg)

    Untuk menganalisis gambar kedua, masukkan perintah berikut:

    ```PowerShell
    ./find-faces.ps1 store-camera-2.jpg
    ```

1. Tinjau hasil analisis wajah untuk gambar kedua.

1. Mari coba sekali lagi:

    ![Gambar orang dengan troli belanja](media/create-face-solutions/store-camera-3.jpg)

    Untuk menganalisis gambar ketiga, masukkan perintah berikut:

    ```PowerShell
    ./find-faces.ps1 store-camera-3.jpg
    ```

1. Tinjau hasil analisis wajah untuk gambar ketiga.

## Pelajari lebih lanjut

Aplikasi sederhana ini hanya menunjukkan beberapa kemampuan layanan Face. Untuk mempelajari lebih lanjut tentang apa yang dapat Anda lakukan dengan layanan ini, lihat [halaman Face API](https://azure.microsoft.com/en-us/products/cognitive-services/vision-services).
