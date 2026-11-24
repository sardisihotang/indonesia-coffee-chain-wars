# Ringkasan Analisis — Indonesia Coffee Chain Wars

## Ruang lingkup
- Dataset: Indonesia Coffee Chain Wars Enriched 2026
- Record: 13,000
- Kolom: 26
- Metode: Exploratory Data Analysis (EDA), statistik deskriptif, agregasi dan perbandingan kategori.

## KPI keseluruhan
| Metrik | Nilai |
|---|---:|
| Total Estimated Revenue | Rp104,918,419,573 |
| Total Cups Sold | 7,487,690 |
| Average Selling Price (revenue / cups) | Rp14,012 |
| Average Customer Rating | 4.52 |
| Average Discount | 25.69% |
| Average Estimated Operating Margin | 29.51% |

## Temuan utama
- Brand dengan estimasi pendapatan tertinggi: Kopi Kenangan (Rp34,587,021,444).
- Brand dengan volume cup tertinggi: Tomoro Coffee (3,201,451 cup).
- Kota dengan estimasi pendapatan tertinggi: Jakarta Selatan (Rp17,942,953,103).
- Metode pemesanan dengan estimasi pendapatan tertinggi: Mobile App Pickup.
- Tier loyalitas terbanyak: Value-Conscious-Regular (5,827 record).
- Pemicu perpindahan brand yang paling sering tercatat: Cheaper-Competitor-Opening (3,961 record).

## Data quality
- Missing values: 0
- Exact duplicate rows: 0
- Duplicate transaction IDs: 0
- Revenue formula max absolute difference: Rp0

## Interpretasi dan batasan
Kolom pendapatan dan margin mengandung kata `estimate`; keduanya perlu disebut sebagai estimasi. Tidak ada kolom tanggal, sehingga analisis tren waktu tidak dapat dilakukan. Tidak ada ID pelanggan atau ID outlet yang berdiri sendiri, sehingga jumlah record bukan jumlah pelanggan/outlet unik. Korelasi diskon dan cup (0.345) tidak membuktikan hubungan sebab-akibat.
