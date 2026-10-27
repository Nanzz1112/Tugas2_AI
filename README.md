
# PREDIKSI HARGA EMAS

## Anggota Kelompok

- Dhananjaya (2501010023)
- I Gede Anggara Suryantara (2501010013)
- Kadek Angga Putra Artawan (2501010030)

## 1. Latar Belakang

Emas banyak dipilih sebagai instrumen investasi karena nilainya dianggap relatif stabil. Meski begitu, harganya tetap naik dan turun setiap hari karena dipengaruhi kondisi ekonomi dan pasar global. Bagi orang yang ingin membeli atau menjual emas, kemampuan memperkirakan apakah harga besok akan naik atau turun tentu sangat membantu. Machine learning dapat dipakai untuk hal ini karena model dapat mempelajari pola dari data harga di masa lalu.

## 2. Dataset

Dataset yang digunakan hanya satu, yaitu dataset harga emas harian di Kaggle yang sudah dilengkapi fitur dan label arah harga.

- **Nama dataset:** XAUUSD Gold Price Dataset 1980–2026 with Features
- **Pembuat:** aravaravind
- **Link Kaggle:** https://www.kaggle.com/datasets/aravaravind/xauusd-gold-price-dataset-19802026-with-features
- **Isi data:** Harga emas terhadap dolar AS (XAU/USD), data harian mulai 26 Desember 1979, lebih dari 12.000 baris.
- **Fitur:** `open`, `high`, `low`, `close`, `returns`, `lag 1`, `lag 2`, dan `lag 3`.
- **Label (target):** `target_direction`, yaitu `1` berarti harga naik dan `0` berarti harga turun.

**Catatan:** Kolom `target_next_close` tidak dipakai sebagai fitur karena berisi harga hari berikutnya, yaitu jawaban yang akan diprediksi. Cek kembali daftar kolom di halaman Kaggle sebelum memulai.

## 3. Model yang Dipilih: Random Forest

### Cara Kerja

Random Forest adalah kumpulan banyak decision tree. Setiap pohon dilatih dengan sampel data yang diambil secara acak dan hanya memakai sebagian fitur secara acak. Saat memprediksi, setiap pohon memberi suara (naik atau turun), lalu kelas dengan suara terbanyak menjadi hasil akhir.

### Fitur-Fitur Utama Model Ini

- Dapat menangkap hubungan non-linear antarvariabel tanpa perlu menentukan bentuk hubungannya.
- Lebih tahan terhadap noise dan outlier, serta tidak wajib melakukan penskalaan fitur.
- Menyediakan feature importance, yaitu urutan fitur yang paling berpengaruh pada prediksi.
- Risiko overfitting lebih kecil dibanding satu decision tree karena hasilnya dirata-ratakan dari banyak pohon.
- Parameter utama yang diatur: jumlah pohon (`n_estimators`), kedalaman maksimum (`max_depth`), dan jumlah minimum data di daun (`min_samples_leaf`).

### Kegunaan di Studi Kasus Ini

Random Forest dipakai untuk mengklasifikasikan setiap hari menjadi "naik" atau "turun", sekaligus menunjukkan apakah harga terbaru, return, atau harga beberapa hari sebelumnya yang paling berpengaruh.

### Model Pembanding

Sebagai pembanding hanya dipakai Logistic Regression. Model ini sederhana, menghitung peluang kelas dari kombinasi linear fitur, dan cepat dilatih. Jika Random Forest tidak lebih baik dari Logistic Regression, berarti kerumitan model tidak memberi manfaat.

## 4. Mengapa Random Forest Dipilih

| Model | Alasan tidak dijadikan model utama |
|---|---|
| Decision Tree | Satu pohon mudah menghafal data latih (overfitting); Random Forest memperbaiki kelemahan ini. |
| Support Vector Machine | Perlu penskalaan fitur dan tuning parameter C dan gamma, serta lebih lambat pada data belasan ribu baris. |
| K-Nearest Neighbors (k-NN) | Sangat bergantung pada skala fitur, lambat saat memprediksi, dan mudah terganggu noise pada data harga. |
| Gradient Boosting | Sering sangat akurat, tetapi lebih sensitif terhadap tuning dan noise. Cocok menjadi alternatif jika waktu cukup. |
| Logistic Regression | Hanya mampu hubungan linear, sehingga dipakai sebagai pembanding, bukan model utama. |

### Kesimpulan Pemilihan

Random Forest dipilih karena data harga emas bersifat berisik dan non-linear, sedangkan Random Forest stabil, tidak butuh banyak penyesuaian, dan dapat menjelaskan fitur yang berpengaruh.

Dari semua model yang dibahas di atas, Random Forest dinilai paling sesuai untuk studi kasus ini karena dapat menangkap hubungan non-linear pada data harga yang berisik, tidak perlu penskalaan fitur, hanya butuh sedikit tuning, lebih stabil daripada satu Decision Tree, serta menunjukkan fitur yang berpengaruh lewat feature importance.

Gradient Boosting bisa menyaingi tetapi lebih sensitif terhadap tuning dan noise. SVM dan K-NN perlu penskalaan dan lebih lambat, sedangkan Logistic Regression hanya mampu hubungan linear sehingga cocok sebagai pembanding. Penilaian ini berdasarkan karakter data dan model.

## 5. Fitur yang Digunakan dalam Program

Program menggunakan fitur-fitur berikut untuk melatih model prediksi arah pergerakan harga emas.

