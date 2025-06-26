---
title: Data Mining Project
date: 2025-06-24
categories: [Data Mining]
tags: [Regression, Algorithm, project]
---

# Pengembangan Model Prediksi Harga Sewa Kos di Makassar Menggunakan Teknik Data Mining

## Ringkasan

> Penelitian ini mengembangkan model prediksi harga sewa kos di Kota Makassar untuk mengatasi masalah penetapan harga yang subjektif. Dengan menggunakan dataset 1162 listing dari Mamikos.com, beberapa model regresi dievaluasi. Dilakukan rekayasa fitur untuk mengubah data fasilitas menjadi numerik dan membuat fitur jarak dari kos ke kampus-kampus utama. Ditemukan bahwa penanganan outlier harga dengan metode Interquartile Range (IQR) secara signifikan meningkatkan performa model. Model terbaik, XGBoost Regressor, berhasil mencapai Mean Absolute Error (MAE) sebesar Rp219.420 dan R-squared 0,606, yang berarti model dapat menjelaskan 60,6% variasi harga. Fasilitas seperti AC, kloset duduk, dan tipe kos campur menjadi faktor penentu harga paling signifikan. Hasil penelitian ini adalah sebuah prototipe alat bantu keputusan untuk membantu pemilik properti menetapkan harga sewa yang lebih objektif dan kompetitif.

---

## 1. Pendahuluan

