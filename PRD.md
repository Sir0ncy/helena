# Helena: Dashboard Intelijen CVE & Analisis Kerentanan menggunakan LLM

## 1. Masalah yang Dihadapi
Di era AI saat ini, setiap pagi bisa berubah menjadi situasi darurat. Model AI keamanan otonom terus memindai kode sumber lama dan menemukan kerentanan kritis **CVSS 9.0+** berusia 20–30 tahun yang tersembunyi di fungsi dasar sederhana seperti `memcpy()`. Kode yang bertahan puluhan tahun dari audit manusia kini dapat ditembus dalam hitungan menit.

Bagi developer solo maupun tim kecil, kondisi ini menimbulkan dua masalah utama:
- **Kelelahan Alert Tanpa Konteks (Alert Fatigue):** Pemindai standar hanya memuntahkan daftar CVE mentah tanpa memahami arsitektur server pengguna. Kerentanan CVSS 9.8 pada modul yang tidak diaktifkan dianggap sama daruratnya dengan celah RCE di port publik, sehingga membuang waktu triase manual.
- **Informasi Ancaman Terpecah:** Memantau CVE terbaru, mengecek status eksploitasi aktif (CISA KEV), dan mencocokkannya dengan versi package proyek harus dilakukan secara terpisah di berbagai situs.

## 2. Solusi yang Ditawarkan
**Helena** adalah aplikasi web ringan yang menggabungkan dua fitur utama dalam satu platform:
- **Dashboard Intelijen CVE Real-Time:** Menampilkan daftar CVE terbaru, skor tingkat keparahan (CVSS), dan status eksploitasi aktif (CISA KEV) secara otomatis lewat sinkronisasi berkala—tanpa membebani kuota LLM saat halaman dibuka.
- **Analisis Kerentanan Kontekstual LLM:** Menggunakan pencarian akurat ke database kerentanan publik (OSV.dev) lalu memanfaatkan model LLM untuk menganalisis apakah kerentanan tersebut benar-benar bisa dieksploitasi pada konfigurasi aplikasi/server pengguna.

## 3. Skenario Penggunaan Utama
**Situasi:** Pagi hari sebelum merilis pembaruan aplikasi web, seorang backend engineer membuka Helena untuk memastikan sistemnya aman setelah mendengar kabar ditemukannya kerentanan kritis baru berbasis buffer overflow (`memcpy`) pada pustaka populer.

1. **Memantau Dashboard Utama:** Devs melihat ringkasan metrik di bagian atas dashboard (jumlah CVE 24 jam terakhir, jumlah CVE Kritis, dan status CISA KEV). Di tabel utama, kerentanan baru tersebut muncul dengan label `Critical (CVSS 9.1)` dan lencana `Actively Exploited (KEV)`.
2. **Cek Cepat Dampak Satu CVE:** Ia mengklik tombol **Analisis Dampak** pada baris CVE tersebut untuk membuka panel samping, lalu memberikan konteks kondisi servernya. **LLM** menganalisis vektor serangan CVE tersebut terhadap konteks servernya dan menurunkan tingkat risiko efektif menjadi `Mitigated / Low`.
3. **Hasil Analisis:** **LLM** mengekstrak daftar package dan versi, sistem mencocokkannya secara deterministik ke **OSV.dev** dan **CISA KEV**, lalu **LLM** akan menilai ulang setiap temuan berdasarkan konteks deployment. Hasil akhirnya adalah daftar prioritas perbaikan yang ringkas memisahkan celah *false positive* dan memberikan perintah upgrade siap salin untuk kerentanan yang benar-benar berbahaya.

## 4. Fitur Utama & Alur Kerja Sistem

### A. Dashboard Ancaman Terbaru
- **Sinkronisasi Otomatis:** Pekerjaan latar belakang (cron setiap 3 jam) menarik data terbaru dari **NVD API 2.0** dan katalog **CISA KEV** ke database PostgreSQL.
- **Ringkasan KPI & Filter:** Menampilkan 4 kartu metrik utama (CVE Baru, CVE Kritis, Status KEV, Ekosistem Terdampak) serta tabel interaktif dengan filter tingkat keparahan (`Critical`, `High`, `Medium`), filter khusus `KEV Only`, dan pencarian kata kunci.
- **Panel Uji Cepat CVE:** Setiap baris CVE pada dashboard dapat diklik untuk membaca ringkasan kerentanan dan langsung diuji kecocokannya terhadap konfigurasi pengguna.