---
lab:
  title: Menjelajahi terjemahan
---

# Menjelajahi terjemahan

> **Catatan** Untuk menyelesaikan lab ini, Anda memerlukan [langganan Azure](https://azure.microsoft.com/free?azure-portal=true) dengan akses administrator.

Salah satu kekuatan pendorong yang telah memungkinkan peradaban manusia untuk berkembang adalah kemampuan untuk berkomunikasi satu sama lain. Dalam kebanyakan usaha manusia, kuncinya adalah komunikasi.

Kecerdasan Buatan (AI) dapat membantu menyederhanakan komunikasi dengan menerjemahkan teks atau ucapan antar bahasa, sehingga membantu menyingkirkan hambatan komunikasi lintas-negara dan budaya.

Untuk menguji kemampuan layanan Penerjemah, kami akan menggunakan aplikasi baris perintah sederhana yang berjalan di Cloud Shell. Prinsip dan fungsi yang sama berlaku dalam solusi dunia nyata, seperti situs web atau aplikasi ponsel.

## Membuat sumber daya *layanan Azure AI*

Anda dapat menggunakan layanan Penerjemah dengan membuat sumber daya **Penerjemah** atau sumber daya **layanan Azure AI** .

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

1. Lihat halaman **Kunci dan Titik Akhir** untuk sumber daya layanan Azure AI Anda. Anda akan membutuhkan kunci dan lokasi untuk terhubung dari aplikasi klien.

### Mendapatkan Kunci dan Lokasi untuk sumber daya layanan Azure AI Anda

1. Tunggu hingga penerapan selesai. Lalu buka sumber daya layanan Azure AI Anda, dan pada halaman **Gambaran Umum** , pilih tautan untuk mengelola kunci layanan. Anda akan memerlukan kunci dan lokasi untuk menyambungkan ke sumber daya layanan Azure AI Anda dari aplikasi klien.

1. Lihat halaman **Kunci dan Titik Akhir** untuk sumber daya Anda. Anda memerlukan **lokasi/wilayah** dan **kunci** untuk terhubung dari aplikasi klien.

> **Catatan** Untuk menggunakan layanan Penerjemah, Anda tidak perlu menggunakan titik akhir layanan Azure AI. Titik akhir global hanya disediakan untuk layanan Penerjemah. 

## Menjalankan Cloud Shell

Untuk menguji kemampuan layanan Terjemahan, kita akan menggunakan aplikasi baris perintah sederhana yang berjalan di Cloud Shell di Azure. 

1. Di portal Azure, pilih tombol **[>_]** (*Cloud Shell*) di bagian atas halaman di sebelah kanan kotak pencarian. Tindakan ini akan membuka panel Cloud Shell di bagian bawah portal.

    ![Mulai Cloud Shell dengan mengeklik ikon di sebelah kanan kotak pencarian di atas](media/translate-text-and-speech/powershell-portal-guide-1.png)

1. Saat pertama kali membuka Cloud Shell, Anda mungkin diminta untuk memilih jenis shell yang ingin digunakan (*Bash* atau *PowerShell*). Pilih **PowerShell**. Jika Anda tidak melihat opsi ini, lewati langkah ini.  

1. Jika Anda diminta membuat penyimpanan untuk Cloud Shell, pastikan langganan ditentukan dan pilih **Buat penyimpanan**. Kemudian tunggu sekitar satu menit hingga penyimpanan dibuat.

    ![Buat penyimpanan dengan mengklik konfirmasi.](media/translate-text-and-speech/powershell-portal-guide-2.png)

1. Pastikan jenis shell yang ditunjukkan di kiri atas panel Cloud Shell dialihkan ke *PowerShell*. Jika *Bash*, alihkan ke *PowerShell* dengan menggunakan menu drop-down. 

    ![Cara menemukan menu drop down sebelah kiri untuk beralih ke PowerShell](media/translate-text-and-speech/powershell-portal-guide-3.png) 

1. Tunggu PowerShell untuk memulai. Anda akan melihat layar berikut di portal Microsoft Azure:  

    ![Tunggu PowerShell untuk memulai.](media/translate-text-and-speech/powershell-prompt.png)

## Mengonfigurasi dan menjalankan aplikasi klien

Setelah Anda memiliki model kustom, Anda dapat menjalankan aplikasi klien sederhana yang menggunakan layanan Terjemahan.

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

    ![Editor kode.](media/translate-text-and-speech/powershell-portal-guide-4.png)

1. Di panel **File** di sebelah kiri, perluas **ai-900** dan pilih **translator.ps1**. File ini berisi beberapa kode yang menggunakan layanan Penerjemah:

    ![Editor yang berisi kode untuk menggunakan layanan Penerjemah](media/translate-text-and-speech/translate-code.png)

1. Jangan terlalu khawatir tentang detail kode, yang penting adalah memerlukan wilayah/lokasi dan salah satu kunci untuk sumber daya layanan Azure AI Anda. Salin ini dari halaman **Kunci dan Titik Akhir** untuk sumber daya Anda dari portal Microsoft Azure dan tempel ke penyunting kode, menggantikan nilai tempat penampung **YOUR_KEY** dan **YOUR_LOCATION**.

    Setelah menempelkan nilai kunci dan lokasi, baris pertama kode akan terlihat seperti ini:

    ```PowerShell
    $key="1a2b3c4d5e6f7g8h9i0j...."
    $location="somelocation"
    ```

1. Di kanan atas panel editor, gunakan tombol **...** untuk membuka menu dan pilih **Simpan** untuk menyimpan perubahan Anda. Kemudian, buka menu lagi dan pilih **Tutup Editor**.

    Contoh aplikasi klien akan menggunakan layanan Penerjemah untuk melakukan beberapa tugas:
    - Menerjemahkan teks dari bahasa Inggris ke dalam bahasa Prancis, Italia, dan Tionghoa.
    - Menerjemahkan audio dari bahasa Inggris ke dalam teks dalam bahasa Prancis

    Gunakan pemutar video di bawah ini untuk mendengarkan audio input yang akan diproses aplikasi:

    <div class="embeddedvideo"><iframe src="https://www.microsoft.com/videoplayer/embed/RWORN0" frameborder="0" allowfullscreen="true" data-linktype="external"></iframe></div>


    > **Catatan** Aplikasi sebenarnya dapat menerima input dari mikrofon dan mengirim respons ke speaker, namun dalam contoh sederhana ini, kami akan menggunakan input yang direkam sebelumnya dalam file audio.

1. Di panel Cloud Shell, masukkan perintah berikut untuk menjalankan kode:

    ```PowerShell
    cd ai-900
    ./translator.ps1
    ```

1. Tinjau output. Apakah Anda melihat terjemahan dari teks dalam bahasa Inggris ke Bahasa Prancis, Italia, dan Tionghoa?  Apakah Anda melihat audio berbahasa Inggris "hello" diterjemahkan ke dalam teks bahasa Prancis?

## Pelajari lebih lanjut

Aplikasi sederhana ini hanya menampilkan beberapa kemampuan layanan Penerjemah. Untuk mempelajari lebih lanjut tindakan yang dapat Anda lakukan dengan layanan ini, lihat [Halaman Penerjemah](https://docs.microsoft.com/azure/cognitive-services/translator/translator-overview).