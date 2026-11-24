# Indonesia Coffee Chain Wars — Data Analysis & Power BI Dashboard

## Tentang project
Di project ini saya mencoba menganalisis data persaingan beberapa jaringan coffee shop di Indonesia. Saya menggunakan dataset *Indonesia Coffee Chain Wars Enriched 2026* untuk melihat perbandingan performa brand, wilayah, metode pemesanan, karakteristik gerai, serta gambaran perilaku pelanggan.

Tujuan saya bukan hanya menampilkan angka dalam dashboard, tetapi mencoba mencari tahu bagian mana yang terlihat menonjol dan hal apa yang bisa menjadi bahan pertimbangan bisnis, misalnya performa brand, pola diskon, loyalitas pelanggan, dan faktor yang berpotensi membuat pelanggan berpindah.

> **Catatan penting:** dataset ini berisi data yang sudah diperkaya (*enriched*). Hasil analisis merupakan gambaran berdasarkan data yang tersedia, bukan laporan keuangan resmi atau hasil pengukuran langsung dari seluruh gerai coffee shop di Indonesia.

## Tools yang digunakan
- Microsoft Excel — membuka dan memeriksa struktur data.
- Power Query — proses pengecekan dan persiapan data sebelum digunakan.
- Power BI Desktop — membuat visualisasi dan dashboard.
- DAX — membuat measure/KPI untuk perhitungan dashboard.

## Isi repository
- `dashboard/` — tempat file Power BI (`.pbix`) yang saya buat.
- `data/` — dataset sumber dalam format Excel.
- `dokumentasi/` — catatan proses dan informasi pendukung.
- `reports/` — hasil pemeriksaan kualitas data dan ringkasan agregasi.
- `screenshots/` — gambar tampilan dashboard.
- `README.md` — penjelasan project.

## 1. Memahami data
Dataset utama memiliki **13,000 baris dan 26 kolom**. Beberapa kolom yang saya gunakan antara lain:
- `coffee_brand`, `city_location`, dan `store_format` untuk melihat perbandingan brand, kota, dan format gerai.
- `daily_cups_sold` dan `daily_revenue_estimate_idr` untuk melihat volume penjualan dan estimasi pendapatan.
- `discount_voucher_pct`, `base_price_idr`, dan `net_discounted_price_idr` untuk melihat harga serta diskon.
- `customer_review_rating`, `primary_customer_segment`, dan `customer_brand_loyalty_tier` untuk melihat gambaran pelanggan.
- `competitor_density_500m` dan `primary_brand_switch_driver` untuk melihat konteks persaingan dan alasan perpindahan pelanggan yang tercatat di dataset.

## 2. Data cleaning dan data quality check
Saya melakukan pemeriksaan awal sebelum data dipakai di Power BI. Hasil pemeriksaannya:

| Pemeriksaan | Hasil |
|---|---:|
| Jumlah baris | 13,000 |
| Jumlah kolom | 26 |
| Nilai kosong | 0 |
| Duplikat seluruh baris | 0 |
| Duplikat `store_transaction_id` | 0 |
| ID transaksi unik | 13,000 |

Dari pemeriksaan tersebut, saya tidak menemukan nilai kosong, baris duplikat identik, maupun ID transaksi yang berulang. Jadi, **tidak ada baris yang perlu dihapus karena missing value atau duplikat**.

Persiapan data yang saya lakukan/pertimbangkan:
1. Memeriksa nama kolom dan tipe data agar sesuai dengan isi kolomnya.
2. Memastikan kolom numerik seperti harga, jumlah cup, rating, diskon, dan margin terbaca sebagai angka.
3. Memastikan kolom kategori seperti brand, kota, format gerai, metode pemesanan, segmen pelanggan, dan tier loyalitas dapat digunakan sebagai filter atau kategori visual.
4. Memeriksa hubungan perhitungan antara jumlah cup, harga bersih, dan estimasi pendapatan.

Saya juga memperhatikan bahwa `discount_voucher_pct` dan `outlet_operating_margin_estimate_pct` disimpan dalam skala 0–100. Saat membuat measure persentase di DAX, nilainya perlu dibagi 100 sebelum diformat sebagai Percentage agar tidak tampil 100 kali lebih besar.

### Pemeriksaan rumus estimasi pendapatan
Saya memeriksa apakah nilai `daily_revenue_estimate_idr` sesuai dengan rumus:

`daily_cups_sold × net_discounted_price_idr`

Pada data ini, selisih maksimum hasil perhitungan dengan kolom estimasi pendapatan adalah **Rp0**. Artinya, kolom tersebut konsisten dengan rumus perkalian tersebut. Karena itu, saya tetap menyebutnya **estimasi pendapatan**, bukan pendapatan aktual yang sudah diaudit.

## 3. Metode analisis
Metode yang saya gunakan adalah **Exploratory Data Analysis (EDA)** dan analisis deskriptif. Saya membandingkan nilai total, rata-rata, dan jumlah record berdasarkan kategori tertentu.

