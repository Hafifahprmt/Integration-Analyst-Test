# Technical Assessment: Integration Analyst - PT. XYZ

Dokumen ini berisi penjelasan mengenai arsitektur sistem dan integrasi untuk pengembangan aplikasi pinjaman online (pinjol) di PT. XYZ.

## 1. Deskripsi Arsitektur & Alur Integrasi (System Flow)

Berdasarkan diagram sistem yang ada, berikut adalah urutan alur kerja dari sisi pengguna hingga ke sistem inti perbankan:

### A. Client & Frontend Layer (Service Worker)
Alur dimulai dari perangkat pengguna. Penggunaan **Service Worker** memastikan aplikasi mobile PT. XYZ memiliki performa yang cepat, mampu menangani caching data, dan mendukung fitur luring (offline) serta push notification untuk status pinjaman nasabah.

### B. Integration & Contract Layer (OpenAPI 3.1)
Sebelum data dikirim ke server, terdapat **OpenAPI 3.1** yang berfungsi sebagai "kontrak integrasi". 
* **Standardisasi**: Memastikan tim Frontend dan Backend memiliki persepsi yang sama mengenai struktur data (Request/Response).
* **Validasi**: Menjamin data pengajuan pinjaman yang masuk sudah sesuai standar keamanan finansial sebelum diproses lebih lanjut.

### C. Business Logic Layer (Spring Boot & SOA)
Data yang dikirim melalui API akan diproses oleh layanan yang dibangun dengan framework **Spring Boot**.
* **Microservices**: Implementasi layanan yang terpisah-pisah (seperti layanan verifikasi, layanan skor kredit, dan layanan pencairan).
* **SOA (Service Oriented Architecture)**: Arsitektur ini mengelola bagaimana layanan-layanan tersebut berinteraksi secara modular agar sistem tetap stabil, aman, dan mudah dikembangkan secara independen.

### D. Data Source Layer (Core Banking System)
Layanan Spring Boot bertindak sebagai jembatan menuju **Core Banking System**. Ini adalah pusat data utama di mana saldo, limit pinjaman, dan riwayat transaksi nasabah divalidasi dan disimpan secara permanen.

### E. Operations & Monitoring (Jenkins & Kibana)
Untuk menjaga kualitas layanan selama siklus pengembangan dan operasional:
* **Jenkins**: Mengotomatiskan proses pengujian dan penyebaran kode (CI/CD) agar setiap fitur baru bisa dirilis dengan aman dan cepat.
* **Kibana**: Berfungsi sebagai pusat pemantauan (monitoring). Jika terjadi kegagalan integrasi atau eror pada aplikasi, log data akan divisualisasikan di sini untuk mempercepat proses troubleshooting.

---

## 2. Ringkasan Komponen Utama

| Komponen | Peran dalam Proyek Pinjaman Online |
| :--- | :--- |
| **SOA** | Fondasi arsitektur agar layanan pinjaman bersifat modular dan scalable. |
| **OpenAPI 3.1** | Dokumentasi dan kontrak standar API antara mobile apps dan server. |
| **Spring Boot** | Framework utama untuk memproses logika bisnis dan integrasi sistem. |
| **Core Banking** | Sumber data utama (Single Source of Truth) untuk informasi nasabah. |
| **Jenkins** | Alat otomatisasi untuk deployment fitur secara Continuous Integration. |
| **Kibana** | Dashboard monitoring untuk memantau kesehatan integrasi sistem. |
