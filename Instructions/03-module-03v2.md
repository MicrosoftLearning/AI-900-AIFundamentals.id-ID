---
lab:
  title: Menjelajahi analitik teks
---

# Menjelajahi analitik teks

> **Catatan** Untuk menyelesaikan lab ini, Anda memerlukan [langganan Azure](https://azure.microsoft.com/free?azure-portal=true) dengan akses administrator.

Natural Language Processing (NLP) adalah cabang kecerdasan buatan (AI) yang berhubungan dengan bahasa tertulis dan lisan. Anda dapat menggunakan NLP untuk membangun solusi yang mengekstraksi makna semantik dari teks atau ucapan, atau yang merumuskan tanggapan yang berarti dalam bahasa alami.

Microsoft Azure *Cognitive Services* mencakup kemampuan analitik teks dalam layanan *Bahasa*, yang menyediakan beberapa kemampuan NLP unik, termasuk identifikasi frasa kunci dalam teks, dan klasifikasi teks berdasarkan sentimen.

Misalnya, misalkan organisasi *Perjalanan Margie* fiktif menganjurkan pelanggan mengirim ulasan tentang menginap di hotel. Anda dapat menggunakan layanan Bahasa untuk meringkas ulasan dengan mengekstraksi frasa kunci, menentukan ulasan mana yang positif dan mana yang negatif, atau menganalisis teks ulasan untuk menyebutkan entitas yang dikenal seperti lokasi atau orang.

Untuk menguji kemampuan layanan Bahasa, kami akan menggunakan aplikasi baris perintah sederhana yang berjalan di Cloud Shell. Prinsip dan fungsi yang sama berlaku dalam solusi dunia nyata, seperti situs web atau aplikasi ponsel.

## Membuat sumber daya Azure App Service

Anda dapat menggunakan layanan Bahasa dengan membuat sumber daya **Bahasa** atau sumber daya **Cognitive Services**.

Jika Anda belum melakukannya, buat sumber daya **Layanan bahasa** di langganan Azure Anda.

1. Di tab browser lain, buka portal Microsoft Azure di [https://portal.azure.com](https://portal.azure.com?azure-portal=true), masuk dengan akun Microsoft Anda.

1. **Klik &#65291; Buat tombol sumber daya** dan cari *layanan* Azure AI. Pilih **buat** **paket layanan** Azure AI. Anda akan dibawa ke halaman untuk membuat sumber daya layanan Azure AI. Konfigurasikan PuTTY dengan pengaturan berikut:
    - **Langganan**: *Langganan Azure Anda*.
    - **Grup sumber daya**: *Pilih atau buat grup sumber daya dengan nama unik*.
    - **Wilayah**: *Pilih wilayah yang tersedia*.
    - **Nama**: *Masukkan nama unik*.
    - **Tingkat harga**: Standar S0
    - **Dengan mencentang kotak ini, saya menyatakan bahwa saya telah membaca dan memahami semua persyaratan di bawah**: Dipilih.

1. Tinjau dan buat sumber daya.

### Mendapatkan kunci dan titik akhir untuk sumber daya Cognitive Services Anda

1. Tunggu hingga penerapan selesai. Kemudian buka sumber daya Cognitive Services Anda, dan pada halaman **Gambaran umum**, klik tautan untuk mengelola kunci layanan. Anda akan memerlukan titik akhir dan kunci untuk terhubung ke sumber daya Cognitive Services dari aplikasi klien.

1. Lihat halaman **Kunci dan Titik Akhir** untuk sumber daya Anda. Anda memerlukan **kunci** dan **titik akhir** untuk terhubung dari aplikasi klien.

## Menjalankan Cloud Shell

Untuk menguji kemampuan analitik teks dari layanan Bahasa, kami akan menggunakan aplikasi baris perintah sederhana yang berjalan di Cloud Shell di Azure.

1. Di portal Microsoft Azure, pilih tombol **[>_]** (*Cloud Shell*) di bagian atas halaman di sebelah kanan kotak pencarian. Tindakan ini akan membuka panel Cloud Shell di bagian bawah portal.

    ![Mulai Cloud Shell dengan mengeklik ikon di sebelah kanan kotak pencarian di atas](media/analyze-text-language-service/powershell-portal-guide-1.png)

1. Saat pertama kali membuka Cloud Shell, Anda mungkin diminta untuk memilih jenis shell yang ingin digunakan (*Bash* atau *PowerShell*). Pilih **PowerShell**. Jika Anda tidak melihat opsi ini, lewati langkah ini.  

1. Jika Anda diminta membuat penyimpanan untuk Cloud Shell, pastikan langganan ditentukan dan pilih **Buat penyimpanan**. Kemudian tunggu sekitar satu menit hingga penyimpanan dibuat.

    ![Buat penyimpanan dengan mengklik konfirmasi.](media/analyze-text-language-service/powershell-portal-guide-2.png)

1. Pastikan jenis shell yang ditunjukkan di kiri atas panel Cloud Shell dialihkan ke *PowerShell*. Jika *Bash*, alihkan ke *PowerShell* dengan menggunakan menu drop-down.

    ![Cara menemukan menu drop down sebelah kiri untuk beralih ke PowerShell](media/analyze-text-language-service/powershell-portal-guide-3.png)

1. Tunggu PowerShell untuk memulai. Anda akan melihat layar berikut di portal Microsoft Azure:  

    ![Tunggu PowerShell untuk memulai.](media/analyze-text-language-service/powershell-prompt.png)

## Mengonfigurasi dan menjalankan aplikasi klien

Setelah memiliki model khusus, Anda dapat menjalankan aplikasi klien sederhana yang menggunakan layanan Bahasa.

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

    ![Editor kode.](media/analyze-text-language-service/powershell-portal-guide-4.png)

1. Di panel **File** di sebelah kiri, perluas **ai-900** dan pilih **analyze-text.ps1**. File ini berisi beberapa kode yang menggunakan layanan Bahasa:

    ![Editor yang berisi kode untuk menggunakan layanan Bahasa](media/analyze-text-language-service/analyze-text-code.png)

1. Jangan terlalu memikirkan detail kode. Menavigasi ke sumber daya App Service Anda di portal Azure Lalu pilih halaman **Kunci dan Titik Akhir** di panel sebelah kiri. Salin kunci dan titik akhir dari halaman dan tempelkan ke dalam editor kode, menggantikan nilai tempat penampung **YOUR_KEY** dan **YOUR_ENDPOINT** masing-masing.

    > **Tips** Anda mungkin perlu menggunakan bilah pemisah untuk menyesuaikan area layar saat bekerja dengan panel **Tombol dan Titik Akhir** serta **Editor**.

    ![Temukan tab kunci dan titik akhir di panel sebelah kiri sumber Cognitive Services Anda.](media/analyze-text-language-service/key-endpoint-support.png)

    Setelah mengganti nilai kunci dan titik akhir, baris pertama kode harus sama dengan ini:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."
    $endpoint="https..."
    ```

1. Di kanan atas panel editor, gunakan tombol **...** untuk membuka menu dan pilih **Simpan** untuk menyimpan perubahan Anda. Kemudian, buka menu lagi dan pilih **Tutup Editor**.

    Aplikasi klien sampel akan menggunakan layanan Bahasa Cognitive Services untuk mendeteksi bahasa, mengekstrak frasa kunci, menentukan sentimen, dan mengekstrak entitas yang dikenal untuk ditinjau.

1. Di Cloud Shell, masukkan perintah berikut untuk menjalankan kode:

    ```PowerShell
    cd ai-900
    ./analyze-text.ps1 review1.txt
    ```

    Anda akan meninjau teks ini:

    >Hotel dan Staf yang Baik Royal Hotel, London, Inggris 3/2/2018 Kamar bersih, layanan yang baik, lokasi yang bagus di dekat Istana Buckingham dan Westminster Abbey, dan sebagainya. Kami sangat menikmati saat menginap. Halamannya sangat tenang dan kami pergi ke restoran yang merupakan bagian dari kelompok yang sama dan restoran India (banyak ikan di pantai) dengan Mechelin Star. Kami disuguhi menu pencicip yang luar biasa. Kamar ditata dengan sangat baik disertai dapur, ruang santai, kamar tidur, dan kamar mandi yang sangat besar. Sangat direkomendasikan.

1. Tinjau output.

1. Di panel PowerShell, masukkan perintah berikut untuk menjalankan kode:

    ```PowerShell
    ./analyze-text.ps1 review2.txt
    ```

    Anda akan meninjau teks ini:

    >Hotel lama dengan pelayanan yang buruk The Royal Hotel, London, Inggris Raya 5/6/2018 Ini adalah hotel tua (sudah berdiri sejak tahun 1950-an) dan perabotan kamarnya rata-rata - sudah terlalu tua untuk digunakan di zaman sekarang dan perlu diganti. Internet tidak berfungsi dan harus datang ke salah satu ruang kantor mereka untuk check-in penerbangan pulang saya. Situs web mengatakan bahwa kantor ini dekat dengan British Museum, tetapi terlalu jauh dengan jalan kaki.

1. Tinjau output.

1. Di panel PowerShell, masukkan perintah berikut untuk menjalankan kode:

    ```PowerShell
    ./analyze-text.ps1 review3.txt
    ```

    Anda akan meninjau teks ini:

    >Lokasi yang bagus dan staf yang membantu, tapi di jalan yang sibuk.
    Lombard Hotel, San Francisco, AS 8/16/2018 Kami menginap di sini pada bulan Agustus setelah membaca ulasan. Kami sangat senang dengan lokasi, tepat di belakang Chestnut Street, daerah kosmopolitan dan trendi dengan banyak pilihan restoran. Distrik Marina sangat indah dilalui, rumah-rumah yang sangat menarik. Pastikan untuk Anda menelusuri San Francisco Museum of Fine Arts dan Marina untuk mendapatkan pemandangan jembatan Golden Gate dan kota yang indah. Di rute bus dan mudah masuk ke pusat. Kamarnya bersih dengan banyak ruang dan stafnya ramah dan membantu. Satu-satunya kelemahannya adalah suara bising dari Lombard Street sehingga meminta kamar yang paling jauh dari kebisingan lalu lintas.

1. Tinjau output.

1. Di panel PowerShell, masukkan perintah berikut untuk menjalankan kode:

    ```PowerShell
    ./analyze-text.ps1 review4.txt
    ```

    Anda akan meninjau teks ini:

    >Sangat bising dan kamarnya kecil The Lombard Hotel, San Francisco, USA 9/5/2018 Hotel ini berada di jalan Lombard yang merupakan jalan ENAM lajur yang sangat sibuk langsung dari Jembatan Golden Gate. Lalu lintas dari pagi hingga larut malam terutama di akhir pekan. Kebisingan tidak akan begitu buruk jika kamarnya terisolasi dengan baik, tetapi faktanya tidak. Harus meletakkan bola kapas di telinga agar bisa tertidur - terlalu lelah untuk menikmati kota keesokan harinya. Kamarnya kecil. Saya memilih kamar ini karena berisi dua tempat tidur ukuran besar - tetapi kamarnya hampir kehabisan ruang untuk tipe tempat tidur tersebut. Kamarnya terlalu sempit untuk keluarga empat orang. Kesimpulannya, kamarnya bersih dan mereka telah berusaha untuk memperbaruinya. Hotel ini berada di distrik Marina dengan banyak tempat makan yang bagus, dan sangat dekat dengan Presidio. Hotel ini mungkin cocok bagi remaja dengan anggaran terbatas

1. Tinjau output.

## Pelajari lebih lanjut

Aplikasi sederhana ini hanya menunjukkan beberapa kemampuan layanan Bahasa. Untuk mempelajari selengkapnya tentang apa yang dapat Anda lakukan dengan layanan ini, lihat [halaman layanan Bahasa](https://azure.microsoft.com/services/cognitive-services/language-service/).

