---
lab:
  title: Menjelajahi deteksi objek
---

# Menjelajahi deteksi objek

> **Catatan** Untuk menyelesaikan lab ini, Anda memerlukan [langganan Azure](https://azure.microsoft.com/free?azure-portal=true) dengan akses administrator.

*Deteksi objek* adalah bentuk computer vision di mana model pembelajaran mesin dilatih untuk mengklasifikasikan masing-masing instans objek dalam gambar, dan menunjukkan *kotak pembatas* yang menandai lokasinya. Anda dapat menganggap hal ini sebagai perkembangan dari *klasifikasi gambar* (di mana model menjawab pertanyaan "gambar apa ini?") untuk membangun solusi di mana kita dapat menanyakan model "objek apa yang ada di dalam gambar, dan di mana mereka?".

Misalnya, inisiatif keselamatan jalan mungkin mengidentifikasi pejalan kaki dan pengendara sepeda sebagai pengguna jalan yang paling rentan di persimpangan lalu lintas. Dengan menggunakan kamera untuk memantau persimpangan, gambar pengguna jalan dapat dianalisis untuk mendeteksi pejalan kaki dan pengendara sepeda untuk memantau jumlah mereka atau bahkan mengubah perilaku sinyal lalu lintas.

Layanan kognitif **Custom Vision** di Microsoft Azure menyediakan solusi berbasis cloud untuk membuat dan menerbitkan model deteksi objek kustom. Di Azure, Anda dapat menggunakan layanan Custom Vision untuk melatih model klasifikasi gambar berdasarkan gambar yang ada. Ada dua elemen untuk membuat solusi deteksi objek. Pertama, Anda harus melatih model untuk mendeteksi lokasi dan kelas objek menggunakan gambar berlabel. Kemudian, saat model dilatih, Anda harus menerbitkannya sebagai layanan yang dapat digunakan oleh aplikasi.

Untuk menguji kemampuan layanan Custom Vision guna mendeteksi objek dalam gambar, kita akan menggunakan aplikasi baris perintah sederhana yang berjalan di Cloud Shell. Prinsip dan fungsi yang sama berlaku dalam solusi dunia nyata, seperti situs web atau aplikasi ponsel.

## Membuat sumber daya Azure App Service

Anda dapat menggunakan layanan Custom Vision dengan membuat sumber daya **Custom Vision** atau sumber daya **Cognitive Services**.

> **Catatan** Tidak semua sumber daya tersedia di setiap wilayah. Baik Anda membuat sumber daya Custom Vision atau Cognitive Services, hanya sumber daya yang dibuat di [wilayah tertentu](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services) yang dapat digunakan untuk mengakses layanan Custom Vision. Demi kemudahan, wilayah telah dipilih sebelumnya untuk Anda dalam instruksi konfigurasi di bawah ini.

Membuat sumber daya Azure **Layanan bahasa ** di langganan Azure Anda

1. Di tab browser lain, buka portal Microsoft Azure di [https://portal.azure.com](https://portal.azure.com?azure-portal=true), masuk dengan akun Microsoft Anda.

1. **Klik &#65291; Buat tombol sumber daya** dan cari *layanan* Azure AI. Pilih **buat** **paket layanan** Azure AI. Anda akan dibawa ke halaman untuk membuat sumber daya layanan Azure AI. Konfigurasikan PuTTY dengan pengaturan berikut:
    - **Langganan**: *Langganan Azure Anda*.
    - **Grup sumber daya**: *Pilih atau buat grup sumber daya dengan nama unik*.
    - **Wilayah**: US Timur
    - **Nama**: *Masukkan nama unik*.
    - **Tingkat harga**: Standar S0
    - **Dengan mencentang kotak ini, saya menyatakan bahwa saya telah membaca dan memahami semua persyaratan di bawah**: Dipilih.

1. Tinjau dan buat sumber daya, dan tunggu hingga penyebaran selesai. Lalu pergi ke sumber daya yang disebarkan.

1. Lihat halaman **Kunci dan Titik Akhir** untuk mengetahui sumber daya Cognitive Services Anda. Anda akan memerlukan titik akhir dan kunci untuk terhubung dari aplikasi klien.

## Membuat proyek Visual Kustom

Untuk melatih model deteksi objek, Anda perlu membuat proyek Custom Vision berdasarkan sumber daya pelatihan. Untuk melakukannya, Anda akan menggunakan portal Custom Vision.

1. Di tab browser baru, buka portal Custom Vision di [https://customvision.ai](https://customvision.ai?azure-portal=true), dan masuk menggunakan akun Microsoft yang terkait dengan langganan Azure Anda.

1. Buat proyek baru dengan pengaturan berikut:
    - **Nama**: Traffic Brankas ty
    - **Deskripsi**: Deteksi objek untuk keselamatan jalan.
    - **Sumber daya**: *Sumber daya yang Anda buat sebelumnya*
    - **Jenis Proyek**: Deteksi Objek
    - **Domain**: Umum

1. Tunggu proyek dibuat dan dibuka di browser.

## Tambahkan dan beri tag gambar

Untuk melatih model deteksi objek, Anda perlu mengunggah gambar yang berisi kelas yang ingin diidentifikasi oleh model, dan memberi tag gambar untuk menunjukkan kotak pembatas untuk setiap instans objek.

1. Unduh dan ekstrak gambar pelatihan dari [https://aka.ms/traffic-images](https://aka.ms/traffic-images). Folder yang diekstrak berisi kumpulan gambar buah-buahan.

1. Di portal Custom Vision, di proyek deteksi objek Traffic Brankas ty** Anda**, pilih **Tambahkan gambar** dan unggah semua gambar di folder yang diekstrak.

    ![Cuplikan layar kotak dialog Unggah Gambar di Custom Vision Studio.](media/create-object-detection-solution/upload-images.png)

1. Setelah gambar diunggah, pilih yang pertama untuk membukanya.

1. Tahan mouse di atas objek apa pun pada gambar hingga wilayah yang terdeteksi secara otomatis ditampilkan seperti gambar di bawah ini. Kemudian pilih objek, dan jika perlu ubah ukuran wilayah untuk mengelilinginya. Atau, Anda cukup menyeret objek untuk membuat wilayah.

    Saat objek dipilih dengan erat dalam wilayah persegi panjang, masukkan tag yang sesuai untuk objek (*Pengendara Sepeda* atau *Pejalan* Kaki) dan gunakan **tombol Wilayah** tag (**+**) untuk menambahkan tag ke proyek.

    ![Cuplikan layar gambar dengan wilayah yang ditandai dalam kotak dialog Detaol Gambar.](media/create-object-detection-solution/tag-image.png)

1. Gunakan tautan  di sebelah kanan untuk membuka gambar berikutnya, dan beri tag pada objeknya. Kemudian terus kerjakan seluruh kumpulan gambar, beri tag pada setiap apel, pisang, dan jeruk.

    Saat Anda menandai gambar, perhatikan hal berikut:

    - Beberapa gambar berisi beberapa objek, berpotensi dari berbagai jenis. Tandai masing-masing, bahkan jika tumpang tindih.
    - Setelah tag dimasukkan sekali, Anda dapat memilihnya dari daftar saat menandai objek baru.
    - Anda dapat kembali dan meneruskan gambar untuk menyesuaikan tag.

    ![Cuplikan layar gambar dengan wilayah yang ditandai dalam kotak dialog Detaol Gambar.](media/create-object-detection-solution/multiple-objects.png)

1. Setelah Anda selesai memberi tag pada gambar terakhir, tutup penyunting **Detail Gambar** dan pada halaman **Gambar Pelatihan**, di bawah **Tag**, pilih **Diberi Tag** untuk melihat semua gambar yang diberi tag:

    ![Cuplikan layar gambar yang ditandai dalam proyek.](media/create-object-detection-solution/tagged-images.png)

## Latih dan uji model

Sekarang setelah memberi tag gambar dalam proyek, Anda siap untuk melatih model.

1. Dalam proyek Custom Vision, klik **Latih** untuk melatih model deteksi objek menggunakan gambar yang diberi tag. Pilih opsi **Pelatihan Cepat**.

    > Pelatihan mungkin memerlukan waktu beberapa menit untuk diselesaikan. Sambil menunggu, lihat Analitik [video untuk kota](https://www.microsoft.com/research/video/video-analytics-for-smart-cities/) pintar, yang menjelaskan proyek nyata untuk menggunakan visi komputer dalam inisiatif peningkatan keselamatan jalan.

2. Tunggu hingga pelatihan selesai (mungkin memerlukan waktu sekitar sepuluh menit), lalu tinjau metrik kinerja *Presisi*, *Recall*, dan *mAP* - ini mengukur prediksi kebaikan model deteksi objek, dan semuanya harus tinggi.

3. Sesuaikan **Ambang** Probabilitas di sebelah kiri, tingkatkan dari 50% menjadi 90% dan amati pengaruhnya pada metrik performa. Pengaturan ini menentukan nilai probabilitas yang harus dipenuhi atau melebihi setiap evaluasi tag untuk dihitung sebagai prediksi.

    ![Cuplikan layar metrik performa untuk model terlatih.](media/create-object-detection-solution/performance-metrics.png)

4. Di kanan atas halaman, klik **Uji Cepat**, lalu di kotak **URL Gambar**, masukkan `https://aka.ms/pedestrian-cyclist` dan lihat prediksi yang dihasilkan.

    Di panel sebelah kanan, di bawah **Prediksi**, setiap objek yang terdeteksi tercantum dengan tag dan probabilitasnya. Pilih setiap objek untuk melihatnya disorot dalam gambar.

    Objek yang diprediksi mungkin tidak semuanya benar - setelah semua, pengendara sepeda dan pejalan kaki berbagi banyak fitur umum. Prediksi bahwa model paling yakin tentang memiliki nilai probabilitas tertinggi. Gunakan penggeser **Nilai** Ambang untuk menghilangkan objek dengan probabilitas rendah. Anda harus dapat menemukan titik di mana hanya prediksi yang benar yang disertakan (mungkin sekitar 85-90%).

    ![Cuplikan layar metrik performa untuk model terlatih.](media/create-object-detection-solution/test-detection.png)

5. Kemudian tutup jendela **Uji Cepat**.

## Terbitkan model deteksi objek

Sekarang Anda siap untuk menerbitkan model terlatih dan menggunakannya dari aplikasi klien.

1. Klik **&#128504; Terbitkan** untuk menerbitkan model terlatih dengan pengaturan berikut:
    - **Nama model**: keselamatan lalu lintas
    - **Sumber daya prediksi**: *Sumber daya yang Anda buat sebelumnya*.

1. Setelah menerbitkan, klik ikon *URL Prediksi* (&#127760;) untuk melihat informasi yang diperlukan untuk menggunakan model yang diterbitkan.

    ![Cuplikan layar URL prediksi.](media/create-object-detection-solution/prediction-url.png)

Nanti, Anda akan memerlukan URL yang sesuai dan nilai-nilai Kunci Prediksi untuk mendapatkan prediksi dari URL Gambar, jadi biarkan kotak dialog ini tetap terbuka dan lanjutkan ke tugas berikutnya.

## Menyiapkan aplikasi klien

Untuk menguji kemampuan layanan Custom Vision, kami akan menggunakan aplikasi baris perintah sederhana yang berjalan di Cloud Shell di Azure.

1. Di portal Microsoft Azure, pilih tombol **[>_]** (**Cloud Shell**) di bagian atas halaman di sebelah kanan kotak pencarian. Tindakan ini akan membuka panel Cloud Shell di bagian bawah portal.

    Saat pertama kali membuka Cloud Shell, Anda mungkin diminta untuk memilih jenis shell yang ingin digunakan (*Bash* atau *PowerShell*). Pilih **PowerShell**.

    Jika Anda diminta membuat penyimpanan untuk Cloud Shell, pastikan langganan ditentukan dan pilih **Buat penyimpanan**. Kemudian tunggu sekitar satu menit hingga penyimpanan dibuat.

    Ketika cloud shell siap, cloud shell akan terlihat mirip dengan ini:
    
    ![Cuplikan layar saat membuka Azure Cloud Shell di portal Microsoft Azure.](media/create-object-detection-solution/cloud-shell.png)

    > Pastikan jenis shell yang ditunjukkan di kiri atas panel Cloud Shell dialihkan ke PowerShell. Jika *Bash*, alihkan ke *PowerShell* dengan menggunakan menu drop-down.

    Perhatikan bahwa Anda dapat mengubah ukuran cloud shell dengan menyeret bilah pemisah di bagian atas panel, atau menggunakan ikon **&#8212;**, **&#9723;**, dan **X** di kanan atas panel untuk meminimalkan, memaksimalkan, dan menutup panel. Untuk informasi selengkapnya tentang menggunakan Azure Cloud Shell, lihat [dokumentasi Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).

2. Di shell perintah, masukkan perintah berikut untuk mengunduh file untuk latihan ini dan simpan di folder bernama **ai-900** (setelah menghapus folder tersebut jika sudah ada)

    ```PowerShell
    rm -r ai-900 -f
    git clone https://github.com/MicrosoftLearning/AI-900-AIFundamentals ai-900
    ```

3. Setelah file diunduh, masukkan perintah berikut untuk mengubah ke **direktori ai-900** dan edit file kode untuk latihan ini:

    ```PowerShell
    cd ai-900
    code detect-objects.ps1
    ```

    Perhatikan bagaimana perintah ini membuka penyunting seperti pada gambar di bawah ini:

     ![Cuplikan layar editor kode di cloud shell.](media/create-object-detection-solution/code-editor.png)

     > **Tips**: Anda dapat menggunakan bilah pemisah antara baris perintah cloud shell dan editor kode untuk mengubah ukuran panel.

4. Jangan terlalu memikirkan detail kode. Yang penting adalah dimulai dengan beberapa kode untuk menentukan URL prediksi dan kunci untuk model Custom Vision Anda. Anda harus memperbaruinya sehingga sisa kode menggunakan model Anda.

    *Dapatkan URL* prediksi dan *kunci* prediksi dari kotak dialog yang Anda tinggalkan terbuka di tab browser untuk proyek Custom Vision Anda. Anda memerlukan versi yang akan digunakan *jika Anda memiliki URL* gambar.

    Gunakan nilai-nilai ini untuk menggantikan **tempat penampung YOUR_PREDICTION_URL** dan **YOUR_PREDICTION_KEY** dalam file kode.

    Setelah menempelkan nilai URL Prediksi dan Kunci Prediksi, dua baris kode pertama akan terlihat seperti ini:

    ```PowerShell
    $predictionUrl="https..."
    $predictionKey ="1a2b3c4d5e6f7g8h9i0j...."
    ```

5. Setelah membuat perubahan pada variabel dalam kode, tekan **CTRL+S** untuk menyimpan file. Kemudian tekan Ctrl+Q untuk menutup editor kode.

## Menguji aplikasi klien

Sekarang Anda dapat menggunakan aplikasi klien sampel untuk mendeteksi pengendara sepeda dan pejalan kaki dalam gambar.

1. Di panel PowerShell, masukkan perintah berikut untuk menjalankan kode:

    ```PowerShell
    ./detect-objects.ps1 1
    ```

    Kode ini menggunakan model Anda untuk mendeteksi objek dalam gambar berikut:

    ![Foto pejalan kaki dan pengendara sepeda.](media/create-object-detection-solution/road-safety-1.jpg)

1. Tinjau prediksi, yang mencantumkan objek apa pun yang terdeteksi dengan probabilitas 90% atau lebih, bersama dengan koordinat kotak pembatas di sekitar lokasinya.

1. Sekarang mari kita coba gambar lain: Jalankan perintah ini:

    ```PowerShell
    ./detect-objects.ps1 2
    ```

    Kali ini gambar berikut dianalisis:

    ![Foto sekelompok pejalan kaki.](media/create-object-detection-solution/road-safety-2.jpg)

Mudah-mudahan, model deteksi objek Anda melakukan pekerjaan yang baik untuk mendeteksi pejalan kaki dan pengendara sepeda dalam gambar pengujian.

