# Clustering Saham LQ45 Berdasarkan Karakter Pergerakan Harga Menggunakan K-Means

## Gambaran Umum Proyek

Proyek ini bertujuan untuk mengelompokkan saham-saham LQ45 di Bursa Efek Indonesia (BEI) secara otomatis berdasarkan karakter pergerakan harganya, tanpa memberikan label atau petunjuk apa pun kepada model terlebih dahulu.

Tujuan utama proyek ini adalah menerapkan konsep *Unsupervised Learning*, yaitu menemukan pola tersembunyi dari data, serta mengeksplorasi bagaimana hasil *clustering* dapat membantu memahami profil risiko dan imbal hasil saham dalam analisis portofolio.

Dalam proyek ini, digunakan satu algoritma *machine learning*, yaitu K-Means Clustering.

## Tujuan Proyek

- Mengunduh data harga historis 30 saham LQ45 dari Yahoo Finance.
- Menghitung fitur karakteristik setiap saham (volatilitas, return rata-rata, dan harga terakhir).
- Melakukan normalisasi data agar skala fitur seragam.
- Menentukan jumlah *cluster* menggunakan Metode Elbow.
- Mengelompokkan saham menggunakan K-Means Clustering.
- Menginterpretasikan dan memberi nama setiap *cluster* berdasarkan karakteristik rata-ratanya.
- Memvisualisasikan hasil *clustering*.
- Menghasilkan insight berdasarkan hasil analisis.

## Dataset

Data yang digunakan dalam proyek ini adalah data harga penutupan harian saham yang diunduh dari Yahoo Finance.

- **Sumber:** Yahoo Finance (melalui library `yfinance`)
- **Jumlah Saham:** 30 saham (format ticker `KODE.JK`)
- **Periode:** 1 tahun terakhir (16 Juni 2025 s/d 15 Juni 2026)
- **Jumlah Hari Perdagangan:** 243 hari
- **Dimensi Data:** 243 baris × 30 kolom (baris = hari, kolom = saham)
- **Jenis Harga:** Harga penutupan (*Close*) dengan penyesuaian *corporate actions* (`auto_adjust=True`)

Daftar saham yang dianalisis:

| Sektor | Saham |
|---|---|
| Perbankan | BBCA, BBRI, BMRI, BBTN, BJBR |
| Telekomunikasi | TLKM, EXCL, ISAT |
| Konsumen & Makanan | UNVR, GGRM, HMSP, ICBP, INDF, CPIN |
| Farmasi & Ritel | KLBF, ACES |
| Pertambangan & Energi | PTBA, ANTM, INCO, MDKA, ADRO, PGAS |
| Industri, Infrastruktur & Properti | ASII, JSMR, SMGR, INKP, TKIM, BSDE, PWON |
| Media | MNCN |

Data diunduh secara otomatis dari internet menggunakan `yfinance`, sehingga hasil dapat berbeda jika notebook dijalankan pada tanggal lain.

## Tools dan Teknologi

Tools dan library yang digunakan dalam proyek ini:

- **Python:** Bahasa pemrograman utama.
- **yfinance:** Mengunduh data harga saham dari Yahoo Finance.
- **Pandas dan NumPy:** Pengolahan dan manipulasi data.
- **Matplotlib:** Visualisasi data.
- **Scikit-learn:** Normalisasi (`StandardScaler`) dan algoritma `KMeans`.
- **Jupyter Notebook:** Lingkungan untuk analisis dan eksperimen.

## Tahapan Pengerjaan Proyek

1. **Import Library:** Mengimpor library yang dibutuhkan untuk pengolahan data, visualisasi, dan *machine learning*.
2. **Penentuan Daftar Saham:** Memilih 30 saham dari berbagai sektor sebagai objek analisis.
3. **Data Acquisition:** Mengunduh harga penutupan harian selama 1 tahun menggunakan `yfinance`.
4. **Feature Engineering:** Menghitung tiga karakteristik setiap saham, yaitu Volatilitas (standar deviasi return harian), Return Harian Rata-rata, dan Harga Terakhir.
5. **Normalisasi Data:** Melakukan *Z-score normalization* menggunakan `StandardScaler` pada fitur yang digunakan untuk *clustering*.
6. **Penentuan Jumlah Cluster:** Menggunakan Metode Elbow dengan mencoba nilai k dari 1 sampai 10.
7. **K-Means Clustering:** Menjalankan K-Means dengan k = 3, `n_init=10`, `max_iter=300`, dan `random_state=42`.
8. **Interpretasi Cluster:** Menganalisis rata-rata karakteristik tiap *cluster* dan memberi nama yang bermakna.
9. **Visualisasi:** Membuat *scatter plot* Volatilitas vs Return (lengkap dengan centroid) dan *bar chart* distribusi saham per *cluster*.
10. **Ringkasan Hasil:** Merangkum profil dan anggota setiap *cluster*.

