# 📝 **README.md — YouTube Comment Sentiment Analysis (AI Anxiety Topic)**

## 📌 **1. Deskripsi Proyek**

Proyek ini membangun sistem **analisis sentimen**, **deteksi toxic**, dan **deteksi spam** menggunakan data komentar YouTube pada video berjudul:

> **“Hati-Hati Pengangguran Karena AI”**

Dataset diperoleh melalui proses **web crawling**, menghasilkan **1055 komentar asli** dari pengguna YouTube.
Proyek ini bertujuan untuk:

* Mengukur persepsi publik terhadap isu AI dan pengangguran
* Mengidentifikasi komentar bernada **positif**, **negatif**, atau **netral**
* Mendeteksi komentar **toxic** dan **spam**
* Membangun **model machine learning** untuk klasifikasi sentimen, toxic, dan spam
* Menyimpan seluruh model untuk dapat digunakan ulang

Proyek ini menggunakan model baseline **Logistic Regression** dengan **TF-IDF Vectorizer**, dan juga dilakukan eksperimen **IndoBERT Fine-Tuning**, namun mengalami beberapa kendala teknis.

---

## 📂 **2. Dataset**

* Total komentar: **1055**
* Sumber: YouTube
* Topik: *AI, pekerjaan, pengangguran, teknologi*
* Setelah pembersihan (cleaning + deduplication) → digunakan untuk model sentiment, toxic, dan spam.

---

## 📊 **3. Hasil Analisis Statistik Dataset**
## 📉 Distribusi Sentimen Komentar

<img src="Sentimen Yt/Gambar/Gambar 2.png" width="600"/>

### **Distribusi Sentimen**

```
neutral     871
negative     95
positive     89
```

Mayoritas komentar bersifat **netral**, sedangkan komentar negatif sedikit lebih dominan daripada positif.

---

### **Distribusi Toxic**

```
non_toxic    1033
toxic          22
```

Komentar toxic relatif sedikit, hanya sekitar **2%** dari total komentar.

---

### **Distribusi Spam**

```
non_spam    1012
spam          43
```

Komentar spam sekitar **4%**, banyak berupa promosi, link, atau pesan otomatis.

---

### **Komentar yang Mengandung Kata "AI"**

Jumlah komentar mengandung "AI": **560 komentar**

Distribusi sentimen khusus pada komentar tersebut:

```
neutral     458
negative     69
positive     33
```

Sebagian besar komentar tentang AI masih **netral**, namun **komentar negatif lebih banyak daripada positif**, menunjukkan adanya kekhawatiran masyarakat terhadap isu AI & pengangguran.

---

## ⚙️ **4. Model Machine Learning**

### **Model Utama (Baseline): Logistic Regression + TF-IDF**

Model dilatih menggunakan:

* **TF-IDF Vectorizer** (uni-gram dan bi-gram)
* **Logistic Regression**
* **SMOTE** untuk menyeimbangkan label (positive, negative, neutral)

---

## 📈 **5. Evaluasi Model Sentimen (Logistic Regression)**
## 📈 Klasifikasi Model

<img src="Sentimen Yt/Gambar/gambar1.png" width="600"/>

### **Classification Report**

```
              precision    recall  f1-score   support

negative      0.95         0.96      0.96       172
neutral       0.86         0.94      0.90       172
positive      0.98         0.89      0.93       172

accuracy                               0.93       516
macro avg     0.93         0.93      0.93
weighted avg  0.93         0.93      0.93
```

### **Confusion Matrix**

```
[[165   7   0]
 [  8 161   3]
 [  0  19 153]]
```

**Kesimpulan Evaluasi:**

* Model mencapai **akurasi 93%**, sangat baik untuk baseline.
* Model sangat bagus mengenali:

  * komentar **negatif yang jelas**
  * komentar **positif yang eksplisit**
* Kesalahan paling umum terjadi antara **neutral ↔ positive**, sesuai ekspektasi karena banyak komentar ambigu.

---

## 🤖 **6. Eksperimen IndoBERT Fine-Tuning**

Proyek ini juga mencoba melakukan fine-tuning dengan model:

* `indolem/indobert-base-uncased`

Namun proses fine-tuning mengalami kendala:

### **Kendala IndoBERT**

1. **TrainingArguments tidak kompatibel**

   * Environment ternyata menggunakan class `TrainingArguments` versi lama
   * Tidak mendukung parameter modern seperti:
     `evaluation_strategy`, `save_strategy`, `logging_strategy`

2. **Logging tidak muncul**

   * Beberapa versi `Trainer` tidak menyimpan log otomatis
   * Membuat grafik training menjadi tidak lengkap

3. **WandB muncul otomatis**

   * Beberapa environment default memaksa integrasi WandB
   * Harus dinonaktifkan manual

4. **Checkpoint dan trainer_state.json tidak konsisten**

   * Beberapa file tidak tersimpan sesuai ekspektasi
   * Bagian `log_history` kosong

5. **Batch tokenization lambat**

   * Komentar YouTube banyak yang panjang dan noise → training lambat
   * Membutuhkan GPU dengan VRAM lebih besar dan runtime stabil

Karena kendala tersebut, IndoBERT fine-tuning **belum berhasil dilakukan sampai evaluasi final**.

---

## 💾 **7. Penyimpanan Model**

Semua model disimpan dalam folder `models/`:

```
Model sentimen disimpan di: models/sentiment_logistic_regression_model.joblib
TF-IDF Vectorizer disimpan di: models/tfidf_vectorizer_sentiment.joblib
Model toxic/non-toxic disimpan di: models/toxic_pipeline_model.joblib
Model spam/non-spam disimpan di: models/spam_pipeline_model.joblib
Semua model dan vectorizer berhasil disimpan. ✅
```

Model bisa dipanggil kembali untuk prediksi real-time.

---

## 🚀 **8. Kesimpulan**

Proyek ini berhasil:

* Melakukan crawling + preprocessing komentar YouTube
* Membuat model **sentiment analysis**, **toxic detection**, dan **spam detection**
* Mendapatkan baseline performance sangat baik (93%) dengan Logistic Regression
* Menyimpan seluruh model untuk penggunaan ulang

Eksperimen IndoBERT gagal karena kendala teknis environment, namun pipeline sudah siap untuk dicoba kembali pada environment yang lebih stabil.
