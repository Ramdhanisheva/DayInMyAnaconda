# Analisis Data Eksploratif: Faktor-Faktor Penentu Kepuasan Penumpang Maskapai Penerbangan (Airline Passenger Satisfaction)

Proyek Mata Kuliah **Statistika & Probabilitas (STATPROB)**  
**Kelompok:** Day in My Anaconda  
**Dataset Utama:** `data/train.csv` (103.904 baris observasi, 24 variabel analitis)

---

## Pembagian Peran Anggota Kelompok

Untuk menjaga alur analisis tetap runtut dan mendalam, pengerjaan tugas besar ini dibagi ke dalam tiga modul kerja:

1. **Dyah Indana Zulfa** — **Modul 1: Pemahaman Data & Audit Kualitas Data**
   - Berkas: [`bagi-tugas/tugas2(dyah).ipynb`](bagi-tugas/tugas2(dyah).ipynb)
   - Lingkup kerja: Pemuatan data mentah `train.csv`, pembersihan kolom indeks tidak relevan, verifikasi integritas baris/ID unik, penyusunan kamus data dan taksonomi variabel, pengujian mekanisme data hilang (*Missing at Random* / MAR), komparasi empiris metode imputasi keterlambatan, analisis nilai rating 0, serta evaluasi keterbatasan metode IQR pada variabel *zero-inflated*.

2. **Andika Pratama** — **Modul 2: Analisis Univariat & Karakteristik Penumpang**
   - Berkas: [`bagi-tugas/tugas3(andika).ipynb`](bagi-tugas/tugas3(andika).ipynb)
   - Lingkup kerja: Pemeriksaan keseimbangan variabel target (`satisfaction`), sebaran demografi penumpang (jenis kelamin, usia, loyalitas pelanggan), profil perjalanan (tujuan perjalanan dan kelas kabin), serta pemetaan skor rata-rata pada 14 aspek fasilitas layanan maskapai.

3. **Sheva Ramadhan** — **Modul 3: Analisis Bivariat, Uji Confounding, & Pemodelan Prediktif**
   - Berkas: [`bagi-tugas/tugas1(sheva).ipynb`](bagi-tugas/tugas1(sheva).ipynb)
   - Lingkup kerja: Pengujian ukuran efek kategorikal (Cramér's V), deteksi kurva non-linear "U" pada skor layanan (Grafik Kunci 1), analisis pengaruh usia (kurva U terbalik) dan titik jenuh keterlambatan (> 15 menit), pembongkaran korelasi semu jarak tempuh penerbangan per kelas kabin (Grafik Kunci 2 / *Simpson's Paradox*), perancangan fungsi *preprocessing* data-driven, serta pengujian validasi model baseline internal (partisi data 80:20).

*Seluruh modul di atas juga telah digabungkan secara utuh dan berurutan pada berkas master:* [**`eda_DayInMyAnaconda.ipynb`**](eda_DayInMyAnaconda.ipynb).

---

## Ringkasan Temuan Utama

Dari 103.904 observasi penumpang pada data latih, berikut adalah kesimpulan pokok yang diperoleh:

