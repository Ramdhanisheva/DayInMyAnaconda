# Analisis Data Eksploratif: Faktor-Faktor Penentu Kepuasan Penumpang Maskapai Penerbangan *(Airline Passenger Satisfaction)*

Proyek Tugas Besar Mata Kuliah **Statistika & Probabilitas (STATPROB)**  
**Nama Kelompok:** Day in My Anaconda  
**Dataset Analisis:** `data/train.csv` (103.904 baris observasi, 24 variabel analitis)  

---

## Biodata Anggota Kelompok & Pembagian Tugas

Untuk memastikan analisis dilakukan secara komprehensif, terstruktur, dan objektif, pengerjaan tugas besar ini dibagi ke dalam empat tugas modul mandiri yang saling berkesinambungan:

1. **Dyah Indana Zulfa** (NRP: **5027261079**) — **Tugas 1: Pemahaman Data, Audit Kualitas Data & Penanganan Missing Value**
   - **Berkas Notebook:** [`bagi-tugas/tugas1(dyah).ipynb`](bagi-tugas/tugas1(dyah).ipynb)
   - **Lingkup Kerja:**
     - Pemuatan data mentah `train.csv` dan pembersihan kolom indeks teknis (`Unnamed: 0`).
     - Verifikasi integritas data (pemeriksaan duplikasi baris observasi dan ID unik).
     - Penyusunan kamus data dan klasifikasi taksonomi skala pengukuran seluruh 24 variabel.
     - Analisis sebaran nilai hilang serta pengujian mekanisme data hilang (*Missing Completely at Random* / MCAR vs. *Missing at Random* / MAR) pada fitur `Arrival Delay in Minutes`.
     - Komparasi empiris performa imputasi median vs. imputasi `Departure Delay in Minutes` berbasis *Mean Absolute Error* (MAE).
     - Investigasi mendalam nilai rating 0 sebagai sinyal non-penggunaan fasilitas (*behavioral signal*).
     - Evaluasi distorsi rumus baku Tukey ($Q_3 + 1,5 \times \text{IQR}$) pada variabel keterlambatan yang bersifat *zero-inflated*.

2. **I Ketut Andika Dharma Diputra** (NRP: **5027261010**) — **Tugas 2: Analisis Univariat & Profil Karakteristik Penumpang**
   - **Berkas Notebook:** [`bagi-tugas/tugas2(andika).ipynb`](bagi-tugas/tugas2(andika).ipynb)
   - **Lingkup Kerja:**
     - Evaluasi keseimbangan proporsi variabel target kepuasan (`satisfaction`) untuk menentukan nilai *baseline* tebak mayoritas (*majority class*).
     - Analisis sebaran karakteristik demografi penumpang (distribusi gender, sebaran usia, dan proporsi loyalitas pelanggan).
     - Analisis karakteristik penerbangan (tujuan perjalanan dan proporsi kelas kabin penumpang).
     - Pemetaan skor rata-rata pada 14 aspek fasilitas layanan maskapai (skala 1–5 di luar rating 0) yang dikelompokkan ke dalam tiga pilar operasional: *Layanan Digital & Bandara*, *Kenyamanan Kabin*, dan *Pelayanan Awak Pesawat & Bagasi*.

