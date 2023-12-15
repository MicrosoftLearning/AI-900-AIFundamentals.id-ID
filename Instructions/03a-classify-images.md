---
lab:
  title: Menjelajahi klasifikasi gambar
---

# Menjelajahi klasifikasi gambar

Layanan kognitif *Computer Vision* menyediakan model yang telah dibangun sebelumnya yang berguna untuk bekerja dengan gambar, tetapi Anda sering kali perlu melatih model Anda sendiri untuk computer vision. Misalnya, organisasi konservasi satwa liar ingin melacak penampakan hewan dengan menggunakan kamera yang sensitif terhadap gerakan. Gambar yang ditangkap oleh kamera kemudian dapat digunakan untuk memverifikasi keberadaan spesies tertentu di area tertentu dan membantu upaya konservasi untuk spesies yang terancam punah. Untuk mencapai hal ini, organisasi akan mendapat manfaat dari *model klasifikasi* gambar yang dilatih untuk mengidentifikasi berbagai spesies hewan dalam foto yang ditangkap.

Di Azure, Anda dapat menggunakan layanan Custom Vision untuk melatih model klasifikasi gambar berdasarkan gambar yang ada. Ada dua elemen untuk membuat solusi klasifikasi gambar. Pertama, Anda harus melatih model untuk mengenali kelas yang berbeda menggunakan gambar yang ada. Kemudian, saat model dilatih, Anda harus menerbitkannya sebagai layanan yang dapat digunakan oleh aplikasi.

Untuk menguji kemampuan layanan Custom Vision, kita akan menggunakan aplikasi baris perintah sederhana yang berjalan di Cloud Shell. Prinsip dan fungsi yang sama berlaku dalam solusi dunia nyata, seperti situs web atau aplikasi ponsel.

## Sebelum memulai