Beberapa perhitungan yang digunakan:
- Total Estimated Revenue = jumlah dari `daily_revenue_estimate_idr`.
- Total Cups Sold = jumlah dari `daily_cups_sold`.
- Average Selling Price = Total Estimated Revenue dibagi Total Cups Sold.
- Average Customer Rating = rata-rata `customer_review_rating`.
- Average Discount = rata-rata `discount_voucher_pct`.
- Average Operating Margin = rata-rata `outlet_operating_margin_estimate_pct` (tetap diperlakukan sebagai estimasi).

Saya juga membandingkan hasil berdasarkan brand, kota, metode pemesanan, format gerai, segmen pelanggan, tier loyalitas, dan alasan utama perpindahan brand.

## 4. Hasil dan insight awal
Berikut beberapa temuan dari data yang saya olah:

1. **Performa brand:** berdasarkan total estimasi pendapatan, Kopi Kenangan berada di posisi tertinggi dengan sekitar **Rp34,587,021,444**. Sementara itu, Tomoro Coffee memiliki total cup terjual paling tinggi, yaitu **3,201,451 cup**. Jadi, brand dengan estimasi pendapatan tertinggi tidak selalu sama dengan brand dengan volume cup tertinggi.
2. **Wilayah:** Jakarta Selatan memiliki total estimasi pendapatan tertinggi, sekitar **Rp17,942,953,103** pada dataset ini.
3. **Metode pemesanan:** Mobile App Pickup memiliki total estimasi pendapatan paling besar dibanding metode pemesanan lain pada data ini.
4. **Loyalitas pelanggan:** kategori Value-Conscious-Regular merupakan kelompok dengan jumlah record terbanyak. Rata-rata diskonnya sekitar **21.8%**, sedangkan kelompok Promo-Chaser-Opportunist menerima rata-rata diskon sekitar **38.6%**. Ini memberi gambaran bahwa karakter kelompok pelanggan di dataset berbeda-beda.
5. **Alasan perpindahan brand:** alasan yang paling sering tercatat adalah `Cheaper-Competitor-Opening` sebanyak **3,961 record**. Alasan `Taste-Inconsistency-Drop` memiliki rata-rata rating paling rendah di antara kategori alasan perpindahan pada data ini.

Saya tidak langsung menyimpulkan bahwa diskon menyebabkan kenaikan penjualan. Korelasi sederhana antara persentase diskon dan jumlah cup pada data ini sekitar **0.35**, sedangkan korelasi diskon dengan estimasi pendapatan sekitar **-0.34**. Korelasi hanya menunjukkan hubungan statistik pada data, bukan bukti sebab-akibat.

## 5. Batasan analisis
Ada beberapa hal yang perlu diperhatikan agar hasilnya tidak salah diartikan:
- Dataset tidak memiliki kolom tanggal/periode, sehingga saya tidak membuat analisis tren bulanan atau tahunan.
- `store_transaction_id` adalah ID record/transaksi, bukan ID pelanggan atau ID gerai yang terpisah. Jumlah record tidak boleh disebut sebagai jumlah pelanggan unik atau jumlah outlet unik.
- `daily_revenue_estimate_idr` merupakan estimasi yang dihitung dari jumlah cup dan harga bersih, bukan angka pendapatan aktual yang sudah diverifikasi.
- `outlet_operating_margin_estimate_pct` juga merupakan estimasi, sehingga tidak saya samakan dengan margin aktual perusahaan.
- Korelasi antara dua variabel tidak otomatis berarti satu variabel menyebabkan perubahan pada variabel lainnya.
- Hasil analisis menggambarkan dataset yang digunakan dan tidak otomatis mewakili seluruh pasar coffee shop Indonesia.

## 6. Kesimpulan
Dari project ini saya belajar bahwa analisis data bukan hanya membuat visualisasi yang terlihat menarik. Saya perlu memahami arti setiap kolom, mengecek kualitas data, memilih perhitungan yang sesuai, lalu menjelaskan hasilnya dengan tetap memperhatikan batasan dataset.

Dashboard ini saya buat supaya perbandingan performa brand, wilayah, metode pemesanan, karakteristik gerai, dan pelanggan bisa dilihat dengan lebih mudah. Hasilnya bisa menjadi titik awal untuk diskusi atau analisis lanjutan, bukan satu-satunya dasar untuk mengambil keputusan bisnis.

## Pengembangan selanjutnya
Jika tersedia data yang lebih lengkap, analisis ini bisa dikembangkan dengan data tanggal, ID outlet yang konsisten, ID pelanggan anonim, biaya operasional aktual, serta data transaksi historis. Dengan begitu, analisis tren, retensi pelanggan, dan profitabilitas bisa dilakukan dengan lebih kuat.

---
**Project:** Indonesia Coffee Chain Wars  
**Focus:** Data Analysis | Exploratory Data Analysis | Power BI Dashboard  
**Status:** Personal learning project
