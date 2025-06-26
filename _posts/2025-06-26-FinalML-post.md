---
title: Machine Learning Project
date: 2024-06-26
categories: [Machine Learning]
tags: [Regression, Algorithm, project]
---

# Prediksi Skor Kredit Menggunakan Metode Machine Learning

## Ringkasan

> Penilaian kredit merupakan aspek krusial di sektor keuangan untuk menilai risiko dan membuat keputusan pinjaman yang tepat. Penelitian ini mengeksplorasi penggunaan *machine learning* untuk prediksi skor kredit dengan membandingkan algoritma Random Forest Regressor dan XGBoost Regressor. Dataset yang digunakan berasal dari Kaggle, mencakup 100.000 data nasabah dengan 28 fitur. Proses penelitian meliputi *preprocessing* data, *cross-validation*, dan *tuning parameter* untuk optimasi model. Kinerja model dievaluasi menggunakan metrik MAE, MSE, RMSE, dan R². Hasilnya menunjukkan bahwa *tuning parameter* meningkatkan akurasi kedua model, namun XGBoost secara konsisten lebih unggul dengan nilai error lebih rendah dan R² lebih tinggi (0.598 vs 0.567 pada Random Forest). Dengan demikian, XGBoost direkomendasikan sebagai model yang lebih optimal untuk prediksi skor kredit.

-----

## 1\. Pendahuluan

  * **Latar Belakang:** Penilaian kredit adalah pilar penting dalam industri keuangan untuk mengevaluasi risiko pinjaman. Metode tradisional seringkali terbatas dalam menangani volume dan kompleksitas data modern.
  * **Peran Machine Learning:** Seiring kemajuan teknologi, *machine learning* (ML) menawarkan kemampuan analisis data yang lebih dalam untuk mengidentifikasi pola tersembunyi dan meningkatkan akurasi prediksi skor kredit.
  * **Tujuan Penelitian:** Penelitian ini bertujuan untuk menerapkan dan membandingkan kinerja beberapa algoritma *machine learning* dalam memprediksi skor kredit secara akurat, guna memberikan kontribusi bagi industri keuangan.

-----

## 2\. Metodologi Penelitian

Penelitian ini mengikuti alur kerja sistematis seperti berikut:

### 2.1 Pengumpulan Dataset

  * Dataset yang digunakan dalam penelitian ini diambil dari platform **Kaggle**.
  * Dataset ini terdiri dari **100.000 baris data** nasabah dengan **28 kolom (fitur)**.

### 2.2 Pembersihan Data (Data Cleaning)

Tahap ini dilakukan untuk memastikan kualitas data sebelum pemodelan.

  * **Menangani Nilai Hilang (*Missing Values*):** Nilai yang hilang diisi menggunakan metode imputasi (rata-rata untuk numerik, modus untuk kategorikal).
  * **Menangani Outliers:** Nilai ekstrem (outliers) diidentifikasi dan dihapus menggunakan metode *Interquartile Range* (IQR).
  * **Menangani Duplikasi & Fitur Tidak Relevan:** Data duplikat dan beberapa fitur yang dianggap tidak relevan dihapus untuk meningkatkan performa model.

### 2.3 Transformasi Data (Data Transformation)

Data mentah diubah menjadi format yang sesuai untuk model *machine learning*.

  * **Encoding Variabel Kategorikal:** Menggunakan *one-hot encoding* untuk kategori tidak berurutan dan *label encoding* untuk kategori berurutan.
  * **Mapping Skor Kredit:** Melakukan pemetaan pada variabel target (skor kredit) untuk mempermudah interpretasi.
  * **Normalisasi:** Menyamakan skala data pada semua kolom numerik ke dalam rentang 0-1.

### 2.4 Pelatihan & Pengujian Model

  * **Pembagian Data:** Dataset dibagi dengan proporsi **80% untuk data latih** dan **20% untuk data uji**.
  * **Algoritma yang Digunakan:** Model yang dibandingkan adalah **Random Forest Regressor** dan **XGBoost Regressor**.
  * **Validasi Model:** Menggunakan metode **3-Fold Cross Validation** untuk mengevaluasi performa model secara robust.

### 2.5 Evaluasi Model

Performa model diukur menggunakan beberapa metrik standar untuk regresi:

  * Mean Absolute Error (MAE)
  * Mean Squared Error (MSE)
  * Root Mean Squared Error (RMSE)
  * R-squared score

-----

## 3\. Hasil dan Analisis

### 3.1 Hasil Random Forest Regressor

Setelah dilakukan *tuning parameter*, model Random Forest menunjukkan sedikit peningkatan performa. Nilai R-squared meningkat dari 0.563 menjadi 0.567, yang mengindikasikan kemampuan model yang sedikit lebih baik dalam menjelaskan variasi data.

