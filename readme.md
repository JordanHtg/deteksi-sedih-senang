# 🎭 Deteksi Ekspresi Wajah: Sedih vs Senang

Proyek ini adalah implementasi sistem klasifikasi gambar berbasis Machine Learning untuk mendeteksi ekspresi wajah, khususnya mengenali emosi **Senang (Happy)** dan **Sedih (Sad)**. Sistem ini dibangun dengan fleksibilitas tinggi, menyediakan dua pilihan antarmuka: aplikasi desktop interaktif (OpenCV) dan aplikasi web (Streamlit).

---

## 🚀 Fitur Utama

- **Multi-Platform Interface**: 
  - **Desktop App (`app_regular.py`)**: Aplikasi ringan berbasis OpenCV untuk inferensi langsung di mesin lokal.
  - **Web Dashboard (`app_streamlit.py`)**: Antarmuka web interaktif berbasis Streamlit yang modern dan mudah diakses.
- **Tiga Mode Input Fleksibel**:
  - 📷 **Webcam Realtime**: Deteksi langsung dari kamera dengan informasi FPS dan bar probabilitas real-time.
  - 🎥 **Video File**: Pemrosesan video dari file lokal secara frame-by-frame dengan kontrol pause/resume.
  - 🖼️ **Gambar/Foto**: Analisis cepat untuk satu atau beberapa gambar statis (mendukung multiple uploads di Streamlit).
- **Visualisasi Prediksi yang Informatif**: Menampilkan overlay interaktif berupa teks prediksi utama, persentase kepercayaan (confidence rate), serta bar chart probabilitas untuk semua kelas target.
- **Fallback Mechanism**: Penanganan error cerdas pada model Deep Learning. Jika terjadi perbedaan versi TensorFlow/Keras antara environment training dan deployment, aplikasi secara otomatis mencoba melakukan rebuild arsitektur dan meload weights.
- **Snapshot Otomatis**: Kemampuan untuk menyimpan frame hasil prediksi sebagai screenshot secara langsung (dengan menekan tombol `S` pada desktop).

---

## 📁 Struktur Direktori & File

Berdasarkan analisis arsitektur proyek, berikut adalah komponen utamanya:

```text
deteksi-sedih-senang/
│
├── template_ml_image.ipynb   # Notebook Google Colab untuk preprocessing dataset dan training model.
├── app_regular.py            # Script utama untuk menjalankan aplikasi desktop OpenCV.
├── app_streamlit.py          # Script implementasi aplikasi web berbasis Streamlit.
│
└── ml_output/                # Direktori hasil training (Model & Visualisasi Metrik)
    ├── best_model.keras      # Checkpoint model Deep Learning terbaik selama proses training.
    ├── model_MobileNetV2.keras # Model utama yang dirender untuk inferensi.
    ├── model_MobileNetV2.h5  # Format model cadangan (sebagai fallback kompatibilitas).
    ├── metadata.json         # Metadata konfigurasi (algoritma, akurasi, resolusi input, nama kelas).
    ├── class_names.json      # Mapping indeks ke nama label (happy, sad).
    ├── confusion_matrix.png  # Visualisasi performa matriks klasifikasi model.
    ├── distribusi_kelas.png  # Grafik jumlah sampel per kelas pada dataset.
    ├── sample_gambar.png     # Gambar preview/sample dari dataset yang digunakan.
    └── training_history.png  # Grafik pergerakan grafik Loss dan Akurasi dari setiap epoch.
```

---

## 📊 Hasil Analisis Model

Berdasarkan ekstraksi `metadata.json` pada folder `ml_output/`, model yang dihasilkan memiliki karakteristik dan performa sebagai berikut:

- **Algoritma Ekstraksi/Klasifikasi**: `MobileNetV2` (Arsitektur Deep Learning)
- **Ukuran Input Gambar**: `128 x 128` pixels (RGB)
- **Jumlah Kelas Target**: 2 Kelas
- **Daftar Kelas**: `[ "happy", "sad" ]`
- **Tingkat Akurasi (Accuracy)**: **69.36%**

*Catatan Analisis: Dengan ukuran citra 128x128 dan penggunaan MobileNetV2, model sudah cukup ringan untuk Edge Deployment (webcam realtime). Akurasi sebesar 69.36% mengindikasikan model telah membedakan pola dasar emosi, namun sangat direkomendasikan untuk melakukan penambahan dataset wajah dengan berbagai kondisi pencahayaan dan variasi orang untuk meningkatkan performa model (idealnya >85%).*

---

## 💻 Panduan Penggunaan

### 1. Menjalankan Aplikasi Desktop (OpenCV)
Pastikan dependensi dasar telah terinstal: `opencv-python`, `tensorflow`, `scikit-learn`, `scikit-image`, dan `pillow`.

```bash
# Menjalankan menu utama yang interaktif
python app_regular.py

# Atau, melewati menu dan menjalankan mode spesifik secara langsung
python app_regular.py --mode webcam
python app_regular.py --mode video --source path/to/video.mp4
python app_regular.py --mode image --source path/to/image.jpg
```

**Kontrol Keyboard Khusus Mode Desktop:**
- `Q` / `ESC` : Keluar dari program
- `S` : Menyimpan tangkapan layar beserta overlay hasil prediksi
- `P` : Pause/Resume pemrosesan video

### 2. Menjalankan Aplikasi Web (Streamlit)
Mode web sangat praktis apabila Anda tidak ingin menggunakan command prompt sebagai antarmuka atau ingin menggunakan fitur realtime webcam berbasis browser (`streamlit-webrtc`).

```bash
# Instalasi modul khusus Streamlit
pip install streamlit streamlit-webrtc av

# Menjalankan server aplikasi web
streamlit run app_streamlit.py
```
Setelah server berjalan, akses URL `http://localhost:8501` pada web browser Anda. Anda dapat mengunggah file foto atau video untuk dianalisis oleh antarmuka sistem.