## Fitur yang Digunakan

Tiga fitur dihitung untuk setiap saham, namun **hanya dua fitur yang digunakan sebagai input K-Means**:

| Fitur | Digunakan untuk Clustering | Keterangan |
|---|:---:|---|
| Volatilitas (%) | Ya | Standar deviasi return harian; semakin tinggi, semakin fluktuatif |
| Return Harian Rata-rata (%) | Ya | Rata-rata perubahan harga harian selama 1 tahun |
| Harga Terakhir (Rp) | Tidak | Hanya ditampilkan sebagai informasi pendukung pada ringkasan *cluster* |

Dengan hanya memakai dua fitur, hasil *clustering* dapat divisualisasikan langsung pada grafik 2D (sumbu X = Volatilitas, sumbu Y = Return).

## Model Machine Learning

Algoritma yang digunakan adalah **K-Means Clustering**, yang bekerja dengan cara:

1. Menempatkan K centroid secara acak.
2. Menugaskan setiap saham ke centroid terdekat.
3. Memperbarui posisi centroid berdasarkan rata-rata anggotanya.
4. Mengulang langkah 2 dan 3 hingga centroid tidak berubah.

Konfigurasi model:

| Parameter | Nilai |
|---|---|
| `n_clusters` | 3 |
| `n_init` | 10 |
| `max_iter` | 300 |
| `random_state` | 42 |

Jumlah *cluster* ditentukan melalui Metode Elbow (k = 1 sampai 10), kemudian dipilih k = 3. Notebook hanya menggunakan Metode Elbow; metrik evaluasi *clustering* lain seperti Silhouette Score tidak dihitung.

## Hasil Clustering

Distribusi dan karakteristik rata-rata setiap *cluster*:

| Cluster | Nama | Jumlah Saham | Volatilitas Rata-rata | Return Harian Rata-rata | Harga Terakhir Rata-rata |
|:---:|---|---:|---:|---:|---:|
| 0 | Saham Defensif (Kalem) | 16 | 2,0605% | -0,0847% | Rp 2.613 |
| 1 | Saham Agresif (Lincah) | 3 | 4,1441% | 0,2363% | Rp 7.863 |
| 2 | Saham Moderat (Seimbang) | 11 | 3,1723% | 0,0976% | Rp 3.185 |

Anggota setiap *cluster*:

| Cluster | Anggota |
|---|---|
| Saham Defensif (Kalem) | ACES, BBCA, BBRI, BJBR, BMRI, BSDE, CPIN, ICBP, INDF, JSMR, KLBF, MNCN, PGAS, PTBA, PWON, SMGR |
| Saham Agresif (Lincah) | GGRM, INCO, MDKA |
| Saham Moderat (Seimbang) | ADRO, ANTM, ASII, BBTN, EXCL, HMSP, INKP, ISAT, TKIM, TLKM, UNVR |

### Interpretasi Hasil

- **Saham Defensif (Kalem):** volatilitas paling rendah (2,06%). Namun, rata-rata return hariannya negatif (-0,0847%), artinya kelompok ini cenderung turun selama periode data. Nama "defensif" di sini merujuk pada tingkat fluktuasi yang rendah, bukan jaminan kinerja yang baik.
- **Saham Agresif (Lincah):** volatilitas tertinggi (4,14%) dengan rata-rata return harian tertinggi (0,2363%). Kelompok ini hanya terdiri dari 3 saham.
- **Saham Moderat (Seimbang):** berada di tengah-tengah, dengan volatilitas 3,17% dan return harian 0,0976%.
- Nama setiap *cluster* ditentukan manual oleh penulis berdasarkan rata-rata karakteristik, bukan dihasilkan oleh algoritma.

## Kesimpulan

Proyek ini menunjukkan penerapan *Unsupervised Learning* untuk mengelompokkan 30 saham LQ45 berdasarkan volatilitas dan return rata-rata harian menggunakan K-Means dengan k = 3.

Hasil *clustering* membagi saham menjadi tiga kelompok: Defensif (16 saham), Agresif (3 saham), dan Moderat (11 saham). Kelompok Defensif memiliki fluktuasi terendah, kelompok Agresif memiliki fluktuasi dan rata-rata return tertinggi, sedangkan kelompok Moderat berada di antara keduanya.

Secara keseluruhan, proyek ini memberikan pengalaman dalam menerapkan alur kerja *clustering*, mulai dari pengambilan data, *feature engineering*, normalisasi, penentuan jumlah *cluster*, pemodelan, hingga interpretasi hasil.