**Tabel Hasil Random Forest**

| Parameter | MAE   | MSE   | RMSE  | R-squared |
| :-------- | :---- | :---- | :---- | :-------- |
| Default   | 0.045 | 0.211 | 0.146 | 0.563     |
| Tuning    | 0.044 | 0.210 | 0.145 | 0.567     |

### 3.2 Hasil XGBoost Regressor

Model XGBoost menunjukkan peningkatan performa yang sangat signifikan setelah *tuning parameter*. Nilai R-squared melonjak dari 0.489 menjadi 0.598, menandakan bahwa model yang telah dioptimasi jauh lebih baik dalam menjelaskan variasi data dan membuat prediksi yang akurat.

**Tabel Hasil XGBoost**

| Parameter | MAE   | MSE   | RMSE  | R-squared |
| :-------- | :---- | :---- | :---- | :-------- |
| Default   | 0.052 | 0.228 | 0.162 | 0.489     |
| Tuning    | 0.041 | 0.203 | 0.140 | 0.598     |

### 3.3 Perbandingan Model

  * Setelah dilakukan *tuning*, model **XGBoost Regressor menunjukkan performa yang lebih unggul** di semua metrik evaluasi dibandingkan Random Forest Regressor.
  * XGBoost memiliki nilai MAE, MSE, dan RMSE yang lebih rendah, serta nilai R-squared yang lebih tinggi (0.598 vs 0.567).
  * Ini menunjukkan bahwa XGBoost lebih akurat dan lebih efektif dalam menjelaskan variabilitas data pada dataset ini, sehingga menjadi pilihan model yang lebih superior.

-----

## 4\. Kesimpulan

Penelitian ini berhasil membandingkan kinerja dua algoritma *machine learning* untuk prediksi skor kredit. Hasilnya secara konsisten menunjukkan bahwa **XGBoost Regressor**, setelah melalui proses *tuning parameter*, **lebih unggul** daripada Random Forest Regressor. Dengan nilai error yang lebih rendah dan kemampuan menjelaskan variasi data yang lebih tinggi (R-squared=0.598), XGBoost direkomendasikan sebagai model yang lebih optimal untuk studi kasus ini.

### Rencana Pengembangan Selanjutnya

  * Melakukan **Feature Engineering** yang lebih mendalam untuk meningkatkan kualitas fitur.
  * Mengeksplorasi penggunaan model yang lebih kompleks seperti **Neural Networks** untuk menangani hubungan non-linear dalam data.

-----

## 5\. Referensi

1.  Kurniawan, A., Rifa'i, A., Nafis, M. A., Sefrida, N., and Patria, H., "[Pemilihan metode predictive analytics dengan machine learning untuk analisis dan strategi peningkatan kualitas kredit perbankan](https://jurnal.uns.ac.id/ijas/article/view/55483/35729)," *Indonesian Journal of Applied Statistics*, vol. 5, no. 1, pp. 1-10, 2022.
2.  Putra, A. S., and Bakri, A., "[Analisa performa algoritma random forest & logistic regression dalam sistem credit scoring](https://www.google.com/search?q=https://jurnal.unidha.ac.id/index.php/jteksis/article/view/1308/771)," *Jurnal Teknologi dan Sistem Informasi*, vol. 4, no. 2, pp. 45-53, 2021.
3.  Bello, O. A., "[Machine learning algorithms for credit risk assessment: An economic and financial analysis](https://www.researchgate.net/publication/381548370_Citation_Bello_OA_2023_Machine_Learning_Algorithms_for_Credit_Risk_Assessment_An_Economic_and_Financial_Analysis)," *International Journal of Management Technology*, vol. 10, no. 1, pp. 109-133, 2023.
4.  Leuwol, F. S., and Bakri, A., "[Implementation of gradient-boosted tree, support vector machines, and random forest for credit card fraud detection](https://www.google.com/search?q=https://www.semanticscholar.org/paper/Implementation-of-Gradient-Boosted-Tree%252C-Support-to-LeuwolBakri/7c916e542fd22e4e6b39e3ab01d9ac5a5a096805)," *Journal of Information and Data Technology*, vol. 3, no. 2, pp. 123-130, 2023.
5.  Chacko, A., and Aravindhar, J., "[Enhancing credit score analysis: A novel approach](https://www.google.com/search?q=https://www.semanticscholar.org/paper/Enhancing-Credit-Score-Analysis%253A-A-Novel-Approach-ChackoJohnAravindhar/77040ee50d1285c11e33def211cef4384afb1caa)," *International Journal of Financial Studies*, vol. 12, no. 3, pp. 45-60, 2023.