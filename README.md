# 🥭🍎 Proyek Akhir: Image Classification dengan CNN  
Kelas: Belajar Machine Learning untuk Pemula – Dicoding Indonesia  

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

├── saved_model/
│ ├── saved_model.pb
│ ├── fingerprint.pb
│ ├── assets/
│ └── variables/
├── tflite/
│ ├── model.tflite
│ └── label.txt
├── tfjs_model/
│ ├── model.json
│ └── group1-shard1of1.bin
├── notebook.ipynb
├── README.md
└── requirements.txt


---

## 📦 Dataset

Dataset yang digunakan adalah **Fruits360** (100x100 resolution).

Link Dataset (Kaggle):  
https://www.kaggle.com/datasets/moltean/fruits

Jumlah data:
- Total gambar: ± 95.000 (train) + 23.000 (test)
- Total kelas: 227 kelas buah & sayuran

Dataset dibagi menjadi:
- **Train set** (80%)
- **Validation set** (20% dari train)
- **Test set** (Fruits360 original folder)

---

## 🏗 Arsitektur Model

Model dibuat menggunakan **Keras Sequential** dan terdiri dari:

- 4 blok konvolusi:
  - Conv2D → BatchNorm → Conv2D → BatchNorm → MaxPooling → Dropout  
- Dense Layer:
  - Flatten → Dense(256) → BatchNorm → Dropout  
- Output Layer:
  - Dense(num_classes, activation='softmax')

Optimizer: **Adam**  
Loss Function: **Categorical Crossentropy**  
Callbacks yang digunakan:
- EarlyStopping
- ReduceLROnPlateau
- ModelCheckpoint

---

## 📊 Hasil Training

### Akurasi:
- **Training Accuracy:** 98.8%  
- **Validation Accuracy:** 97.1%  
- **Test Accuracy:** 98.4%

### Loss:
- Training loss rendah dan stabil.
- Validation loss juga mendekati training loss → model tidak overfitting.

Visualisasi grafik disertakan di notebook.

---

## 🧪 Evaluasi Model

Evaluasi dilakukan pada:
- Validation set
- Test set
- Classification report lengkap per kelas

Model menunjukkan performa stabil dan konsisten dengan rata-rata f1-score sangat tinggi.

---

## 🔍 Inference menggunakan SavedModel

Inferensi dilakukan menggunakan:
- Model SavedModel
- Gambar yang di-*upload* dari luar dataset (gambar bebas)

Flow inference:
1. Upload gambar lewat file upload Colab
2. Preprocess (resize ke 100x100, normalize)
3. Model memprediksi kelas & confidence
4. Gambar ditampilkan beserta hasil prediksi

Catatan:
- Model sangat akurat pada data Fruits360.
- Pada gambar luar dataset, hasil prediksi bergantung pada kesamaan visual dengan gaya data Fruits360 (background plain, pencahayaan stabil).

Penjelasan lengkap ada di notebook.

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

Tersedia di file `requirements.txt`, berisi library utama seperti:

- TensorFlow
- TensorFlowJS
- Matplotlib
- NumPy
- Scikit-Learn

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
**Nama:** Muhammad Rahman Shiddiq (rahmanshiddiq09@gmail.com)  
**Developed with:** TensorFlow & Keras  
