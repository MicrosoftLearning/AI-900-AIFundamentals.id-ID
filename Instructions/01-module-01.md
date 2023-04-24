---
lab:
  title: Jelajahi Cognitive Services
  module: Module 1 - Introduction to AI
---

# <a name="explore-cognitive-services"></a>Jelajahi Cognitive Services

> **Catatan** Untuk menyelesaikan lab ini, Anda memerlukan [langganan Azure](https://azure.microsoft.com/free?azure-portal=true) dengan akses administrator.

Azure Cognitive Services merangkum fungsionalitas AI umum yang dapat dikategorikan ke dalam empat pilar utama: layanan penglihatan, ucapan, bahasa, dan keputusan. Dalam latihan ini, Anda akan melihat salah satu layanan keputusan untuk mendapatkan gambaran umum tentang cara menyediakan dan menggunakan sumber daya cognitive services dalam aplikasi perangkat lunak.

Cognitive services khusus yang akan Anda jelajahi dalam latihan ini adalah *Detektor Anomali*. Detektor Anomali digunakan untuk menganalisis nilai data dari waktu ke waktu, dan untuk mendeteksi nilai yang tidak biasa yang mungkin mengindikasikan persoalan atau masalah untuk penyelidikan lebih lanjut. Misalnya, sensor di fasilitas penyimpanan yang dikontrol suhu dapat memantau suhu setiap menit dan mencatat nilai yang diukur. Anda dapat menggunakan layanan Detektor Anomali untuk menganalisis nilai suhu yang dicatat dan menandai apa pun yang berada jauh di luar kisaran normal suhu yang diperkirakan.

Untuk menguji kemampuan layanan Pendeteksi Anomali, kita akan menggunakan aplikasi baris perintah sederhana yang berjalan di Cloud Shell. Prinsip dan fungsionalitas yang sama berlaku dalam solusi dunia nyata, seperti situs web atau aplikasi telepon.

> **Catatan** Tujuan dari latihan ini adalah untuk mendapatkan gambaran umum tentang bagaimana cognitive services disediakan dan digunakan. Detektor Anomali digunakan sebagai contoh, tetapi Anda tidak diharapkan untuk memperoleh pengetahuan komprehensif tentang deteksi anomali dalam latihan ini!

## <a name="create-an-anomaly-detector-resource"></a>Membuat sumber daya *Pendeteksi Anomali*

Mari kita mulai dengan membuat sumber daya **Pendeteksi Anomali** di langganan Azure Anda:

1. Di tab browser lain, buka portal Microsoft Azure di [https://portal.azure.com](https://portal.azure.com?azure-portal=true), masuk dengan akun Microsoft Anda.

1. Klik tombol **&#65291;Buat sumber daya**, cari *Pendeteksi Anomali*, dan buat sumber daya **Pendeteksi Anomali** dengan pengaturan berikut:
    - **Langganan**: *Langganan Azure Anda*.
    - **Grup sumber daya**: *Pilih grup sumber daya yang ada atau buat yang baru*.
    - **Wilayah**: *Pilih wilayah yang tersedia*.
    - **Nama**: *Masukkan nama unik*.
    - **Tingkat harga**: Gratis F0

1. Tinjau dan buat sumber daya. Tunggu hingga penyebaran selesai, lalu buka sumber daya yang disebarkan.

1. Lihat halaman **Kunci dan Titik Akhir** untuk sumber daya Pendeteksi Anomali Anda. Anda akan membutuhkan titik akhir dan kunci untuk terhubung dari aplikasi klien.

## <a name="run-cloud-shell"></a>Menjalankan Cloud Shell

Untuk menguji kemampuan layanan Pendeteksi Anomali, kita akan menggunakan aplikasi baris perintah sederhana yang berjalan di Cloud Shell di Azure.

1. Di portal Microsoft Azure, pilih tombol **[>_]** (*Cloud Shell*) di bagian atas halaman di sebelah kanan kotak pencarian. Tindakan ini akan membuka panel Cloud Shell di bagian bawah portal.

    ![Mulai Cloud Shell dengan mengeklik ikon di sebelah kanan kotak pencarian di atas](media/anomaly-detector/powershell-portal-guide-1.png)

1. Saat pertama kali membuka Cloud Shell, Anda mungkin diminta untuk memilih jenis shell yang ingin digunakan (*Bash* atau *PowerShell*). Pilih **PowerShell**. Jika Anda tidak melihat opsi ini, lewati langkah ini.  

1. Jika Anda diminta membuat penyimpanan untuk Cloud Shell, pastikan langganan ditentukan dan pilih **Buat penyimpanan**. Kemudian tunggu sekitar satu menit hingga penyimpanan dibuat.

    ![Buat penyimpanan dengan mengklik konfirmasi.](media/anomaly-detector/powershell-portal-guide-2.png)

1. Pastikan jenis shell yang ditunjukkan di kiri atas panel Cloud Shell dialihkan ke *PowerShell*. Jika *Bash*, alihkan ke *PowerShell* dengan menggunakan menu drop-down.

    ![Cara menemukan menu drop down sebelah kiri untuk beralih ke PowerShell](media/anomaly-detector/powershell-portal-guide-3.png)

1. Tunggu PowerShell untuk memulai. Anda akan melihat layar berikut di portal Microsoft Azure:  

    ![Tunggu PowerShell untuk memulai.](media/anomaly-detector/powershell-prompt.png)

## <a name="configure-and-run-a-client-application"></a>Mengonfigurasi dan menjalankan aplikasi klien

Setelah memiliki lingkungan Cloud Shell, Anda dapat menjalankan aplikasi sederhana yang menggunakan layanan Detektor Anomali untuk menganalisis data.

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

    ![Penyunting kode.](media/anomaly-detector/powershell-portal-guide-4.png)

1. Di panel **File** di sebelah kiri, luaskan **ai-900** dan pilih **detect-anomalies.ps1**. File ini berisi beberapa kode yang menggunakan layanan Pendeteksi Anomali, seperti yang ditunjukkan di sini:

    ![Penyunting yang berisi kode untuk mendeteksi anomali](media/anomaly-detector/detect-anomalies-code.png)

1. Jangan terlalu khawatir tentang detail kode, yang penting kode tersebut memerlukan URL titik akhir dan salah satu kunci untuk sumber daya Pendeteksi Anomali Anda. Salin ini dari halaman **Kunci dan Titik Akhir** untuk sumber daya Anda (yang seharusnya masih berada di area atas browser) dan tempel ke penyunting kode, menggantikan **YOUR_KEY** dan nilai tempat penampung **YOUR_ENDPOINT** masing-masing.

    > **Tips** Anda mungkin perlu menggunakan bilah pemisah untuk menyesuaikan area layar saat bekerja dengan panel **Tombol dan Titik Akhir** serta **Editor**.

    Setelah menempelkan nilai kunci dan titik akhir, dua baris pertama dari kode akan terlihat seperti ini:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."    
    $endpoint="https..."
    ```

1. Di kanan atas panel editor, gunakan tombol **...** untuk membuka menu dan pilih **Simpan** untuk menyimpan perubahan Anda. Kemudian, buka menu lagi dan pilih **Tutup Editor**.

    Ingat, deteksi anomali adalah teknik kecerdasan buatan yang digunakan untuk menentukan apakah nilai dalam rangkaian berada dalam parameter yang diharapkan. Aplikasi klien sampel akan menggunakan layanan Pendeteksi Anomali Anda untuk menganalisis file yang berisi serangkaian tanggal/waktu dan nilai numerik. Aplikasi harus mengembalikan hasil yang menunjukkan pada setiap titik waktu, semisal nilai numerik berada dalam parameter yang diharapkan.

1. Di panel PowerShell, masukkan perintah berikut untuk menjalankan kode:

    ```PowerShell
    cd ai-900
    .\detect-anomalies.ps1
    ```

1. Tinjau hasilnya, perhatikan bahwa kolom terakhir dalam hasil adalah **True** atau **False** untuk menunjukkan apakah nilai yang dicatat pada setiap tanggal/waktu dianggap sebagai anomali atau tidak. Pertimbangkan cara kita dapat menggunakan informasi ini dalam situasi kehidupan nyata. Tindakan apa yang dapat dipicu oleh aplikasi jika nilai suhu lemari es atau tekanan darah lalu terdeteksi adanya anomali?  

## <a name="learn-more"></a>Pelajari lebih lanjut

Aplikasi sederhana ini hanya menunjukkan beberapa kemampuan layanan Pendeteksi Anomali. Untuk mempelajari selengkapnya tentang apa yang dapat Anda lakukan dengan layanan ini, lihat [halaman Pendeteksi Anomali](https://azure.microsoft.com/services/cognitive-services/anomaly-detector/).

## <a name="clean-up"></a>Pembersihan

Ada baiknya di akhir proyek untuk mengidentifikasi apakah Anda masih membutuhkan sumber daya yang Anda buat. Sumber daya yang dibiarkan beroperasi dapat dikenakan biaya. 

Jika Anda melanjutkan ke modul Dasar-Dasar AI lainnya, Anda dapat menyimpan sumber daya untuk digunakan di lab lain.

Jika telah menyelesaikan pembelajaran, Anda dapat menghapus grup sumber daya atau sumber daya individual dari langganan Azure:

1. Di [portal Azure](https://portal.azure.com/), di halaman **Grup sumber daya**, buka grup sumber daya yang Anda tentukan saat membuat sumber daya.

2. Klik **Hapus grup sumber daya**, ketik nama grup sumber daya untuk mengonfirmasi bahwa Anda ingin menghapusnya, dan pilih **Hapus**. Anda juga dapat memilih untuk menghapus sumber daya individual dengan memilih sumber daya, mengklik tiga titik untuk melihat opsi lainnya, dan mengklik **Hapus**.