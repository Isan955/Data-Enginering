# DATA-ENGINEERING  
# Proyek : Analisis Dampak Tingkat HDI dan GDP terhadap Pertumbuhan dan Dinamika Populasi di Berbagai Negara

## Deskripsi Proyek  
Proyek ini bertujuan untuk mengembangkan pipeline ETL (Extract, Transform, Load) untuk menganalisis korelasi antara Indeks Pembangunan Manusia (HDI), Produk Domestik Bruto (GDP), dengan tren angka bunuh diri secara global. Data yang relevan dikumpulkan dari berbagai sumber, kemudian dibersihkan, diintegrasikan, dan disiapkan untuk analisis lebih lanjut serta untuk melatih model machine learning. Hasil akhir dari proyek ini adalah sebuah dataset yang siap pakai sebagai dasar untuk pengambilan kebijakan berbasis data terkait kesehatan mental dan pembangunan sosial-ekonomi. 

---

## Manfaat Data / Use Case  
- **Tujuan Proyek:** Menyediakan data terintegrasi yang menghubungkan indikator sosial-ekonomi (HDI, GDP) dengan data demografi terkait kasus bunuh diri dan pertumbuhan populasi.  
- **Manfaat:**  
  - Menjelaskan hubungan antara pembangunan dan dinamika populasi.  
  - Menjadi dasar untuk model prediktif guna memproyeksikan tren populasi di masa depan berdasarkan variabel sosial-ekonomi.  
  - Menyediakan insight dan rekomendasi berbasis data untuk pemerintah, lembaga kesehatan mental, dan organisasi perencanaan pembangunan.

---

## Serving Analisis  
Data hasil ETL disimpan dalam format Postgresql yang bersih dan terstruktur. Data ini siap digunakan untuk analisis eksploratif serta visualisasi tren menggunakan perangkat lunak seperti Looker untuk menemukan pola dan korelasi.

## Serving Machine Learning  
Dataset yang telah dibersihkan dan distandardisasi digunakan untuk melatih model machine learning. Secara spesifik, berbagai model regresi dievaluasi menggunakan pustaka pycaret untuk menemukan model terbaik dalam memprediksi jumlah pertumbuhan populasi. Model Huber Regressor terpilih sebagai model dengan performa terbaik untuk tugas prediksi ini.

---

# Pipeline
## Extract ( Pengambilan Data ) 
- **Sumber Data:**  
  - Mental Health – Kaggle  
    https://www.kaggle.com/datasets/imtkaggleteam/mental-health  
  - Suicide Rates – Kaggle  
    https://www.kaggle.com/datasets/omkargowda/suicide-rates-overview-1985-to-2021  
  - Gross Domestic Product - Our World in Data
    https://ourworldindata.org/grapher/gdp-per-capita-worldbank?tab=table
  - Human Development Index – Our World in Data  
    https://ourworldindata.org/grapher/human-development-index?time=latest

- **Metode Pengambilan:**  
  - File CSV diunduh dari URL menggunakan wget.  
  - File dari Kaggle diunduh menggunakan kaggle datasets download.  
  - Penanganan file ZIP yang diekstrak secara otomatis.  

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
    https://colab.research.google.com/drive/1Q6Ute3fIaVB_-GHH8ViZu3tbAU3BE0rA?usp=sharing
  - Machine Learning:  
    https://colab.research.google.com/drive/1jsvbjMqI8EXPhcuA_PQ3xgNSGhmaowEB?usp=sharing
  - Looker
    https://lookerstudio.google.com/reporting/b444a019-af01-4a6c-b313-9178138a915a

---

## Kontributor

| Nama Lengkap                       | NIM         | Peran                |
|------------------------------------|-------------|----------------------|
| Brandewa Pandu A                   | 234311034   | Data Engineer        |
| Hanan Labib R                      | 234311041   | Data Analyst         |
| Muhammad Hasanuddin                | 234311045   | Data Engineer        |
| Rara Alviana G                     | 234311050   | Project Manager      |