Untuk menyelesaikan lab ini, Anda memerlukan [langganan Azure](https://azure.microsoft.com/free?azure-portal=true) tempat Anda memiliki akses administratif.

## Membuat sumber daya Azure App Service

Anda dapat menggunakan layanan Custom Vision dengan membuat sumber daya **Custom Vision** atau sumber daya **Cognitive Services**.

>**Catatan** Tidak semua sumber daya tersedia di setiap wilayah. Baik Anda membuat sumber daya Custom Vision atau Cognitive Services, hanya sumber daya yang dibuat di [wilayah tertentu](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services) yang dapat digunakan untuk mengakses layanan Custom Vision. Demi kemudahan, wilayah telah dipilih sebelumnya untuk Anda dalam instruksi konfigurasi di bawah ini.

Membuat sumber daya Azure **Layanan bahasa ** di langganan Azure Anda

1. Buka portal Azure di [https://portal.azure.com](https://portal.azure.com?azure-portal=true), masuk dengan akun Microsoft Anda.

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

1. Unduh dan ekstrak gambar pelatihan dari [https://aka.ms/animal-images](https://aka.ms/animal-images). Gambar-gambar ini disediakan dalam folder zip, yang jika diekstrak berisi subfolder yang disebut **apel**, **pisang**, dan **jeruk**.

1. Di tab browser baru, buka portal Visi Khusus di [https://customvision.ai](https://customvision.ai?azure-portal=true). Jika diminta, masuk menggunakan akun Microsoft yang terkait dengan langganan Azure Anda dan setujui ketentuan layanan.

1. Di portal Custom Vision, buat proyek baru dengan pengaturan berikut:

    - **Nama**: Identifikasi Hewan
    - **Deskripsi**: Klasifikasi gambar untuk belanjaan
    - **Sumber daya**: *Sumber daya Custom Vision yang Anda buat sebelumnya*
    - **Jenis Proyek**: Klasifikasi
    - **Jenis Klasifikasi**: Multikelas (Tag tunggal per gambar)
    - **Domain**: Umum

1. Klik **Tambahkan gambar**, dan pilih semua file di folder **apel** yang Anda ekstrak sebelumnya. Kemudian unggah file gambar, tentukan tag *apel*, seperti ini:

    ![Cuplikan layar antarmuka pengunggahan Gambar.](media/create-image-classification-system/upload-elephants.png)

1. Gunakan tombol **Tambahkan gambar** ([+]) untuk mengunggah gambar di **folder jerami** dengan jerami* tag*, dan gambar di **folder singa** dengan singa* tag*.

1. Jelajahi gambar yang telah Anda unggah di proyek Custom Vision - harus ada 15 gambar dari setiap kelas, seperti ini:

    ![Cuplikan layar gambar pelatihan yang diberi tag di portal Custom Vision.](media/create-image-classification-system/animal-training-images.png)

1. Dalam proyek Custom Vision, di atas gambar, klik **Latih** untuk melatih model klasifikasi menggunakan gambar yang diberi tag. Pilih opsi **Pelatihan Cepat**, lalu tunggu hingga perulangan pelatihan selesai (hal ini mungkin memerlukan waktu sekitar satu menit).

    > **Tips**: Pelatihan mungkin memakan waktu beberapa menit. Sambil menunggu, lihat [Bagaimana selfie macan tutul salju dan AI dapat membantu menyelamatkan spesies dari kepunahan](https://news.microsoft.com/transform/snow-leopard-selfies-ai-save-species/), yang menggambarkan proyek nyata yang menggunakan visi komputer untuk melacak hewan yang terancam punah di alam liar.

1. Ketika perulangan model telah dilatih, tinjau metrik performa *Presisi*, *Pengenalan*, dan *AP* - hal ini mengukur akurasi prediksi model klasifikasi, dan semua harus tinggi.

## Menguji model

Sebelum menerbitkan perulangan model ini untuk digunakan aplikasi, Anda harus mengujinya.

1. Di atas metrik performa, klik **Uji Cepat**.

1. Dalam kotak **URL** Gambar, ketik `https://aka.ms/giraffe` dan klik tombol **gambar uji cepat (&#10132;).**

1. Lihat prediksi yang ditampilkan oleh model Anda - skor peluang untuk *apel* harus yang tertinggi, seperti ini:

    ![Cuplikan layar antarmuka Uji Cepat.](media/create-image-classification-system/quick-test.png)

1. Tutup jendela **Uji Cepat**.

## Terbitkan model klasifikasi gambar

Sekarang Anda siap untuk menerbitkan model terlatih dan menggunakannya dari aplikasi klien.

1. Klik **&#128504; Terbitkan** untuk menerbitkan model terlatih dengan pengaturan berikut:
    - **Nama model**: hewan
    - **Sumber Daya** Prediksi: *Layanan Azure AI atau sumber daya prediksi Custom Vision yang Anda buat sebelumnya*.

1. Setelah menerbitkan, klik ikon *URL Prediksi* (&#127760;) untuk melihat informasi yang diperlukan untuk menggunakan model yang diterbitkan.

    ![Cuplikan layar URL prediksi.](media/create-image-classification-system/prediction-url.png)

Nanti, Anda akan memerlukan URL yang sesuai dan nilai-nilai Kunci Prediksi untuk mendapatkan prediksi dari URL Gambar, jadi biarkan kotak dialog ini tetap terbuka dan lanjutkan ke tugas berikutnya.

## Menyiapkan aplikasi klien

Untuk menguji kemampuan layanan Custom Vision, kami akan menggunakan aplikasi baris perintah sederhana yang berjalan di Cloud Shell di Azure.

1. Di portal Microsoft Azure, pilih tombol **[>_]** (**Cloud Shell**) di bagian atas halaman di sebelah kanan kotak pencarian. Tindakan ini akan membuka panel Cloud Shell di bagian bawah portal.

    Saat pertama kali membuka Cloud Shell, Anda mungkin diminta untuk memilih jenis shell yang ingin digunakan (*Bash* atau *PowerShell*). Pilih **PowerShell**.

    Jika Anda diminta membuat penyimpanan untuk Cloud Shell, pastikan langganan ditentukan dan pilih **Buat penyimpanan**. Kemudian tunggu sekitar satu menit hingga penyimpanan dibuat.

    Ketika cloud shell siap, cloud shell akan terlihat mirip dengan ini:
    
    ![Cuplikan layar saat membuka Azure Cloud Shell di portal Microsoft Azure.](media/create-image-classification-system/cloud-shell.png)

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
    code classify-image.ps1
    ```

    Perhatikan bagaimana perintah ini membuka penyunting seperti pada gambar di bawah ini:

     ![Cuplikan layar editor kode di cloud shell.](media/create-image-classification-system/code-editor.png)

     > **Tips**: Anda dapat menggunakan bilah pemisah antara baris perintah cloud shell dan editor kode untuk mengubah ukuran panel.

4. Jangan terlalu memikirkan detail kode. Yang penting adalah dimulai dengan beberapa kode untuk menentukan URL prediksi dan kunci untuk model Custom Vision Anda. Anda harus memperbaruinya sehingga sisa kode menggunakan model Anda.

    *Dapatkan URL* prediksi dan *kunci* prediksi dari kotak dialog yang Anda tinggalkan terbuka di tab browser untuk proyek Custom Vision Anda. **Anda memerlukan versi yang akan digunakan *jika Anda memiliki URL* gambar.**

    Gunakan nilai-nilai ini untuk menggantikan **tempat penampung YOUR_PREDICTION_URL** dan **YOUR_PREDICTION_KEY** dalam file kode.

    Setelah menempelkan nilai URL Prediksi dan Kunci Prediksi, dua baris kode pertama akan terlihat seperti ini:

    ```PowerShell
    $predictionUrl="https..."
    $predictionKey ="1a2b3c4d5e6f7g8h9i0j...."
    ```

5. Setelah membuat perubahan pada variabel dalam kode, tekan **CTRL+S** untuk menyimpan file. Kemudian tekan Ctrl+Q untuk menutup editor kode.

## Menguji aplikasi klien

Sekarang Anda dapat menggunakan aplikasi klien sampel untuk mengklasifikasikan gambar berdasarkan hewan yang dikandungnya.

1. Di panel PowerShell, masukkan perintah berikut untuk menjalankan kode:

    ```PowerShell
    ./classify-image.ps1 1
    ```

    Kode ini menggunakan model Anda untuk mengklasifikasikan gambar berikut:

    ![Foto jerami.](media/create-image-classification-system/animal-1.jpg)

1. Tinjau prediksi, yang seharusnya **apel**.

1. Sekarang mari kita coba gambar lain: Jalankan perintah ini:

    ```PowerShell
    ./classify-image.ps1 2
    ```

    Kali ini gambar berikut diklasifikasikan:

    ![Foto gajah.](media/create-image-classification-system/animal-2.jpg)

1. Pastikan model mengklasifikasikan gambar ini sebagai **jeruk**.

1. Mari coba sekali lagi: Jalankan perintah ini:

    ```PowerShell
    ./classify-image.ps1 3
    ```

    Jadwal akhir terlihat seperti ini:

    ![Foto seekor anjing.](media/create-image-classification-system/animal-3.jpg)

1. Pastikan model mengklasifikasikan gambar ini sebagai **jeruk**.

Mudah-mudahan, model klasifikasi gambar Anda mengklasifikasikan ketiga gambar dengan benar.


