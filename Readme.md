# DATA-ENGINEERING  
# Proyek : Pengaruh Luas Kebakaran Hutan dan Lahan terhadap Peningkatan Emisi Karbon (CO₂e) di Indonesia

## Deskripsi Proyek  
Project ini di kembangkan untuk mengidentifikasi pola hubungan antara luas kebakaran hutan dan lahan dengan peningkatan emisi karbon (CO₂e) di Indonesia. Dengan Tujuan memahami kontribusi kebakaran hutan terhadap emisi karbon dan menentukan tingkat emisi berdasarkan luas lahan, luas kebakaran, dan jumlah emisi karbon. Selain itu, Project ini juga menganalisis perubahan luas kebakaran hutan dan emisi karbon per provinsi serta mengidentifikasi wilayah dengan tingkat emisi tinggi akibat kebakaran hutan beserta pola perubahannya setiap tahun

---

## Manfaat Data / Use Case  
- **Tujuan Proyek:** Menyediakan data terintegrasi yang menggambarkan pola hubungan antara luas kebakaran hutan dan lahan dengan peningkatan emisi karbon (CO₂e) di Indonesia.
- **Manfaat:**  
  - Menyediakan sumber data yang telah melalui proses validasi dan transformasi, sehingga siap digunakan untuk studi lanjutan.  
  - Membuka peluang bagi pengembang teknologi prediktif, seperti model machine learning untuk mitigasi bencana 
  - Hasil ETL proyek ini mendukung dashboard visualisasi sebagai upaya meningkatkan efisiensi analisis data lingkungan

---

## Serving Analisis  
Data hasil ETL disimpan dalam format Postgresql yang bersih dan terstruktur. Data ini siap digunakan untuk analisis eksploratif serta visualisasi tren menggunakan perangkat lunak seperti Looker untuk menemukan pola dan korelasi.

## Serving Machine Learning  
Dataset yang telah dibersihkan dan distandardisasi digunakan untuk melatih model machine learning. Secara spesifik, berbagai model regresi dievaluasi menggunakan pustaka pycaret untuk menemukan model terbaik dalam memprediksi jumlah pertumbuhan populasi. Model Huber Regressor terpilih sebagai model dengan performa terbaik untuk tugas prediksi ini.

---

# Pipeline
## Extract ( Pengambilan Data ) 
- **Sumber Data:**  
  - Luas Kebakaran Hutan dan Lahan  – SISKLHK
    (https://statistik.menlhk.go.id/sisklhkX/data_statistik/ppi/table7_6) 
  - Emisi Karbon (Ton CO2e) Akibat Karhutla – Kaggle  
    (https://statistik.menlhk.go.id/sisklhkX/data_statistik/ppi/table7_8) 
  - Data Luas Tutupan Lahan - Google Earth Engine
    - Dataset: `ESA/WorldCover/v100`  
    - Administrasi Provinsi: `FAO/GAUL/2015/level1`

- **Metode Pengambilan:**  
**GeoSpatial (Google Earth Engine):**  
    - Menggunakan Earth Engine API untuk menghitung luas tutupan lahan berdasarkan kelas seperti hutan, pertanian, semak belukar, dll.  
    - Proses melibatkan reduksi histogram klasifikasi citra dan konversi hasil ke hektar untuk masing-masing provinsi.  
  - **Web Scraping (Statistik KLHK):**  
    - Data emisi dan kebakaran diambil dari tabel HTML publik menggunakan `pandas.read_html()` dengan kata kunci “Provinsi”.  
---

## Transform ( Pembersihan & Transformasi )   
- **Pembersihan:**  
  - Menghapus baris duplikat dan baris kosong menggunakan metode seperti dropna().  
  - Mengganti nama kolom agar seragam untuk proses penggabungan (contoh: country menjadi Entity dan year menjadi Year).

- **Transformasi:**  
  - Menggabungkan beberapa dataset (dfHDI, dfMentalHealth, dfKematian, dfGDP) menjadi satu DataFrame tunggal berdasarkan kolom Year dan Entity.  
  - Data mentah disimpan sebelum transformasi dan data yang telah bersih disimpan sebagai output akhir.

---

## Load ( Pemindahan ke Target ) 
- **Target:**  
  - Sebuah tabel baru di dalam database pada server Aiven. Tabel ini merupakan output utama yang dapat diakses oleh layanan lain untuk melakukan analisis langsung di database.

- **Metode:**  
  - Fungsi to_sql() dari pandas digunakan untuk menulis data dari DataFrame langsung ke tabel di database PostgreSQL
  - konfigurasi fungsi to_sql() diatur dengan parameter-parameter kunci:
    - name diisi dengan yang mendefinisikan nama tabel tujuan
    - con diisi dengan variabel engine, yaitu objek koneksi dari SQLAlchemy
      yang telah dikonfigurasi sebelumnya untuk terhubung ke database Aiven
  - Data diverifikasi dengan membaca 5 baris pertama dari tabel baru tersebut menggunakan pd.read_sql() dan df.head()

---

## Arsitektur / Workflow ETL  
- **Alur Modular:**  
  - Proses ETL diringkas dalam sebuah fungsi transformasi() yang mencakup langkah-langkah membaca, membersihkan, mengisi, menggabungkan, dan mengubah data.
  -  Kode diorganisir secara sekuensial di dalam notebook Google Colab.

- **Tools yang Digunakan:**  
  - Python 3.x  
  - Library: `pandas`, `numpy`, `wget`, `kaggle`, `os`, `json`, `logging`, `scikit-learn`, `matplotlib`.
  - Google Colab digunakan sebagai environment untuk pemrosesan dan eksperimen.

---

## Kode Program  
- **Struktur Kode:**  
  - Kode untuk ETL dan Machine Learning dipisahkan dalam dua notebook yang berbeda.
  - Penamaan variabel dan fungsi bersifat deskriptif (contoh: dfHDI, df_agg, transformasi).
    
- **Machine Learning:**  
  - Menggunakan pycaret untuk melakukan setup environment, membandingkan beberapa model regresi, dan memilih model terbaik (huber) untuk prediksi Populasi.  
  - Model dievaluasi menggunakan berbagai plot seperti plot residual dan kesalahan prediksi.  

- **Link Projek:**  
  - ETL Pipeline:  
  - Machine Learning:  
  - Looker

---

## Kontributor

| Nama Lengkap                       | NIM         | Peran                |
|------------------------------------|-------------|----------------------|
| Brandewa Pandu A                   | 234311034   | Data Engineer        |
| Hanan Labib R                      | 234311041   | Data Analyst         |
| Muhammad Hasanuddin                | 234311045   | Data Engineer        |
| Rara Alviana G                     | 234311050   | Project Manager      |
