---
lab:
  title: Jelajahi Computer Vision
---

# <a name="explore-computer-vision"></a>Jelajahi Computer Vision

> **Catatan** Untuk menyelesaikan lab ini, Anda memerlukan [langganan Azure](https://azure.microsoft.com/free?azure-portal=true) dengan akses administrator.

Layanan kognitif *Computer Vision* menggunakan model pembelajaran mesin yang dilatih sebelumnya untuk menganalisis gambar dan mengekstrak informasi tentang gambar tersebut.

Contoh, misalkan perusahaan ritel *Northwind Traders* fiktif memutuskan untuk menerapkan "toko pintar", di mana layanan AI memantau toko untuk mengidentifikasi pelanggan yang memerlukan bantuan, dan mengarahkan karyawan untuk membantu mereka. Dengan menggunakan layanan Computer Vision, gambar yang diambil oleh kamera di seluruh toko dapat dianalisis untuk memberikan deskripsi yang berguna tentang yang dilukiskan dalam gambar.

Di lab ini, Anda akan menggunakan aplikasi baris perintah sederhana untuk melihat cara kerja layanan Computer Vision. Prinsip dan fungsi yang sama berlaku dalam solusi dunia nyata, seperti situs web atau aplikasi ponsel.

## <a name="create-a-cognitive-services-resource"></a>Membuat sumber daya *Cognitive Services*

Anda dapat menggunakan layanan Computer Vision dengan membuat sumber daya **Computer Vision** atau sumber daya **Cognitive Services**.

Jika Anda belum melakukannya, buat sumber daya **Cognitive Services** di langganan Azure Anda.

1. Di tab browser lain, buka portal Microsoft Azure di [https://portal.azure.com](https://portal.azure.com?azure-portal=true), masuk dengan akun Microsoft Anda.

1. Klik tombol **&#65291;Buat sumber daya**, cari *Cognitive Services*, dan buat sumber daya **Cognitive Services** dengan pengaturan berikut:
    - **Langganan**: *Langganan Azure Anda*.
    - **Grup sumber daya**: *Pilih atau buat grup sumber daya dengan nama unik*.
    - **Wilayah**: *Pilih wilayah yang tersedia*.
    - **Nama**: *Masukkan nama unik*.
    - **Tingkat harga**: Standar S0
    - **Dengan mencentang kotak ini, saya menyatakan bahwa saya telah membaca dan memahami semua persyaratan di bawah**: Dipilih.

1. Tinjau dan buat sumber daya, dan tunggu hingga penyebaran selesai. Lalu pergi ke sumber daya yang disebarkan.

1. Lihat halaman **Kunci dan Titik Akhir** untuk sumber daya Cognitive Services Anda. Anda akan membutuhkan titik akhir dan kunci untuk terhubung dari aplikasi klien.

## <a name="run-cloud-shell"></a>Menjalankan Cloud Shell

Untuk menguji kemampuan layanan Computer Vision, kita akan menggunakan aplikasi baris perintah sederhana yang berjalan di Cloud Shell di Azure.

1. Di portal Azure, pilih tombol **[>_]** (*Cloud Shell*) di bagian atas halaman di sebelah kanan kotak pencarian. Tindakan ini akan membuka panel Cloud Shell di bagian bawah portal.

    ![Mulai Cloud Shell dengan mengeklik ikon di sebelah kanan kotak pencarian di atas](media/analyze-images-computer-vision-service/powershell-portal-guide-1.png)

1. Saat pertama kali membuka Cloud Shell, Anda mungkin diminta untuk memilih jenis shell yang ingin digunakan (*Bash* atau *PowerShell*). Pilih **PowerShell**. Jika Anda tidak melihat opsi ini, lewati langkah ini.  

1. Jika Anda diminta membuat penyimpanan untuk Cloud Shell, pastikan langganan ditentukan dan pilih **Buat penyimpanan**. Kemudian tunggu sekitar satu menit hingga penyimpanan dibuat.

    ![Buat penyimpanan dengan mengklik konfirmasi.](media/analyze-images-computer-vision-service/powershell-portal-guide-2.png)

1. Pastikan jenis shell yang ditunjukkan di kiri atas panel Cloud Shell dialihkan ke *PowerShell*. Jika *Bash*, alihkan ke *PowerShell* dengan menggunakan menu drop-down.

    ![Cara menemukan menu drop down sebelah kiri untuk beralih ke PowerShell](media/analyze-images-computer-vision-service/powershell-portal-guide-3.png)

1. Tunggu PowerShell untuk memulai. Anda akan melihat layar berikut di portal Microsoft Azure:  

    ![Tunggu PowerShell untuk memulai.](media/analyze-images-computer-vision-service/powershell-prompt.png)

## <a name="configure-and-run-a-client-application"></a>Mengonfigurasi dan menjalankan aplikasi klien

Setelah memiliki lingkungan Cloud Shell, Anda dapat menjalankan aplikasi sederhana yang menggunakan layanan Computer Vision untuk menganalisis gambar.

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

    ![Editor kode.](media/analyze-images-computer-vision-service/powershell-portal-guide-4.png)

1. Di panel **File** di sebelah kiri, perluas **ai-900** dan pilih **analyze-image.ps1**. File ini berisi beberapa kode yang menggunakan layanan Computer Vision untuk menganalisis gambar, seperti yang ditunjukkan di sini:

    ![Editor yang berisi kode untuk menganalisis gambar](media/analyze-images-computer-vision-service/analyze-image-code.png)

1. Jangan terlalu khawatir dengan kode, yang penting adalah bahwa kode ini memerlukan URL titik akhir dan salah satu kunci untuk sumber daya Cognitive Services Anda. Salin ini dari halaman **Kunci dan Titik Akhir** untuk sumber daya Anda dari portal Azure dan tempelkan ke dalam editor kode, dengan mengganti nilai tempat penampung **YOUR_KEY** dan **YOUR_ENDPOINT** masing-masing.

    > **Tips** Anda mungkin perlu menggunakan bilah pemisah untuk menyesuaikan area layar saat bekerja dengan panel **Tombol dan Titik Akhir** serta **Editor**.

    Setelah menempelkan nilai kunci dan titik akhir, dua baris pertama dari kode akan terlihat seperti ini:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. Di kanan atas panel editor, gunakan tombol **...** untuk membuka menu dan pilih **Simpan** untuk menyimpan perubahan Anda.

    Aplikasi klien sampel akan menggunakan layanan Computer Vision untuk menganalisis gambar berikut, yang diambil oleh kamera di toko Northwind Traders:

    ![Gambar orang tua yang menggunakan kamera ponsel untuk mengambil gambar anak di toko](media/analyze-images-computer-vision-service/store-camera-1.jpg)

1. Di panel PowerShell, masukkan perintah berikut untuk menjalankan kode:

    ```PowerShell
    cd ai-900
    ./analyze-image.ps1 store-camera-1.jpg
    ```

1. Tinjau hasil analisis gambar, yang meliputi:
    - Saran keterangan yang menjelaskan gambar.
    - Daftar objek yang diidentifikasi dalam gambar.
    - Daftar "tag" yang relevan dengan gambar.

1. Sekarang mari kita coba gambar lain:

    ![Gambar orang dengan keranjang belanja di supermarket](media/analyze-images-computer-vision-service/store-camera-2.jpg)

    Untuk menganalisis gambar kedua, masukkan perintah berikut:

    ```PowerShell
    ./analyze-image.ps1 store-camera-2.jpg
    ```

1. Tinjau hasil analisis gambar untuk gambar kedua.

1. Mari coba sekali lagi:

    ![Gambar orang dengan troli belanja](media/analyze-images-computer-vision-service/store-camera-3.jpg)

    Untuk menganalisis gambar ketiga, masukkan perintah berikut:

    ```PowerShell
    ./analyze-image.ps1 store-camera-3.jpg
    ```

1. Tinjau hasil analisis gambar untuk gambar ketiga.

## <a name="learn-more"></a>Pelajari lebih lanjut

Aplikasi sederhana ini hanya menunjukkan beberapa kemampuan layanan Computer Vision. Untuk mempelajari lebih lanjut tindakan yang dapat Anda lakukan dengan layanan ini, lihat [Halaman Computer Vision](https://azure.microsoft.com/services/cognitive-services/computer-vision/).