* **Latar Belakang:** Kota Makassar adalah pusat pendidikan dan ekonomi di Indonesia Timur, yang menyebabkan tingginya permintaan kamar kos dari mahasiswa dan pekerja muda. Platform digital seperti [Mamikos.com](https://mamikos.com/) memperketat persaingan, namun banyak pemilik kos masih menetapkan harga secara subjektif berdasarkan intuisi. Hal ini berisiko menyebabkan *overpricing* (kamar kosong) atau *underpricing* (kehilangan pendapatan).
* **Masalah:** Belum ada alat bantu berbasis data yang spesifik untuk membantu pemilik kos di Makassar dalam menentukan harga secara objektif.
* **Tujuan & Pertanyaan Penelitian:** Penelitian ini bertujuan mengembangkan model prediksi harga sewa kos di Makassar dengan menjawab pertanyaan berikut:
    1.  Faktor (fasilitas, tipe, lokasi) apa yang paling signifikan mempengaruhi harga?
    2.  Bagaimana membangun model prediksi yang andal dari data publik?
    3.  Seberapa efektif model ini dalam memberikan estimasi harga yang kompetitif?

---

## 2. Tinjauan Pustaka

Berbagai penelitian telah membuktikan keberhasilan *machine learning* dalam prediksi harga properti.
* Studi oleh **Fitri (2023)** menemukan Random Forest adalah model terbaik untuk prediksi harga rumah di Jakarta Selatan (R-squared = 0,818).
* Penelitian oleh **Wisnuadhi (2021)** menggunakan XGBoost untuk prediksi harga sewa Airbnb di Berlin dan menemukan tipe kamar sebagai faktor utama (R-squared = 0,74).
* Studi spesifik pasar kos di Indonesia oleh **Christian dan Herman (2023)** di Batam menggunakan Random Forest (akurasi 90%), sementara **Al Hanif dkk. (2023)** di Yogyakarta membuktikan bahwa jarak ke kampus dan fasilitas berkorelasi signifikan dengan harga.
* **Kesenjangan Penelitian:** Belum ada model prediksi harga kos yang spesifik untuk pasar Makassar dengan memanfaatkan fitur jarak spasial ke kampus. Penelitian ini bertujuan mengisi kesenjangan tersebut.
* **Model yang Dipilih:** **XGBoost** dipilih karena merupakan implementasi *gradient boosting* yang efisien, memiliki performa tinggi, dan mampu mencegah *overfitting* serta menangani *missing values*.

---

## 3. Metodologi

Penelitian ini menggunakan pendekatan kuantitatif dengan alur kerja yang sistematis, diimplementasikan menggunakan bahasa pemrograman Python dan beberapa pustaka utama seperti Pandas, Scikit-learn, XGBoost, Geopy, dan Selenium.

### A. Pengumpulan Data
* Data primer dikumpulkan melalui *web scraping* dari situs Mamikos.com untuk wilayah Kota Makassar.
* Awalnya terkumpul 1.162 data, setelah dibersihkan dari duplikasi diperoleh 562 data unik.
* Atribut yang diambil meliputi: harga sewa bulanan, lokasi (kecamatan), tipe kos, dan daftar fasilitas.

### B. Pra-pemrosesan dan Rekayasa Fitur
Tahap ini bertujuan mengubah data mentah agar dapat diproses oleh model *machine learning*.
1.  **Encoding Fasilitas:** Daftar fasilitas teks diubah menjadi fitur biner (nilai 1 atau 0 untuk setiap fasilitas) menggunakan `MultiLabelBinarizer`.
2.  **Encoding Tipe Kos:** Variabel kategori 'Tipe Kos' diubah menjadi representasi numerik menggunakan `One-Hot Encoding`.
3.  **Rekayasa Fitur Spasial:** Nama kecamatan diubah menjadi koordinat geografis (lintang & bujur) menggunakan Geopy. Selanjutnya, jarak (dalam km) dihitung dari setiap kos ke beberapa kampus utama di Makassar. Fitur jarak ini merepresentasikan nilai strategis lokasi.

### C. Penanganan Outlier
* Analisis data menunjukkan adanya harga sewa yang sangat ekstrem (outlier) yang dapat mengganggu pelatihan model.
* Metode *Interquartile Range (IQR)* digunakan untuk mendeteksi dan menghapus outlier tersebut.
* Batas atas ditentukan dengan rumus: `Batas Atas = Q3 + 1.5 * IQR`. Data di atas batas ini dihapus.

### D. Pengembangan Model
* Dua model dievaluasi: **Random Forest Regressor** dan **XGBoost Regressor**.
* Dataset dibagi menjadi 80% data latih dan 20% data uji.
* Performa model diukur menggunakan tiga metrik:
    * **Mean Absolute Error (MAE):** Rata-rata selisih absolut antara harga prediksi dan aktual.
    * **Root Mean Squared Error (RMSE):** Serupa dengan MAE, namun memberi "hukuman" lebih besar pada kesalahan yang ekstrem.
    * **R-squared :** Proporsi varians harga yang dapat dijelaskan oleh model (semakin mendekati 1, semakin baik).

---

## 4. Hasil dan Analisis

### A. Dampak Penanganan Outlier
Langkah penanganan outlier terbukti esensial untuk menghasilkan model yang lebih stabil dan andal. Semua hasil evaluasi berikut didasarkan pada dataset yang telah dibersihkan dari outlier.

### B. Evaluasi Model Final
Perbandingan kinerja model Random Forest dan XGBoost pada data uji disajikan pada tabel berikut.

**Tabel I: Perbandingan Performa Model Regresi Final**

| Model         | MAE (Rupiah) | RMSE (Rupiah) | R-squared |
|:--------------|:-------------|:--------------|:----------|
| Random Forest | 238.753      | 332.766       | 0,531     |
| XGBoost       | 219.420      | 302.442       | 0,606     |

Model **XGBoost Regressor** menunjukkan kinerja yang lebih unggul.
* **MAE sebesar Rp219.420** mengindikasikan rata-rata kesalahan prediksi model terhadap harga asli.
* **R-squared sebesar 0,606** menunjukkan model mampu menjelaskan **60,6%** dari variasi harga sewa kos.
Dengan performa terbaik, XGBoost dipilih sebagai model final.

### C. Faktor Paling Berpengaruh (Feature Importance)
Analisis dari model XGBoost menunjukkan kontribusi setiap fitur dalam memprediksi harga.
* **Fasilitas AC** secara absolut mendominasi sebagai prediktor utama (skor 0,607).
* Fitur signifikan lainnya secara berurutan adalah: **kloset duduk** (0,090), **tipe kos campur** (0,053), lokasi di **Kecamatan Ujung Pandang** (0,041), dan **Fasilitas Wifi** (0,033).

---

## 5. Diskusi

### A. Interpretasi Hasil dan Kontribusi
* Keunggulan XGBoost disebabkan oleh mekanismenya yang secara iteratif memperbaiki kesalahan model sebelumnya, sehingga mampu menangkap pola yang kompleks.
* Pentingnya penanganan outlier ditegaskan oleh peningkatan nilai R-squared yang signifikan setelah data dibersihkan.
* Kontribusi utama penelitian ini adalah pengembangan prototipe alat bantu keputusan berbasis data yang pertama kali spesifik untuk pasar sewa kos di Kota Makassar.

### B. Kelebihan dan Keterbatasan
* **Kelebihan:**
    1.  Model spesifik untuk pasar kos Makassar, mengisi kekosongan penelitian.
    2.  Rekayasa fitur spasial (jarak ke kampus) cukup efektif.
    3.  Luaran penelitian dirancang untuk bisa diimplementasikan menjadi aplikasi praktis.
* **Keterbatasan:**
    1.  Sumber data hanya dari satu platform (Mamikos.com) yang dapat menimbulkan bias.
    2.  Data bersifat *point-in-time* dan tidak menangkap fluktuasi harga musiman.
    3.  Presisi lokasi hanya berdasarkan kecamatan, bukan alamat pasti.

### C. Implikasi Praktis
* Bagi Pemilik Properti, Model ini dapat menjadi alat bantu untuk menetapkan harga yang objektif dan kompetitif, serta memberikan wawasan untuk keputusan investasi (misalnya, menambahkan AC terbukti berdampak signifikan pada harga).

---

## 6. Kesimpulan dan Saran

Penelitian ini berhasil mengembangkan model prediksi harga sewa kos di Makassar menggunakan XGBoost, yang mencapai **R-squared sebesar 0,606**. Faktor kunci yang paling berpengaruh adalah fasilitas **AC**, diikuti **kloset duduk**, dan **tipe kost campur**.

**Saran untuk Penelitian Selanjutnya:**
* Mengambil fitur yang lebih kaya dari halaman detail setiap listing (misal: luas kamar, detail fasilitas).
* Mengembangkan prototipe menjadi aplikasi interaktif dengan visualisasi SHAP.
* Menguji algoritma lain yang lebih kompleks seperti jaringan syaraf tiruan (MLP).
* Melakukan optimasi lebih lanjut untuk menurunkan nilai MAE dan RMSE melalui *hyperparameter tuning*.

---

### Ucapan Terima Kasih
Para penulis berterima kasih kepada Bapak Dr. Eng. Supri Bin Hj. Amir, S.Si., M.Eng. dan Bapak Octavian, S.Si., M.Kom. selaku dosen pengampu mata kuliah Data Mining, serta seluruh anggota tim peneliti atas kerja sama dan dedikasinya.

### Referensi
1. F. 1. Al Hanif, R. B. Hartarto, and I. Hajar, ["The Effect of Campus Existence on Boarding House Rental Prices: A Case Study of Universitas Muhammadiyah Yogyakarta,"](https://journal.umy.ac.id/index.php/jerss/article/view/19225) *Journal of Economics Research and Social Sciences*, vol. 7, no. 2, pp. 231-239, 2023.
2. R. R. Hallan and I. N. Fajri, ["Prediksi Harga Rumah menggunakan Machine Learning Algoritma Regresi Linier,"](https://jurnal.unidha.ac.id/index.php/jteksis/article/view/1732) *Jurnal Teknologi Dan Sistem Informasi Bisnis (JTeksis)*, vol. 7, no. 1, pp. 57-62, 2025.
3. Y. Christian and Herman, ["Rental Price Prediction of Boarding Houses in Batam City Using Linear Regression and Random Forest Algorithms,"](https://jurnal.polibatam.ac.id/index.php/JAIC/article/view/6732) *Journal of Applied Informatics and Computing (JAIC)*, vol. 7. no. 2, pp. 263-270, 2023.
4. E. Fitri, ["Analisis Perbandingan Metode Regresi Linier, Random Forest Regression dan Gradient Boosted Trees Regression Method untuk Prediksi Harga Rumah,"](https://journal.isas.or.id/index.php/JACOST/article/view/491) *Journal of Applied Computer Science and Technology (JACOST)*, vol. 4, no. 1, pp. 58-64, 2023.
5. B. Wisnuadhi and I. Setiawan, ["Rekomendasi Fitur yang Mempengaruhi Harga Sewa Menggunakan Pendekatan Machine Learning."](https://jtiik.ub.ac.id/index.php/jtiik/article/view/3305) *Jurnal Teknologi Informasi dan Ilmu Komputer (JTIIK)*, vol. 8, no. 4, pp. 673-682, 2021.
6. K.-W. Lee, S. N. Njimbouom, and J.-D. Kim, ["Development of Rent House Price Prediction Service based on Machine Learning,"](http://journal.dcs.or.kr/_common/do.php?a=full&b=12&bidx=3170&aidx=35373) *Journal of Digital Contents Society*, vol. 23, no. 12, pp. 2445-2455, 2022.

## 7. Link Eksternal
* Repository github : [Klik disini](https://github.com/muhamadtegar1/Kostimator)