3. **Sheva Ramdhani** (NRP: **50271016**) — **Tugas 3: Analisis Bivariat, Pola Non-Linear, & Uji Confounding**
   - **Berkas Notebook:** [`bagi-tugas/tugas3(sheva).ipynb`](bagi-tugas/tugas3(sheva).ipynb)
   - **Lingkup Kerja:**
     - Pengujian ukuran efek asosiasi fitur-fitur kategorikal terhadap tingkat kepuasan menggunakan koefisien Cramér's V.
     - **Grafik Kunci 1:** Pembuktian empiris kurva non-linear berbentuk "U" pada rating layanan digital dan evaluasi disparitas korelasi linear (Pearson) vs. korelasi monotonik (Spearman).
     - Analisis pengaruh kelompok usia terhadap kepuasan (kurva U terbalik) dan identifikasi titik jenuh penurunan kepuasan akibat keterlambatan penerbangan (> 15 menit).
     - **Grafik Kunci 2:** Pembongkaran korelasi semu variabel jarak tempuh penerbangan (*Flight Distance*) melalui stratifikasi kelas kabin (*Simpson's Paradox*).
     - Analisis efek interaksi multi-variabel: interaksi loyalitas pelanggan $\times$ tujuan perjalanan (krisis segmen *disloyal business*) dan interaksi usia $\times$ kelas kabin.

4. **Muh. Hisyam Wardhana** (NRP: **5027261076**) — **Tugas 4: Diagnostik Outlier Lanjutan, Preprocessing Pipeline, Pemodelan ML Baseline & Rekomendasi Bisnis**
   - **Berkas Notebook:** [`bagi-tugas/tugas4(hisyam).ipynb`](bagi-tugas/tugas4(hisyam).ipynb)
   - **Lingkup Kerja:**
     - Diagnostik pencilan (*outliers*) lanjutan dan peredaman kemencengan (*skewness*) ekstrem variabel keterlambatan menggunakan transformasi logaritma natural `log1p` serta *binning* toleransi `delayed_15`.
     - Validasi empiris temuan EDA melalui pemodelan *machine learning* internal dengan skema partisi 80:20 *train-validation* (pembandingan *Naive Majority*, *Logistic Regression* linear, dan *HistGradientBoosting* non-linear).
     - Evaluasi metrik performa komparatif: Kurva ROC (*Receiver Operating Characteristic*), *Confusion Matrix*, dan analisis *Permutation Feature Importance*.
     - Perancangan dan pengujian fungsi *preprocessing* modular `prepare(df)` yang bersih dan bebas dari kebocoran data (*data leakage*).
     - Perumusan batasan metodologis analisis (*limitations*) serta perumusan 4 pilar rekomendasi strategis bagi pihak manajemen maskapai penerbangan.

> **Dua Varian Laporan Terpadu:**
> 1. [**`eda_DayInMyAnaconda.ipynb`**](eda_DayInMyAnaconda.ipynb) — **Laporan Investigasi Komprehensif & Mendalam** (Bab I s.d. Bab IV: Audit Data, Analisis Univariat/Bivariat, Uji Confounding Simpson's Paradox, Preprocessing Pipeline, dan Validasi Model ML Baseline).
> 2. [**`eda2_DayInMyAnaconda.ipynb`**](eda2_DayInMyAnaconda.ipynb) — **Notebook Panduan Praktis Step-by-Step (Langkah 1 s.d. 15)** (Mengadopsi format pedagogis standar perkuliahan: Kamus Data, Inspeksi Tipe Data, Missing Value MCAR/MAR, Univariat/Bivariat, Deteksi Outlier IQR/Z-score, Ringkasan EDA, Preprocessing Bersih, Checklist, dan 6 Diskusi Latihan Mahasiswa).

---

## Kamus Data & Taksonomi Variabel Lengkap

Berikut adalah klasifikasi skala pengukuran dan deskripsi analitis untuk seluruh 24 variabel dalam berkas `data/train.csv`:

| No | Nama Kolom | Tipe Data Python | Taksonomi Skala Pengukuran | Rentang / Nilai Unik | Catatan & Peran Analitis |
|---|---|---|---|---|---|
| 1 | `id` | `int64` | Identifier (ID Unik) | 1 s.d. 129.880 | Kunci primer observasi; tidak digunakan sebagai prediktor analitis. |
| 2 | `satisfaction` | `object` | Target Biner Nominal | `neutral or dissatisfied`, `satisfied` | Variabel target utama kepuasan penumpang (43,3% puas vs. 56,7% netral/tidak puas). |
| 3 | `Gender` | `object` | Kategorikal Nominal Biner | `Female`, `Male` | Jenis kelamin penumpang (50,7% wanita vs 49,3% pria; tidak berpengaruh terhadap kepuasan). |
| 4 | `Customer Type` | `object` | Kategorikal Nominal Biner | `Loyal Customer`, `disloyal Customer` | Tipe loyalitas pelanggan (81,7% loyal vs 18,3% disloyal). |
| 5 | `Type of Travel` | `object` | Kategorikal Nominal Biner | `Business travel`, `Personal Travel` | Tujuan perjalanan penerbangan (69,0% perjalanan bisnis vs 31,0% pribadi). |
| 6 | `Class` | `object` | Kategorikal Ordinal Bertingkat | `Eco` < `Eco Plus` < `Business` | Kelas kabin penerbangan dengan tingkatan layanan yang terurut secara hierarkis. |
| 7 | `Age` | `int64` | Numerik Diskrit / Rasio | 7 s.d. 85 tahun | Usia penumpang; memiliki pola kepuasan berbentuk kurva U terbalik (puncak pada 40–50 tahun). |
| 8 | `Flight Distance` | `int64` | Numerik Kontinu / Rasio | 31 s.d. 4.983 mil/km | Jarak tempuh penerbangan; terbukti sebagai korelasi semu (*proxy* dari Business Class). |
| 9 | `Inflight wifi service` | `int64` | Skala Likert / Ordinal | 0 s.d. 5 | Kualitas layanan wifi; rating 0 mengindikasikan fasilitas tidak digunakan (kepuasan 99,7%). |
| 10 | `Departure/Arrival time convenient` | `int64` | Skala Likert / Ordinal | 0 s.d. 5 | Kesesuaian jadwal keberangkatan dan kedatangan pesawat. |
| 11 | `Ease of Online booking` | `int64` | Skala Likert / Ordinal | 0 s.d. 5 | Kemudahan proses reservasi dan pembelian tiket daring. |
| 12 | `Gate location` | `int64` | Skala Likert / Ordinal | 0 s.d. 5 | Lokasi dan kemudahan akses gerbang keberangkatan di bandara. |
| 13 | `Food and drink` | `int64` | Skala Likert / Ordinal | 0 s.d. 5 | Kualitas makanan dan minuman yang disajikan selama penerbangan. |
| 14 | `Online boarding` | `int64` | Skala Likert / Ordinal | 0 s.d. 5 | Kemudahan dan kelancaran proses *check-in/boarding* daring (fitur prediktor terkuat). |
| 15 | `Seat comfort` | `int64` | Skala Likert / Ordinal | 0 s.d. 5 | Kenyamanan kursi penumpang di dalam kabin pesawat. |
| 16 | `Inflight entertainment` | `int64` | Skala Likert / Ordinal | 0 s.d. 5 | Variasi dan kualitas media hiburan selama penerbangan. |
| 17 | `On-board service` | `int64` | Skala Likert / Ordinal | 0 s.d. 5 | Kualitas pelayanan awak pesawat selama berada di kabin. |
| 18 | `Leg room service` | `int64` | Skala Likert / Ordinal | 0 s.d. 5 | Kelegaan ruang kaki pada kursi penerbangan. |
| 19 | `Baggage handling` | `int64` | Skala Likert / Ordinal | 1 s.d. 5 | Kualitas penanganan bagasi penumpang (tidak terdapat nilai 0). |
| 20 | `Checkin service` | `int64` | Skala Likert / Ordinal | 0 s.d. 5 | Kecepatan dan keramahan pelayanan meja *check-in* bandara. |
| 21 | `Inflight service` | `int64` | Skala Likert / Ordinal | 0 s.d. 5 | Kualitas pelayanan umum staf maskapai selama penerbangan. |
| 22 | `Cleanliness` | `int64` | Skala Likert / Ordinal | 0 s.d. 5 | Kebersihan kabin, kursi, dan toilet pesawat. |
| 23 | `Departure Delay in Minutes` | `int64` | Numerik Kontinu / Rasio | 0 s.d. 1.592 menit | Durasi keterlambatan saat keberangkatan (*zero-inflated*: 56,5% bernilai 0). |
| 24 | `Arrival Delay in Minutes` | `float64` | Numerik Kontinu / Rasio | 0 s.d. 1.584 menit | Durasi keterlambatan saat kedatangan (*zero-inflated*, memiliki 310 data hilang MAR). |

---

## Ringkasan Temuan Utama Investigasi Data

1. **Hubungan Non-Linear & Kurva Ekspektasi Layanan (Grafik Kunci 1):**  
   Pengaruh fasilitas layanan digital (*Inflight wifi service* dan *Online boarding*) tidak linear. Kepuasan penumpang pada rating 1, 2, dan 3 berada pada tingkat yang serupa (~12% hingga 25%), kemudian melonjak drastis pada rating 4 (~60%) dan rating 5 (87% hingga 99%). Hal ini menunjukkan adanya ambang batas ekspektasi: peningkatan layanan dari sangat buruk ke biasa tidak menaikkan kepuasan secara berarti sebelum mencapai standar yang sangat baik.

2. **Rating 0 Bukan Data Hilang (*Missing Value*):**  
   Penumpang yang memberikan rating 0 pada layanan *Inflight wifi service* memiliki kepuasan mencapai **99,7%**. Nilai 0 merupakan sinyal perilaku bahwa penumpang tidak memanfaatkan fasilitas tersebut. Mengganti nilai 0 dengan nilai median atau rata-rata adalah kesalahan metodologis yang merusak sinyal analitis.

3. **Pembongkaran Korelasi Semu Jarak Penerbangan (Grafik Kunci 2 / *Simpson's Paradox*):**  
   Secara keseluruhan, jarak penerbangan tampak berkorelasi positif (+0,30) terhadap kepuasan. Namun ketika diuji per kelas kabin, kepuasan penumpang pada kelas ekonomi (*Eco* dan *Eco Plus*) justru menurun seiring bertambahnya jarak akibat faktor kelelahan fisik. Korelasi positif pada data agregat semata-mata terjadi karena penerbangan jarak jauh didominasi oleh penumpang kelas bisnis yang puas dengan fasilitas kabin mewahnya.

4. **Hierarki Faktor Penentu Kepuasan:**  
   Berdasarkan pengujian ukuran efek Cramér's V, kelas kabin (*Class*, $V = 0,505$) dan tujuan perjalanan (*Type of Travel*, $V = 0,449$) memiliki asosiasi terkuat terhadap kepuasan. Sebaliknya, variabel demografi jenis kelamin (*Gender*) sama sekali tidak memiliki pengaruh ($V = 0,012$).

5. **Titik Jenuh Penurunan Kepuasan Akibat Keterlambatan:**  
   Dampak penurunan kepuasan penumpang paling sensitif terjadi pada rentang keterlambatan 1 hingga 15 menit pertama. Melewati batas 15 menit, kepuasan penumpang sudah jatuh dan mendatar pada kisaran ~35%, bahkan hingga keterlambatan mencapai lebih dari 3 jam.

6. **Mekanisme Data Hilang Keterlambatan Kedatangan (MAR):**  
   Sebanyak 310 nilai hilang pada `Arrival Delay in Minutes` (0,30%) terbukti berkorelasi kuat dengan `Departure Delay in Minutes`. Mengisi nilai hilang menggunakan data waktu keterlambatan keberangkatan menghasilkan *Mean Absolute Error* (MAE) sebesar **4,97 menit**, jauh lebih presisi dibandingkan metode standar imputasi median (**15,18 menit**).

7. **Validasi Empiris Model Prediksi Baseline:**  
   Model pohon non-linear (*HistGradientBoosting*) mencapai akurasi **96,4%** dan skor ROC-AUC **0,995**, mengungguli model linear (*Logistic Regression*, akurasi 90,3% dan ROC-AUC 0,966) serta baseline tebak mayoritas (56,1%). Hasil ini mengonfirmasi secara empiris bahwa interaksi variabel dan hubungan non-linear yang teridentifikasi pada tahap EDA benar-benar esensial.

---

## Struktur Direktori Repository

```
proyek-eda-DayInMyAnaconda/
├── data/
│   ├── train.csv                      # Dataset utama analisis (103.904 baris, 24 fitur)
│   └── test.csv                       # Dataset uji terpisah
├── eda_DayInMyAnaconda.ipynb          # Master Notebook 1: Laporan Investigasi Komprehensif (Bab I s.d. IV)
├── eda2_DayInMyAnaconda.ipynb         # Master Notebook 2: Panduan Praktis 15 Langkah Step-by-Step
├── bagi-tugas/                        # Lembar kerja pembagian tugas mandiri per anggota
│   ├── tugas1(dyah).ipynb             # Tugas 1: Pemahaman Data, Audit Kualitas & Missing Value (Dyah)
│   ├── tugas2(andika).ipynb           # Tugas 2: Analisis Univariat & Profil Karakteristik (Andika)
│   ├── tugas3(sheva).ipynb            # Tugas 3: Analisis Bivariat, Pola Non-Linear & Confounding (Sheva)
│   └── tugas4(hisyam).ipynb           # Tugas 4: Diagnostik Outlier, ML Baseline & Rekomendasi (Hisyam)
├── .gitignore                         # Konfigurasi pengabaian file sementara
└── README.md                          # Dokumentasi lengkap proyek tugas besar
```

---

## Rekomendasi Strategis untuk Manajemen Maskapai

1. **Prioritas Alokasi Anggaran pada Infrastruktur Digital:**  
   Layanan digital (*Inflight wifi service* rata-rata 2,73 dan *Ease of Online booking* rata-rata 2,76) merupakan titik terlemah maskapai. Karena fitur ini memiliki pengaruh tertinggi (*permutation importance*) dalam menentukan kepuasan, modernisasi infrastruktur wifi di pesawat dan pembaruan aplikasi reservasi harus menjadi prioritas belanja modal utama.
2. **Program Retensi Khusus Segmen Bisnis Non-Loyal:**  
   Penumpang non-loyal yang melakukan perjalanan bisnis merupakan kelompok dengan kepuasan terendah (hanya 23,7% puas, berbanding 70,6% pada penumpang bisnis loyal). Maskapai perlu menawarkan kemudahan perubahan jadwal tiket dinamis, jalur prioritas *check-in*, dan insentif korporat agar segmen ini tidak berpindah ke maskapai kompetitor.
3. **Peningkatan Kenyamanan Kabin Ekonomi Rute Jarak Jauh:**  
   Jarak penerbangan yang semakin jauh pada kelas ekonomi menurunkan tingkat kepuasan. Pada rute penerbangan menengah hingga jauh, maskapai perlu menyediakan fasilitas kenyamanan tambahan (seperti penyesuaian ruang kaki atau penyediaan konsumsi berkala) guna menekan keletihan fisik penumpang.
4. **Standardisasi Respons Operasional Keterlambatan 15 Menit Pertama:**  
   Mengingat penurunan kepuasan paling drastis terjadi pada 15 menit pertama penundaan jadwal, maskapai harus mengoptimalkan prosedur komunikasi cepat dan pemberian kompensasi transparan sebelum durasi keterlambatan melewati ambang batas kritis 15 menit.

---

## Panduan Menjalankan Berkas Notebook

1. Pastikan lingkungan Python 3.9+ telah terpasang dengan dependensi analitis berikut:
   ```bash
   pip install pandas numpy matplotlib seaborn scipy scikit-learn jupyter
   ```
2. Untuk membuka laporan terpadu:
   ```bash
   # Varian 1: Laporan Investigasi Komprehensif & Pemodelan Machine Learning
   jupyter lab eda_DayInMyAnaconda.ipynb

   # Varian 2: Laporan Step-by-Step 15 Langkah Pedagogis Sederhana
   jupyter lab eda2_DayInMyAnaconda.ipynb
   ```
3. Untuk membuka lembar kerja mandiri per anggota kelompok:
   ```bash
   jupyter lab bagi-tugas/tugas1(dyah).ipynb
   jupyter lab bagi-tugas/tugas2(andika).ipynb
   jupyter lab bagi-tugas/tugas3(sheva).ipynb
   jupyter lab bagi-tugas/tugas4(hisyam).ipynb
   ```
   *(Seluruh sel kode di dalam setiap berkas notebook telah dieksekusi lengkap dengan output tabel data dan grafik beresolusi tinggi).*
