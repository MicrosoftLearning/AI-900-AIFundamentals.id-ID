---
lab:
  title: Menjelajahi Pembelajaran Mesin Otomatis di Azure ML
---

# Menjelajahi Pembelajaran Mesin Otomatis di Azure ML

Di lab ini, Anda akan menjelajahi kapabilitas Pembelajaran Mesin Otomatis layanan Azure Machine Learning untuk melatih model pembelajaran mesin. Anda kemudian akan menyebarkan dan menguji model terlatih.

Perintah ini seharusnya memakan waktu sekitar 20 menit untuk menyelesaikannya.

> **Catatan** Untuk menyelesaikan lab ini, Anda memerlukan [langganan Azure](https://azure.microsoft.com/free) dengan akses administrator.

## Membuat ruang kerja Pembelajaran Mesin Microsoft Azure

Untuk menggunakan Azure Pembelajaran Mesin, Anda perlu memprovisikan ruang kerja Azure Pembelajaran Mesin di langganan Azure Anda. Kemudian Anda akan dapat menggunakan studio Azure Pembelajaran Mesin untuk bekerja dengan sumber daya di ruang kerja Anda.

> **Tips**: Jika Anda sudah memiliki ruang kerja Azure Pembelajaran Mesin, Anda dapat menggunakannya dan melompat ke tugas berikutnya.

1. Masuk ke portal Microsoft Azure menggunakan kredensial Microsoft Anda.

1. Pilih **&#65291;Buat sumber daya**, cari *Pembelajaran Mesin*, dan buat sumber daya **Pembelajaran Mesin** baru dengan pengaturan berikut:
    - **Langganan**: *Langganan Azure Anda*.
    - **Grup sumber daya**: *Buat atau pilih grup sumber daya*.
    - **Nama ruang kerja**: *Masukkan nama unik untuk ruang kerja Anda*.
    - **Wilayah**: *Pilih wilayah geografis terdekat*.
    - **Akun penyimpanan**: *Perhatikan akun penyimpanan default baru yang akan dibuat untuk ruang kerja Anda*.
    - **Key vault**: *Perhatikan key vault baru bawaan yang akan dibuat untuk ruang kerja Anda*.
    - **Application insights**: *Perhatikan sumber daya application insights baru bawaan yang akan dibuat untuk ruang kerja Anda*.
    - **Registri kontainer**: Tidak ada (*kontainer akan dibuat secara otomatis saat pertama kali menyebarkan model ke kontainer*).

1. Pilih **Tinjau + buat**, lalu pilih **Buat**. Tunggu hingga ruang kerja Anda dibuat (dapat memakan waktu beberapa menit), lalu buka sumber daya yang disebarkan.

1. Pilih **Luncurkan studio** (atau buka tab browser baru dan arahkan ke [https://ml.azure.com](https://ml.azure.com?azure-portal=true), dan masuk ke studio Azure Machine Learning menggunakan akun Microsoft Anda). Perhatikan pesan kesalahan apa pun yang ditampilkan.

1. Di studio Azure Machine Learning, Anda akan melihat ruang kerja yang baru dibuat. Jika tidak, pilih **Semua ruang** kerja di menu sebelah kiri lalu pilih ruang kerja yang baru saja Anda buat.

## Aktifkan fitur pratinjau

Beberapa fitur Azure Pembelajaran Mesin sedang dalam pratinjau, dan perlu diaktifkan secara eksplisit di ruang kerja Anda.

1. Di Azure Pembelajaran Mesin Studio, klik **kelola fitur** pratinjau (ikon speaker keras - &#128363;).

    ![Cuplikan layar tombol kelola fitur pratinjau pada menu.](../instructions/media/use-automated-machine-learning/severless-compute-1.png)

1. Aktifkan fitur pratinjau berikut:

    - *Pengalaman terpandu untuk mengirimkan pekerjaan pelatihan dengan komputasi tanpa server*

## Gunakan pembelajaran mesin otomatis untuk melatih  menyebarkan model

Pembelajaran mesin otomatis memungkinkan Anda mencoba beberapa algoritma dan parameter untuk melatih beberapa model, dan mengidentifikasi yang terbaik untuk data Anda. Dalam latihan ini, Anda akan menggunakan himpunan data detail persewaan sepeda historis untuk melatih model yang memprediksi jumlah persewaan sepeda yang diharapkan pada hari tertentu, berdasarkan fitur musiman dan meteorologi.

> **Kutipan**: *Data ini berasal dari [Capital Bikeshare](https://www.capitalbikeshare.com/system-data) dan digunakan sesuai dengan [perjanjian lisensi](https://www.capitalbikeshare.com/data-license-agreement)* data yang diterbitkan.


1. Di [studio Azure Machine Learning](https://ml.azure.com?azure-portal=true), lihat halaman **ML Otomatis** (di bawah **Penulis**).

1. Buat pekerjaan ML Otomatis baru dengan pengaturan berikut, menggunakan **Berikutnya** sesuai kebutuhan untuk maju melalui antarmuka pengguna:

    Pengaturan dasar:

    - **Nama** pekerjaan: mslearn-bike-automl
    - **Nama eksperimen baru**:mslearn-bike-rental
    - **Deskripsi**: Pembelajaran mesin otomatis untuk prediksi penyewaan sepeda
    - **** Tag: *tidak ada*

   **Jenis tugas & data**:

    - **Pilih jenis** tugas: Regresi
    - **Pilih himpunan** data: Buat himpunan data baru dengan pengaturan berikut:
        - **Jenis data**:
            - **Nama**: bike-rentals
            - **Deskripsi**: Data penyewaan sepeda historis
            - **Jenis**: Tabular
        - **Sumber Data**:
            - Pilih dari file
        - URL Web
            - **URL Web**: `https://aka.ms/bike-rentals`
            - **Lewati validasi data**: *jangan pilih*
        - **Pengaturan**:
            - **Format file**: Terbatas
            - **Pemisah**: Koma
            - **Pengodean**: UTF-8
            - **Header kolom**: Hanya file pertama yang memiliki header
            - **Lompati baris**: Tidak ada
            - **Himpunan data berisi data multibaris**: *jangan pilih*
        - **Skema**:
            - Sertakan semua kolom selain **Jalur**
            - Meninjau jenis yang terdeteksi secara otomatis

        Pilih himpunan **data penyewaan** sepeda setelah Anda membuatnya.

    **Pengaturan tugas**:

    - **Jenis** tugas: Regresi
    - **Himpunan data**: bike-rentals
    - **Kolom target**: Penyewaan (bilangan bulat)
    - **Setelan konfigurasi tambahan:**
        - Metrik utama: Pilih root mean squared error yang dinormalisasi
        - **Menjelaskan model** terbaik: *Tidak dipilih*
        - **Gunakan semua model yang didukung**: <u>Tidak</u>dipilih. *Anda akan membatasi pekerjaan untuk mencoba hanya beberapa algoritma tertentu.*
        - **Model yang diizinkan**: *Pilih hanya **RandomForest** dan **LightGBM** — biasanya Anda ingin mencoba sebanyak mungkin, tetapi setiap model yang ditambahkan akan menambah waktu yang dibutuhkan untuk menjalankan pekerjaan.*
    - **** Batas: *Perluas bagian ini*
        - **Uji coba maks**: 3
        - **Uji coba** serentak maks: 3
        - Node maksimal: 6
        - **Ambang skor metrik**: 0,085 — *jika model mencapai skor metrik root mean squared error yang dinormalisasi sebesar 0,085 atau kurang, pekerjaan akan berakhir.*
        - --timeout -t
        - **Batas waktu perulangan**: 5
        - **Aktifkan penghentian** dini: *Dipilih*
    - Pengujian dan Validasi
        - **Jenis** validasi: Pemisahan validasi pelatihan
        - **Persentase data** validasi: 10
        - **Himpunan** data pengujian: Tidak ada

    **Komputasi:**

    - **Pilih jenis komputasi**: Tanpa Server
    - **Jenis mesin virtual**: CPU
    - **Tingkatan mesin virtual**: Khusus
    - **Ukuran Komputer Virtual**: Standard_DS11_v2
    - Jumlah instans

1. Kirim pekerjaan pelatihan. Ini dimulai secara otomatis.

1. Tunggu pekerjaan selesai. Mungkin perlu waktu — sekarang mungkin saat yang tepat untuk istirahat sejenak!

## Meninjau model terbaik

Ketika pekerjaan pembelajaran mesin otomatis telah selesai, Anda dapat meninjau model terbaik yang dilatihnya.

1. Di tab **Ringkasan** tugas pembelajaran mesin otomatis, perhatikan ringkasan model terbaik.
    ![Cuplikan layar ringkasan model terbaik dari tugas pembelajaran mesin otomatis dengan kotak di sekitar nama algoritma.](media/use-automated-machine-learning/complete-run.png)

    > Anda mungkin melihat pesan di bawah status "Peringatan: Skor keluar yang ditentukan pengguna tercapai...". Ini adalah pesan yang diharapkan. Lanjutkan ke langkah berikutnya.
  
1. Pilih teks di bawah **Nama algoritma** untuk model terbaik guna melihat detailnya.

1. Pilih tab **Metrik** dan pilih bagan **residuals** dan **predicted_true** jika belum dipilih. 

    Tinjau grafik yang menunjukkan performa model. Bagan **residu** menunjukkan *residu* (perbedaan antara nilai yang diprediksi dan aktual) sebagai histogram. Bagan **predicted_true** membandingkan nilai yang diprediksi dengan nilai benar. 

## Terapkan dan uji model

1. pada tab **Model**, pilih tombol **Sebarkan** dan gunakan opsi **Sebarkan ke layanan web** untuk menyebarkan model dengan pengaturan berikut:
    - **Nama**: predict-rentals
    - **Deskripsi**: Memprediksi penyewaan sepeda
    - **Jenis komputasi**: Azure Container Instance
    - Aktifkan autentikasi: Dipilih

1. Tunggu penyebaran dimulai - ini mungkin memakan waktu beberapa detik. Status **** Sebarkan untuk **titik akhir predict-rentals** akan ditunjukkan di bagian utama halaman sebagai *Berjalan*.
1. Tunggu hingga status berubah menjadi Pelatihan berhasil. Ini mungkin akan membutuhkan waktu hingga 5 menit.

## Menguji layanan yang disebarkan

Sekarang Anda dapat menguji layanan yang disebarkan.

1. Di studio Azure Machine Learning, lihat halaman **Titik Akhir** dan pilih titik akhir realtime **predict-rentals**.

1. Pada laman **Titik Akhir**, buka titik akhir waktu nyata **predict-rentals**.

1. Di panel **Input data ke titik akhir real time pengujian**, ganti template JSON dengan data input berikut:

    ```JSON
    {
      "Inputs": { 
        "data": [
          {
            "day": 1,
            "mnth": 1,   
            "year": 2022,
            "season": 2,
            "holiday": 0,
            "weekday": 1,
            "workingday": 1,
            "weathersit": 2, 
            "temp": 0.3, 
            "atemp": 0.3,
            "hum": 0.3,
            "windspeed": 0.3 
          }
        ]    
      },   
      "GlobalParameters": 1.0
    }
    ```

1. Klik tombol **Uji**.

1. Tinjau hasil pengujian, yang mencakup perkiraan jumlah persewaan berdasarkan fitur input.

    ```JSON
    {
      "Results": [
        444.27799000000000
      ]
    }
    ```

    Panel pengujian mengambil data input dan menggunakan model yang Anda latih untuk menampilkan perkiraan jumlah persewaan.

Mari kita tinjau yang telah Anda lakukan. Anda menggunakan himpunan data tentang data penyewaan sepeda historis untuk melatih model. Model ini memprediksi jumlah penyewaan sepeda yang diharapkan pada hari tertentu, berdasarkan *fitur* musiman dan meteorologi.

## Pembersihan

Layanan web yang Anda buat dihost dalam *Azure Container Instance*. Jika tidak berniat untuk bereksperimen dengan ini lebih lanjut, Anda harus menghapus titik akhir untuk menghindari mengumpulkan penggunaan Azure yang tidak perlu.

1. Di [studio Azure Machine Learning](https://ml.azure.com?azure-portal=true), di tab **Titik Akhir**, pilih titik akhir **predict-rentals**. Kemudian pilih **Hapus** dan konfirmasikan bahwa Anda ingin menghapus titik akhir.

    Menghentikan komputasi memastikan langganan Anda tidak akan ditagih untuk sumber daya komputasi. Namun, Anda akan dikenakan biaya kecil untuk penyimpanan data selama ruang kerja Azure Machine Learning ada di langganan Anda. Jika telah selesai menjelajahi Azure Machine Learning, Anda dapat menghapus ruang kerja Azure Machine Learning dan sumber daya terkait.

Untuk menghapus ruang kerja Anda:

1. Di [portal Microsoft Azure](https://portal.azure.com?azure-portal=true), di halaman **Grup sumber daya**, buka grup sumber daya yang Anda tentukan saat membuat ruang kerja Azure Machine Learning Anda.
2. Klik **Hapus grup sumber daya**, ketik nama grup sumber daya untuk mengonfirmasi bahwa Anda ingin menghapusnya, dan pilih **Hapus**.
