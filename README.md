# 🥭🍎 Proyek Akhir: Image Classification dengan CNN

Kelas: Belajar Fundamental Deep Learning – Dicoding Indonesia

Proyek ini merupakan implementasi lengkap model klasifikasi gambar menggunakan Convolutional Neural Network (CNN).  
Dataset yang digunakan adalah **Fruits360** yang berisi lebih dari 100.000 gambar buah dan sayuran dengan 227 kelas.

Tujuan proyek ini:

1. Melatih model CNN dengan akurasi tinggi.
2. Melakukan evaluasi train/validation/test.
3. Melakukan visualisasi proses training.
4. Menyimpan model dalam format:
   - SavedModel
   - TensorFlow Lite
   - TensorFlow.js
5. Melakukan inference menggunakan SavedModel dengan gambar dari luar dataset.

---

## 📂 Struktur Direktori Submission

📌 Keterangan:

- **saved_model/** → format model TensorFlow SavedModel lengkap beserta variable dan metadata.
- **tflite/** → model versi TensorFlow Lite + file label.txt.
- **tfjs_model/** → model untuk TensorFlow.js (browser atau Node.js).
- **notebook.ipynb** → notebook utama berisi seluruh proses pelatihan & evaluasi.
- **README.md** → dokumentasi proyek (file ini).
- **requirements.txt** → daftar library Python yang dibutuhkan untuk menjalankan notebook

---

## 📦 Dataset

Dataset yang digunakan adalah **Fruits360** (resolusi 100×100 piksel) yang berisi beragam gambar buah dan sayuran.

- Total gambar (Training + Test digabung): ± 118.000+
- Jumlah kelas: **227** kelas buah dan sayuran.

Link dataset (Kaggle):  
https://www.kaggle.com/datasets/moltean/fruits

### 🔀 Pembagian Dataset (Split Manual)

Sesuai ketentuan submission, pembagian dataset **tidak** menggunakan struktur bawaan Fruits360 secara langsung.  
Langkah yang dilakukan:

1. Seluruh gambar dari folder **`Training/`** dan **`Test/`** pada Fruits360 digabung menjadi satu list/tautan file.
2. Dibuat sebuah `DataFrame` yang berisi:
   - `filepath` → path lengkap ke file gambar
   - `label` → nama folder kelas (misalnya: `Apple 10`, `Banana 1`, dll.)
3. Dataset kemudian dibagi ulang menggunakan `train_test_split` dari `scikit-learn` dengan **stratified split** berdasarkan label kelas:
   - **Train set**: 70%
   - **Validation set**: 15%
   - **Test set**: 15%

Dengan cara ini:

- Pembagian dataset dilakukan secara acak,
- Distribusi label tetap seimbang,
- Tidak bergantung lagi pada pembagian default Fruits360,
- Sesuai dengan ketentuan proyek Dicoding.

---

## 🏗 Arsitektur Model

Model dibangun menggunakan **Keras Sequential** dengan arsitektur CNN sebagai berikut:

- **Blok Konvolusi (4 blok):**
  - Conv2D → BatchNormalization → Conv2D → BatchNormalization → MaxPooling2D → Dropout
- **Blok Fully Connected:**
  - Flatten
  - Dense(256)
  - BatchNormalization
  - Dropout
- **Output Layer:**
  - Dense(227, activation = `softmax`)

Konfigurasi training:

- Optimizer: **Adam**
- Loss Function: **Categorical Crossentropy**
- Metrics: **Accuracy**

Callbacks yang digunakan:

- **EarlyStopping** → menghentikan training ketika val_loss tidak membaik.
- **ReduceLROnPlateau** → menurunkan learning rate ketika model stagnan.
- **ModelCheckpoint** → menyimpan model terbaik berdasarkan val_loss.

---

## 📊 Hasil Training

Berikut ringkasan hasil training dan validation (dengan split manual):

**Training & Validation:**

- Training Accuracy : **0.9932**
- Validation Accuracy: **0.9992**
- Training Loss : **0.0207**
- Validation Loss : **0.0031**

Grafik akurasi dan loss menunjukkan:

- Akurasi training dan validation sama-sama tinggi dan stabil.
- Loss training dan validation sama-sama rendah.
- Tidak ditemukan indikasi overfitting yang signifikan karena:
  - Perbedaan antara training dan validation sangat kecil.
  - Validation accuracy dan test accuracy justru sedikit lebih tinggi dari training.

Visualisasi grafik training dan validation (accuracy & loss) disertakan di dalam notebook.

---

## 🧪 Evaluasi Model

Evaluasi akhir dilakukan pada **test set** (15% dari seluruh data) yang tidak pernah dilihat model saat training.

Hasil evaluasi:

- **Test Accuracy:** **0.9996**
- **Test Loss:** **0.0024**
- **Total sampel test:** 23.993 gambar

Selain itu, dilakukan juga:

- **Classification report** untuk seluruh 227 kelas.
- Hasil menunjukkan:
  - `precision`, `recall`, dan `f1-score` rata-rata mendekati **1.00** (baik macro maupun weighted average).
  - Hampir semua kelas memiliki performa **sangat tinggi**, konsisten dengan kualitas dataset Fruits360 yang terstruktur dan bersih.

---

## 🔍 Inference Menggunakan SavedModel

Inference dilakukan menggunakan:

- **Model SavedModel** yang telah dikonversi dari model Keras.
- Gambar yang di-_upload_ dari **luar dataset** (gambar bebas, misalnya foto buah sendiri).

Alur inference:

1. Pengguna meng-upload 1 gambar lewat interface upload di Google Colab.
2. Gambar diproses:
   - Di-resize ke 100×100 piksel.
   - Dinormalisasi (dibagi 255) agar konsisten dengan data training.
3. Gambar dijadikan batch (shape `(1, 100, 100, 3)`).
4. Model SavedModel dipanggil menggunakan `tf.keras.layers.TFSMLayer`.
5. Model menghasilkan probabilitas untuk setiap kelas.
6. Dipilih kelas dengan probabilitas tertinggi sebagai **prediksi akhir**, disertai nilai confidence.
7. Gambar ditampilkan bersama label prediksi & confidence.

Catatan:

- Pada data Fruits360 (test set resmi), performa model hampir sempurna.
- Pada gambar luar dataset, hasil prediksi akan sangat dipengaruhi:
  - kedekatan tampilan gambar dengan gaya dataset (background, pencahayaan, posisi buah),
  - kualitas gambar dan noise visual.

Penjelasan dan contoh kode inference terdapat di bagian akhir notebook.

---

## 📝 Cara Menjalankan Notebook

1. Upload notebook & dataset sesuai path.
2. Jalankan sel import library.
3. Extract dataset Fruits360.
4. Jalankan cell generator (train/val/test).
5. Jalankan cell training.
6. Jalankan cell evaluasi & visualisasi.
7. Jalankan cell konversi model:
   - SavedModel
   - TFLite
   - TFJS
8. Jalankan inference menggunakan gambar luar dataset.

---

## 🧾 Requirements

Daftar library yang digunakan tersedia pada file `requirements.txt`.  
Beberapa library utama yang digunakan:

- TensorFlow
- TensorFlowJS
- NumPy
- Matplotlib
- Scikit-Learn
- Pillow

---

## 🎉 Kesimpulan

Model CNN yang dibangun berhasil:

- Mencapai akurasi yang sangat tinggi pada dataset Fruits360.
- Menghasilkan file model dalam tiga format berbeda.
- Mampu melakukan inference pada gambar test maupun gambar luar dataset.
- Memenuhi seluruh kriteria wajib dan sebagian besar kriteria nilai tambahan dari Dicoding.

Proyek ini memberikan gambaran lengkap tentang alur kerja pengembangan model CNN untuk klasifikasi gambar, mulai dari data loading, training, evaluasi, hingga deployment model.

---

## ✨ Pembuat Proyek

**Nama:** Muhammad Rahman Shiddiq (rahman09)  
**Email:** rahmanshiddiq09@gmail.com  
**Developed with:** TensorFlow & Keras