| Fitur | Keterangan |
|---|---|
| `open` | Harga pembukaan emas |
| `high` | Harga tertinggi emas |
| `low` | Harga terendah emas |
| `close` | Harga penutupan emas |
| `returns` | Persentase perubahan harga harian |
| `lag 1` | Harga penutupan satu hari sebelumnya |
| `lag 2` | Harga penutupan dua hari sebelumnya |
| `lag 3` | Harga penutupan tiga hari sebelumnya |

Target klasifikasi yang digunakan adalah `target_direction`:

- `1` = Harga emas naik.
- `0` = Harga emas turun.

Fitur digunakan sebagai input untuk mempelajari pola historis, sedangkan target digunakan sebagai jawaban yang dipelajari model. Kolom `target_next_close` tidak dimasukkan ke dalam fitur agar tidak terjadi data leakage.

## 6. Rencana Pengerjaan dan Evaluasi

1. Muat dataset, periksa data kosong, lalu buang kolom `target_next_close` dari fitur.
2. Bagi data berdasarkan urutan waktu, 80% data lama untuk latih dan 20% data terbaru untuk uji. Jangan diacak karena data berupa deret waktu.
3. Latih Random Forest dan Logistic Regression. Fitur diskalakan untuk Logistic Regression.
4. Evaluasi dengan accuracy, precision, recall, F1-score, dan confusion matrix.
5. Bandingkan dengan tebakan sederhana, yaitu selalu memilih kelas yang paling sering muncul.
6. Lihat feature importance dan tulis kesimpulan.

Karena fitur hanya berasal dari harga itu sendiri dan arah harga sulit ditebak, akurasi yang sedikit di atas 50% adalah hasil yang mungkin terjadi. Hal ini bisa dibahas sebagai keterbatasan penelitian.

## 7. Cara Membuka dan Menjalankan Program di Google Colab

Program dapat dijalankan menggunakan Google Colab tanpa perlu menginstal Python di komputer.

### Langkah 1: Buka Google Colab

Buka https://colab.research.google.com/, kemudian pilih **New Notebook** untuk membuat notebook baru.

### Langkah 2: Upload File Program dan Dataset

Siapkan dua file berikut:

- `prediksi_harga_emas.py` — file program Python.
- `gold.csv` — dataset harga emas yang diunduh dari Kaggle.

Buat cell baru dan jalankan kode berikut:

```python
from google.colab import files

data_upload = files.upload()
```

Pilih file `prediksi_harga_emas.py` dan `gold.csv` dari komputer. Pastikan kedua file berhasil diunggah.

### Langkah 3: Instal Library

Jalankan kode berikut pada cell baru:

```python
!pip install pandas matplotlib scikit-learn
```

Tunggu sampai proses instalasi selesai.

### Langkah 4: Jalankan Program

Setelah kedua file berhasil diunggah, jalankan kode berikut:

```python
%run prediksi_harga_emas.py
```

Program akan membaca dataset `gold.csv`, melatih Random Forest dan Logistic Regression, menampilkan hasil evaluasi model, serta menampilkan grafik yang dibuat oleh program.

### Langkah 5: Periksa Hasil

Periksa hasil yang ditampilkan, termasuk:

- Jumlah data yang digunakan.
- Fitur yang berhasil dibaca.
- Hasil evaluasi Random Forest.
- Hasil evaluasi Logistic Regression.
- Perbandingan accuracy kedua model.
- Confusion matrix dan feature importance jika ditampilkan oleh program.

**Catatan:** Jika muncul pesan `FileNotFoundError`, pastikan file `gold.csv` sudah diunggah dan namanya sesuai. Jika sesi Google Colab terputus atau diatur ulang, file mungkin perlu diunggah kembali.

## 8. Jurnal Terkait

1. **The Prediction of Gold Price Movement by Comparing Naive Bayes, Support Vector Machine, and K-NN (JISA).** Membahas prediksi pergerakan harga emas, sehingga paling dekat dengan studi kasus ini.  
   Link: https://mail.ojs.trilogi.ac.id/index.php/JISA/article/view/922

2. **Penerapan Metode Random Forest Regression untuk Prediksi Harga Emas di Indonesia (JATI).** Mendukung penggunaan Random Forest pada data harga emas.  
   Link: https://ejournal.itn.ac.id/jati/article/download/18603/9789/

3. **Optimasi Prediksi Harga Emas dengan Metode Support Vector Regression (SVR) Menggunakan Algoritma Grid Search (JATI, Vol. 7 No. 6, Desember 2023).** Contoh tuning parameter pada prediksi harga emas.  
   Link: https://ejournal.itn.ac.id/index.php/jati/article/download/8000/4790

4. **Gold Price Prediction Using Machine Learning Techniques (CEUR Workshop Proceedings, Vol. 3283).** Memakai data Kaggle dan metode ensemble berbasis pohon.  
   Link: https://ceur-ws.org/Vol-3283/Paper108.pdf

## 9. Kesimpulan

Prediksi arah harga emas dapat dirumuskan sebagai masalah klasifikasi dua kelas. Random Forest diusulkan sebagai model utama karena tangguh terhadap noise, tidak banyak membutuhkan penyesuaian, dan dapat menunjukkan fitur yang paling berpengaruh. Logistic Regression dipakai sebagai pembanding sederhana. Kesimpulan akhir bergantung pada hasil percobaan pada data uji.

---

**Catatan:** Proyek ini bertujuan untuk pembelajaran machine learning dan bukan merupakan saran investasi. Hasil prediksi tidak menjamin pergerakan harga emas yang sebenarnya.
