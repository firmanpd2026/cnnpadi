# Panduan Hosting Streamlit ke GitHub dan Streamlit Community Cloud

Aplikasi Streamlit tidak bisa dijalankan langsung sebagai GitHub Pages. GitHub digunakan untuk menyimpan source code, lalu aplikasi dijalankan melalui **Streamlit Community Cloud**.

## 1. Siapkan akun

Pastikan sudah memiliki:

1. Akun GitHub.
2. Akun Streamlit Community Cloud.
3. Repository GitHub untuk project ini, misalnya: `cnn-padi-streamlit`.

## 2. Upload project ke GitHub

### Cara mudah melalui website GitHub

1. Buka GitHub.
2. Klik **New repository**.
3. Isi nama repository, contoh: `cnn-padi-streamlit`.
4. Pilih **Public** agar mudah dideploy ke Streamlit Cloud.
5. Klik **Create repository**.
6. Upload semua file dan folder dari project ini ke repository.
7. Pastikan file berikut ada di root repository:
   - `app.py`
   - `requirements.txt`
   - `runtime.txt`
   - `config.py`
   - `utils.py`
   - `disease_solutions.py`
   - `.streamlit/config.toml`

### Cara melalui Git Bash / CMD

Jalankan dari folder project:

```bash
git init
git add .
git commit -m "Deploy Streamlit CNN penyakit daun padi"
git branch -M main
git remote add origin https://github.com/USERNAME/cnn-padi-streamlit.git
git push -u origin main
```

Ganti `USERNAME` dengan username GitHub masing-masing.

## 3. Deploy ke Streamlit Community Cloud

1. Buka Streamlit Community Cloud.
2. Login menggunakan GitHub.
3. Klik **New app**.
4. Pilih repository: `cnn-padi-streamlit`.
5. Branch: `main`.
6. Main file path: `app.py`.
7. Klik **Deploy**.

Jika berhasil, Streamlit akan memberikan link aplikasi seperti:

```text
https://nama-aplikasi.streamlit.app
```

## 4. Catatan penting tentang model CNN

File model hasil training biasanya berada di:

```text
models/rice_disease_model.keras
models/class_names.txt
```

Aplikasi tetap bisa dibuka walaupun model belum tersedia, tetapi halaman prediksi belum bisa digunakan sampai model tersedia.

Pilihan yang bisa dilakukan:

### Opsi A — Training lokal lalu upload model ke GitHub

1. Jalankan training di laptop:

```bash
python train_model.py --epochs 5
```

2. Setelah training selesai, upload file hasil training berikut ke folder `models/` di GitHub:

```text
models/rice_disease_model.keras
models/class_names.txt
models/model_info.json
models/training_history.csv
models/classification_report.csv
models/confusion_matrix.csv
models/dataset_summary.csv
```

3. Jika file model terlalu besar, gunakan opsi B.

### Opsi B — Simpan model di cloud storage

Jika file `.keras` terlalu besar untuk GitHub, simpan model di Google Drive, Hugging Face, atau layanan penyimpanan lain, lalu tambahkan fitur download model otomatis pada aplikasi.

## 5. Jika deploy gagal

Cek error pada halaman Streamlit Cloud bagian **Manage app → Logs**.

Masalah umum:

1. **TensorFlow gagal install**  
   Pastikan `runtime.txt` berisi:

```text
python-3.11
```

2. **ModuleNotFoundError**  
   Pastikan library ada di `requirements.txt`.

3. **Model belum dilatih**  
   Upload file model ke folder `models/` atau jalankan training terlebih dahulu secara lokal.

4. **Ukuran file terlalu besar**  
   Jangan upload folder `dataset/` ke GitHub. Dataset sebaiknya tetap di Kaggle atau penyimpanan lain.

## 6. Struktur folder untuk deploy

```text
cnn-padi-streamlit/
├── app.py
├── config.py
├── disease_solutions.py
├── utils.py
├── train_model.py
├── download_dataset.py
├── requirements.txt
├── runtime.txt
├── packages.txt
├── DEPLOYMENT.md
├── README.md
├── .streamlit/
│   ├── config.toml
│   └── secrets.toml.example
├── dataset/
│   ├── .gitkeep
│   └── README.txt
└── models/
    ├── .gitkeep
    └── README.txt
```