- **Hubungan Non-Linear pada Skor Layanan:** Fasilitas layanan digital (*Inflight wifi service* dan *Online boarding*) tidak berhubungan secara linear dengan kepuasan. Penumpang yang memberi rating 1, 2, dan 3 sama-sama memiliki tingkat kepuasan rendah (~12% hingga 25%), lalu melonjak tajam pada rating 4 (~60%) dan rating 5 (87% hingga 99%).
- **Sinyal Khusus Rating 0 (Bukan Missing Value):** Nilai 0 pada survei fasilitas menandakan penumpang yang tidak menggunakan layanan tersebut, di mana kelompok ini memiliki tingkat kepuasan mencapai **99,7%** pada fasilitas wifi. Mengubah nilai 0 menjadi median atau rata-rata adalah kekeliruan yang menghilangkan sinyal prediktif penting.
- **Korelasi Jarak Penerbangan Bersifat Semu (*Confounding*):** Jarak tempuh secara agregat berkorelasi positif (+0,30) terhadap kepuasan. Namun, jika dilihat per kelas kabin (*Simpson's Paradox*), kepuasan penumpang pada kelas ekonomi (*Eco* dan *Eco Plus*) justru menurun seiring bertambahnya jarak. Jarak penerbangan semata-mata merupakan proksi dari kelas kabin *Business Class*.
- **Variabel Paling Menentukan:** Variabel kelas kabin (*Class*, Cramér's V = 0,505), tujuan perjalanan (*Type of Travel*, 0,449), dan loyalitas pelanggan (*Customer Type*, 0,188) merupakan faktor utama penentu kepuasan. Faktor jenis kelamin (*Gender*) terbukti tidak memiliki pengaruh yang berarti (Cramér's V = 0,012).
- **Titik Jenuh Efek Keterlambatan (*Delay*):** Penurunan kepuasan paling sensitif terjadi pada rentang keterlambatan 1 hingga 15 menit pertama. Melewati 15 menit, tingkat kepuasan cenderung stabil di angka ~35%, bahkan hingga durasi keterlambatan di atas 3 jam.
- **Mekanisme Data Hilang Keterlambatan Kedatangan:** Sebanyak 310 data hilang pada `Arrival Delay in Minutes` (0,30%) terbukti berstatus *Missing at Random* (MAR) karena berkaitan dengan keterlambatan keberangkatan. Mengimputasi nilai hilang dengan nilai `Departure Delay in Minutes` menghasilkan galat absolut rata-rata (MAE) sebesar **4,97 menit**, jauh lebih presisi dibandingkan imputasi median (**15,18 menit**).
- **Validasi Model Prediksi:** Model non-linear (*HistGradientBoosting*) mencapai akurasi **96,4%** dan ROC-AUC **0,995**, unggul signifikan dibanding model linear (*Logistic Regression*, akurasi 90,3% dan ROC-AUC 0,966), mengonfirmasi bahwa pola non-linear dan interaksi yang ditemukan pada tahap EDA memang nyata.

---

## Struktur Direktori Repository

```
proyek-eda-DayInMyAnaconda/
├── data/
│   ├── train.csv                      # Dataset utama (103.904 baris, 24 fitur)
│   └── test.csv
├── eda_DayInMyAnaconda.ipynb          # Master Notebook: Laporan Terpadu (Bab I s.d. IV)
├── bagi-tugas/                        # Lembar kerja pembagian modul per anggota
│   ├── tugas1(sheva).ipynb            # Modul 3: Bivariate, Confounding & ML (Sheva)
│   ├── tugas2(dyah).ipynb             # Modul 1: Data Understanding & Quality (Dyah)
│   ├── tugas3(andika).ipynb           # Modul 2: Univariate Analysis & Profiling (Andika)
│   └── tugas4(hisyam).ipynb
├── .gitignore
└── README.md
```

---

## Rekomendasi Manajerial untuk Maskapai

1. **Prioritas Pembenahan Layanan Digital:** Rata-rata skor terendah berada pada fasilitas *Inflight wifi service* (2,73) dan *Ease of Online booking* (2,76). Mengingat kedua variabel ini memiliki pengaruh besar terhadap kepuasan, alokasi investasi pada infrastruktur jaringan wifi di pesawat dan kemudahan aplikasi pemesanan tiket menjadi prioritas yang paling berdampak.
2. **Perhatian Khusus bagi Penumpang Bisnis Non-Loyal:** Penumpang non-loyal yang bepergian untuk urusan bisnis memiliki tingkat kepuasan terendah (hanya 23,7% puas, berbanding 70,6% pada penumpang bisnis loyal). Program fleksibilitas jadwal dan insentif korporat diperlukan untuk mempertahankan segmen ini.
3. **Penyediaan Fasilitas Kenyamanan pada Rute Ekonomi Jarak Jauh:** Penambahan jarak tempuh pada kelas ekonomi tidak meningkatkan kepuasan karena faktor kelelahan fisik. Penyediaan ruang kaki yang memadai atau distribusi konsumsi secara berkala dapat membantu memitigasi penurunan kepuasan pada penerbangan rute menengah hingga jauh.
4. **Respon Cepat pada 15 Menit Pertama Keterlambatan:** Karena penurunan kepuasan paling drastis terjadi pada 15 menit pertama delay, maskapai perlu menerapkan prosedur komunikasi berkala dan transparansi informasi sebelum keterlambatan menyentuh angka 15 menit.

---

## Cara Menjalankan Notebook

1. Pastikan Python 3.9+ dan dependensi berikut telah terpasang:
   ```bash
   pip install pandas numpy matplotlib seaborn scipy scikit-learn jupyter
   ```
2. Jalankan Jupyter Lab atau Jupyter Notebook:
   ```bash
   jupyter lab eda_DayInMyAnaconda.ipynb
   ```
3. Seluruh sel kode di dalam berkas notebook telah dieksekusi lengkap dengan output tabel data dan grafik beresolusi tinggi.
