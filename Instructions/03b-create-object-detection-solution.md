---
lab:
  title: Menjelajahi deteksi objek
  module: Module 3 - Computer Vision
---

# <a name="explore-object-detection"></a>Menjelajahi deteksi objek

> **Catatan** Untuk menyelesaikan lab ini, Anda memerlukan [langganan Azure](https://azure.microsoft.com/free?azure-portal=true) dengan akses administrator.

*Deteksi objek* adalah bentuk computer vision di mana model pembelajaran mesin dilatih untuk mengklasifikasikan masing-masing instans objek dalam gambar, dan menunjukkan *kotak pembatas* yang menandai lokasinya. Anda dapat menganggap hal ini sebagai perkembangan dari *klasifikasi gambar* (di mana model menjawab pertanyaan "gambar apa ini?") untuk membangun solusi di mana kita dapat menanyakan model "objek apa yang ada di dalam gambar, dan di mana mereka?".

Misalnya, toko kelontong mungkin menggunakan model deteksi objek untuk menerapkan sistem pembayaran otomatis yang memindai ban berjalan menggunakan kamera, dan dapat mengidentifikasi item tertentu tanpa perlu menempatkan setiap item di ban berjalan dan memindainya satu per satu.

Layanan kognitif **Custom Vision** di Microsoft Azure menyediakan solusi berbasis cloud untuk membuat dan menerbitkan model deteksi objek kustom. Di Azure, Anda dapat menggunakan layanan Custom Vision untuk melatih model klasifikasi gambar berdasarkan gambar yang ada. Ada dua elemen untuk membuat solusi klasifikasi gambar. Pertama, Anda harus melatih model untuk mengenali kelas yang berbeda menggunakan gambar yang ada. Kemudian, saat model dilatih, Anda harus menerbitkannya sebagai layanan yang dapat digunakan oleh aplikasi.

Untuk menguji kemampuan layanan Custom Vision guna mendeteksi objek dalam gambar, kita akan menggunakan aplikasi baris perintah sederhana yang berjalan di Cloud Shell. Prinsip dan fungsionalitas yang sama berlaku dalam solusi dunia nyata, seperti situs web atau aplikasi telepon.

## <a name="create-a-cognitive-services-resource"></a>Buat sumber daya *Cognitive Services*

Anda dapat menggunakan layanan Custom Vision dengan membuat sumber daya **Custom Vision** atau sumber daya **Cognitive Services**.

> **Catatan** Tidak semua sumber daya tersedia di setiap wilayah. Baik Anda membuat sumber daya Custom Vision atau Cognitive Services, hanya sumber daya yang dibuat di [wilayah tertentu](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services) yang dapat digunakan untuk mengakses layanan Custom Vision. Demi kemudahan, wilayah telah dipilih sebelumnya untuk Anda dalam instruksi konfigurasi di bawah ini.

Buat sumber daya **Cognitive Services** di langganan Azure Anda.

1. Di tab browser lain, buka portal Microsoft Azure di [https://portal.azure.com](https://portal.azure.com?azure-portal=true), masuk dengan akun Microsoft Anda.

1. Klik tombol **&#65291;Buat sumber daya**, cari *Cognitive Services*, dan buat sumber daya **Cognitive Services** dengan pengaturan berikut:
    - **Langganan**: *Langganan Azure Anda*.
    - **Grup sumber daya**: *Pilih atau buat grup sumber daya dengan nama unik*.
    - **Wilayah**: US Timur
    - **Nama**: *Masukkan nama unik*.
    - **Tingkat harga**: Standar S0
    - **Dengan mencentang kotak ini, saya menyatakan bahwa saya telah membaca dan memahami semua persyaratan di bawah**: Dipilih.

1. Tinjau dan buat sumber daya, dan tunggu hingga penyebaran selesai. Lalu pergi ke sumber daya yang disebarkan.

1. Lihat halaman **Kunci dan Titik Akhir** untuk sumber daya Cognitive Services Anda. Anda akan memerlukan titik akhir dan kunci untuk terhubung dari aplikasi klien.

## <a name="create-a-custom-vision-project"></a>Membuat proyek Visual Kustom

Untuk melatih model deteksi objek, Anda perlu membuat proyek Custom Vision berdasarkan sumber daya pelatihan. Untuk melakukannya, Anda akan menggunakan portal Custom Vision.

1. Di tab browser baru, buka portal Custom Vision di [https://customvision.ai](https://customvision.ai?azure-portal=true), dan masuk menggunakan akun Microsoft yang terkait dengan langganan Azure Anda.

1. Buat proyek baru dengan pengaturan berikut:
    - **Nama**: Deteksi Belanjaan
    - **Deskripsi**: Deteksi objek untuk belanjaan.
    - **Sumber daya**: *Sumber daya yang Anda buat sebelumnya*
    - **Jenis Proyek**: Deteksi Objek
    - **Domain**: Umum

1. Tunggu proyek dibuat dan dibuka di browser.

## <a name="add-and-tag-images"></a>Tambahkan dan beri tag gambar

Untuk melatih model deteksi objek, Anda perlu mengunggah gambar yang berisi kelas yang ingin diidentifikasi oleh model, dan memberi tag gambar untuk menunjukkan kotak pembatas untuk setiap instans objek.

1. Unduh dan ekstrak gambar pelatihan dari https://aka.ms/fruit-objects. Folder yang diekstrak berisi kumpulan gambar buah-buahan.

1. Di portal Custom Vision [https://customvision.ai](https://customvision.ai?azure-portal=true), pastikan Anda bekerja di proyek deteksi objek _Deteksi Belanjaan_. Kemudian pilih **Tambahkan gambar** dan unggah semua gambar dalam folder yang diekstrak.

    ![Unggah gambar yang diunduh dengan mengklik tambahkan gambar.](media/create-object-detection-solution/fruit-upload.jpg)

1. Setelah gambar diunggah, pilih yang pertama untuk membukanya.

1. Tahan mouse di atas objek apa pun pada gambar hingga wilayah yang terdeteksi secara otomatis ditampilkan seperti gambar di bawah ini. Kemudian pilih objek, dan jika perlu ubah ukuran wilayah untuk mengelilinginya.

    ![Wilayah default untuk objek](media/create-object-detection-solution/object-region.jpg)

    Atau, Anda cukup menyeret objek untuk membuat wilayah.

1. Saat wilayah mengelilingi objek, tambahkan tag baru dengan jenis objek yang sesuai (*apel*, *pisang*, atau *jeruk*) seperti yang ditunjukkan di sini:

    ![Objek yang diberi tag dalam gambar](media/create-object-detection-solution/object-tag.jpg)

1. Pilih dan beri tag satu sama lain pada objek dalam gambar, ubah ukuran wilayah dan tambahkan tag baru sesuai kebutuhan.

    ![Dua objek yang diberi tag dalam gambar](media/create-object-detection-solution/object-tags.jpg)

1. Gunakan tautan **>** di sebelah kanan untuk membuka gambar berikutnya, dan beri tag pada objeknya. Kemudian terus kerjakan seluruh kumpulan gambar, beri tag pada setiap apel, pisang, dan jeruk.

1. Setelah Anda selesai memberi tag pada gambar terakhir, tutup penyunting **Detail Gambar** dan pada halaman **Gambar Pelatihan**, di bawah **Tag**, pilih **Diberi Tag** untuk melihat semua gambar yang diberi tag:

    ![Gambar yang diberi tag dalam proyek](media/create-object-detection-solution/tagged-images.jpg)

## <a name="train-and-test-a-model"></a>Latih dan uji model

Sekarang setelah memberi tag gambar dalam proyek, Anda siap untuk melatih model.

1. Dalam proyek Custom Vision, klik **Latih** untuk melatih model deteksi objek menggunakan gambar yang diberi tag. Pilih opsi **Pelatihan Cepat**.

1. Tunggu hingga pelatihan selesai (mungkin memerlukan waktu sekitar sepuluh menit), lalu tinjau metrik kinerja *Presisi*, *Recall*, dan *mAP* - ini mengukur prediksi kebaikan model deteksi objek, dan semuanya harus tinggi.

1. Di kanan atas halaman, klik **Uji Cepat**, lalu di kotak **URL Gambar**, masukkan `https://aka.ms/apple-orange` dan lihat prediksi yang dihasilkan. Kemudian tutup jendela **Uji Cepat**.

## <a name="publish-the-object-detection-model"></a>Terbitkan model deteksi objek

Sekarang Anda siap untuk menerbitkan model terlatih dan menggunakannya dari aplikasi klien.

1. Klik **&#128504; Terbitkan** untuk menerbitkan model terlatih dengan pengaturan berikut:
    - **Nama model**: detect-produce
    - **Sumber daya prediksi**: *Sumber daya yang Anda buat sebelumnya*.

1. Setelah menerbitkan, klik ikon *URL Prediksi* (&#127760;) untuk melihat informasi yang diperlukan untuk menggunakan model yang diterbitkan. Nanti, Anda akan memerlukan URL yang sesuai dan nilai-nilai Kunci Prediksi untuk mendapatkan prediksi dari URL Gambar, jadi biarkan kotak dialog ini tetap terbuka dan lanjutkan ke tugas berikutnya.

## <a name="run-cloud-shell"></a>Jalankan Cloud Shell

Untuk menguji kemampuan layanan Custom Vision, kami akan menggunakan aplikasi baris perintah sederhana yang berjalan di Cloud Shell di Azure.

1. Di portal Microsoft Azure, pilih tombol **[>_]** (*Cloud Shell*) di bagian atas halaman di sebelah kanan kotak pencarian. Tindakan ini akan membuka panel Cloud Shell di bagian bawah portal. 

    ![Mulai Cloud Shell dengan mengeklik ikon di sebelah kanan kotak pencarian di atas](media/create-object-detection-solution/powershell-portal-guide-1.png)

1. Saat pertama kali membuka Cloud Shell, Anda mungkin diminta untuk memilih jenis shell yang ingin digunakan (*Bash* atau *PowerShell*). Pilih **PowerShell**. Jika Anda tidak melihat opsi ini, lewati langkah ini.  

1. Jika Anda diminta membuat penyimpanan untuk Cloud Shell, pastikan langganan ditentukan dan pilih **Buat penyimpanan**. Kemudian tunggu sekitar satu menit hingga penyimpanan dibuat.

    ![Buat penyimpanan dengan mengklik konfirmasi.](media/create-object-detection-solution/powershell-portal-guide-2.png)

1. Pastikan jenis shell yang ditunjukkan di kiri atas panel Cloud Shell dialihkan ke *PowerShell*. Jika *Bash*, alihkan ke *PowerShell* dengan menggunakan menu drop-down.

    ![Cara menemukan menu drop down sebelah kiri untuk beralih ke PowerShell](media/create-object-detection-solution/powershell-portal-guide-3.png) 

1. Tunggu PowerShell untuk memulai. Anda akan melihat layar berikut di portal Microsoft Azure:  

    ![Tunggu PowerShell untuk memulai.](media/create-object-detection-solution/powershell-prompt.png) 

## <a name="configure-and-run-a-client-application"></a>Konfigurasi dan jalankan aplikasi klien

Sekarang setelah memiliki model kustom, Anda dapat menjalankan aplikasi klien sederhana yang menggunakan layanan Custom Vision untuk mendeteksi objek dalam gambar.

1. Di shell perintah, masukkan perintah berikut untuk mengunduh aplikasi contoh dan menyimpannya ke folder bernama ai-900.

    ```PowerShell
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

    >**Catatan** Jika sudah menggunakan perintah ini di lab lain untuk menggandakan penyimpanan *ai-900*, Anda dapat melewati langkah ini.

1. File diunduh ke folder bernama **ai-900**. Sekarang kami ingin melihat semua file di penyimpanan Cloud Shell Anda dan menggunakannya. Ketik perintah berikut ke dalam shell:

    ```PowerShell
    code .
    ```

    Perhatikan bagaimana perintah ini membuka penyunting seperti pada gambar di bawah ini: 

    ![Penyunting kode.](media/create-object-detection-solution/powershell-portal-guide-4.png)

1. Di panel **File** di sebelah kiri, luaskan **ai-900** dan pilih **detect-objects.ps1**. File ini berisi beberapa kode yang menggunakan layanan Custom Vision untuk mendeteksi objek gambar, seperti yang ditunjukkan di sini:

    ![Penyunting yang berisi kode untuk mendeteksi item dalam gambar](media/create-object-detection-solution/detect-image-code.png)

1. Jangan terlalu khawatir tentang detail kode, yang penting kode tersebut memerlukan URL prediksi dan kunci untuk model Custom Vision Anda saat menggunakan URL gambar. 

    Dapatkan *URL prediksi* dari kotak dialog dalam proyek Custom Vision. 

    >**Catatan** Ingat, Anda sudah meninjau *URL prediksi* setelah menerbitkan model klasifikasi gambar. Untuk menemukan *URL prediksi*, buka tab **Performa** di proyek Anda, lalu klik **URL Prediksi** (jika layar dikompresi, Anda mungkin hanya melihat ikon bola bumi). Kotak dialog akan muncul. Salin url untuk **Jika Anda memiliki URL gambar**. Tempelkan ke dalam penyunting kode, dengan mengganti **YOUR_PREDICTION_URL**. 

    Dengan menggunakan kotak dialog yang sama, dapatkan *kunci prediksi*. Salin kunci prediksi yang ditampilkan setelah *Mengatur Header Kunci-Prediksi ke*. Tempel di penyunting kode, menggantikan nilai tempat penampung **YOUR_PREDICTION_KEY**. 

    ![Cuplikan layar URL prediksi.](media/create-object-detection-solution/find-prediction-url.png)

    Setelah menempelkan nilai URL Prediksi dan Kunci Prediksi, dua baris kode pertama akan terlihat seperti ini:

    ```PowerShell
    $predictionUrl="https..."
    $predictionKey ="1a2b3c4d5e6f7g8h9i0j...."
    ```

1. Di kanan atas panel penyunting, gunakan tombol **...** untuk membuka menu dan pilih **Simpan** untuk menyimpan perubahan Anda. Kemudian buka lagi menu dan pilih **Tutup Penyunting**.

    Anda akan menggunakan aplikasi klien sampel untuk mendeteksi objek dalam gambar ini:

    ![Gambar buah](media/create-object-detection-solution/produce.jpg)

1. Di panel PowerShell, masukkan perintah berikut untuk menjalankan kode:

    ```PowerShell
    cd ai-900
    ./detect-objects.ps1 
    ```

1. Tinjau prediksi, yang seharusnya berupa *pisang jeruk apel*.

## <a name="learn-more"></a>Pelajari lebih lanjut

Aplikasi sederhana ini hanya menunjukkan beberapa kemampuan layanan Custom Vision. Untuk mempelajari selengkapnya tentang apa yang dapat Anda lakukan dengan layanan ini, lihat [halaman Custom Vision](https://azure.microsoft.com/services/cognitive-services/custom-vision-service/).
