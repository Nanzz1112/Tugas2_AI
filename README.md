# Prediksi Arah Pergerakan Harga Emas Menggunakan Machine Learning

## Anggota Kelompok
- Dhananjaya (2501010023)
- I Gede Anggara Suryantara (2501010013)
- Kadek Angga Putra Artawan (2501010030)

## 1. Latar Belakang

Emas banyak dipilih sebagai instrumen investasi karena nilainya dianggap relatif stabil. Meski begitu, harganya tetap naik dan turun setiap hari karena dipengaruhi kondisi ekonomi dan pasar global. Machine learning dapat digunakan untuk memperkirakan apakah harga emas keesokan hari akan naik atau turun berdasarkan pola data historis.

## 2. Rumusan Masalah dan Tujuan

### Rumusan Masalah
1. Bagaimana memprediksi apakah harga emas keesokan hari akan naik atau turun berdasarkan data harga historis?
2. Model supervised learning apa yang paling cocok untuk masalah ini, dan mengapa?

### Tujuan
Membuat model klasifikasi dua kelas (naik atau turun) dan menjelaskan alasan pemilihan model yang digunakan.

## 3. Dataset

- **Nama:** XAUUSD Gold Price Dataset 1980–2026 with Features
- **Pembuat:** aravaravind
- **Sumber:** [Kaggle - XAUUSD Gold Price Dataset](https://www.kaggle.com/datasets/aravaravind/xauusd-gold-price-dataset-19802026-with-features)
- **Isi data:** Harga emas terhadap dolar AS (XAU/USD), data harian mulai 26 Desember 1979 dengan lebih dari 12.000 baris.
- **Fitur:** `open`, `high`, `low`, `close`, `returns`, `lag 1`, `lag 2`, dan `lag 3`.
- **Target:** `target_direction`, dengan nilai `1` berarti harga naik dan `0` berarti harga turun.

> **Catatan:** Kolom `target_next_close` tidak digunakan sebagai fitur karena berisi harga hari berikutnya yang ingin diprediksi.

## 4. Model yang Digunakan

### Random Forest

Random Forest merupakan algoritma machine learning yang menggabungkan banyak Decision Tree. Setiap pohon menghasilkan prediksi, kemudian kelas dengan suara terbanyak menjadi hasil akhir.

Keunggulan:
- Dapat menangkap hubungan non-linear.
- Relatif tahan terhadap noise dan outlier.
- Tidak memerlukan penskalaan fitur.
- Mengurangi risiko overfitting dibandingkan satu Decision Tree.
- Menyediakan feature importance untuk mengetahui fitur yang berpengaruh.

Parameter utama yang dapat digunakan adalah `n_estimators`, `max_depth`, dan `min_samples_leaf`.

### Logistic Regression

Logistic Regression digunakan sebagai model pembanding. Model ini lebih sederhana dan cepat dilatih, tetapi memiliki keterbatasan dalam menangkap hubungan non-linear.

## 5. Alasan Pemilihan Random Forest

| Model | Alasan |
|---|---|
| Decision Tree | Lebih rentan mengalami overfitting jika hanya menggunakan satu pohon. |
| Support Vector Machine (SVM) | Membutuhkan penskalaan fitur dan penyesuaian parameter. |
| K-Nearest Neighbors (k-NN) | Sensitif terhadap skala fitur dan dapat lambat saat prediksi. |
| Gradient Boosting | Dapat memberikan hasil baik, tetapi lebih sensitif terhadap tuning. |
| Logistic Regression | Digunakan sebagai pembanding karena lebih sederhana dan terbatas pada hubungan linear. |

Random Forest dipilih sebagai model utama karena dapat menangkap hubungan non-linear, relatif stabil terhadap noise, dan dapat menunjukkan fitur yang paling berpengaruh.

## 6. Rencana Pengerjaan dan Evaluasi

1. Memuat dataset dan memeriksa data kosong.
2. Menghapus kolom `target_next_close` dari fitur untuk menghindari kebocoran data (*data leakage*).
3. Membagi data berdasarkan urutan waktu: 80% data lama untuk pelatihan dan 20% data terbaru untuk pengujian.
4. Melatih model Random Forest dan Logistic Regression. Fitur Logistic Regression akan diskalakan.
5. Mengevaluasi model menggunakan accuracy, precision, recall, F1-score, dan confusion matrix.
6. Membandingkan hasil dengan baseline berupa prediksi kelas yang paling sering muncul.
7. Menganalisis feature importance dan menyusun kesimpulan.

Karena prediksi hanya menggunakan fitur dari data harga historis, arah harga emas mungkin sulit diprediksi. Akurasi yang sedikit di atas 50% dapat menjadi salah satu kemungkinan hasil, tetapi hasil sebenarnya harus ditentukan melalui pengujian.

## 7. Jurnal Terkait

1. **The Prediction of Gold Price Movement by Comparing Naive Bayes, Support Vector Machine, and K-NN (JISA)**  
   [Baca jurnal](https://mail.ojs.trilogi.ac.id/index.php/JISA/article/view/922)

2. **Penerapan Metode Random Forest Regression untuk Prediksi Harga Emas di Indonesia (JATI)**  
   [Baca jurnal](https://ejournal.itn.ac.id/jati/article/download/18603/9789/)

3. **Optimasi Prediksi Harga Emas dengan Metode Support Vector Regression (SVR) Menggunakan Algoritma Grid Search**  
   [Baca jurnal](https://ejournal.itn.ac.id/index.php/jati/article/download/8000/4790)

4. **Gold Price Prediction Using Machine Learning Techniques (CEUR Workshop Proceedings)**  
   [Baca jurnal](https://ceur-ws.org/Vol-3283/Paper108.pdf)

## 8. Kesimpulan

Prediksi arah harga emas dirumuskan sebagai masalah klasifikasi dua kelas, yaitu naik dan turun. Random Forest diusulkan sebagai model utama karena relatif tahan terhadap noise, dapat menangkap hubungan non-linear, dan menyediakan informasi fitur yang berpengaruh. Logistic Regression digunakan sebagai pembanding sederhana. Kesimpulan akhir akan ditentukan berdasarkan hasil evaluasi pada data uji.
