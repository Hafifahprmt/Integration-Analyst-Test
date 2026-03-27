# Technical Rationale: Loan Application API

Dokumen ini menjelaskan alasan teknis di balik perancangan spesifikasi API pada file `loan-api-spec.yml`.

## 1. Pemilihan Method `POST`
Saya menggunakan metode `POST` untuk endpoint pengajuan pinjaman (`/v1/loans/apply`) karena:
- **Penciptaan Resource**: Sesuai standar RESTful, `POST` digunakan untuk membuat data baru di server.
- **Keamanan Data**: Data sensitif nasabah (ID, nominal, tujuan pinjaman) dikirim melalui *Request Body* yang terenkripsi (HTTPS), bukan melalui URL, guna mencegah kebocoran data pada log server.

## 2. Penggunaan OpenAPI 3.1
Implementasi standar OpenAPI 3.1 bertujuan untuk:
- **Validasi Ketat**: Memastikan input dari aplikasi mobile telah memenuhi syarat sebelum diproses oleh sistem backend.
- **Efisiensi QA**: Spesifikasi yang terdefinisi dengan jelas memudahkan tim Quality Assurance dalam membangun skrip pengujian otomatis (Automation Testing).

## 3. Strategi HTTP Response Codes
Untuk memberikan informasi yang akurat bagi tim Frontend, saya menetapkan status kode berikut:
- **201 Created**: Menandakan pengajuan berhasil diterima dan entri baru telah dibuat di database.
- **422 Unprocessable Entity**: Digunakan khusus untuk kegagalan logika bisnis (misalnya: nasabah sudah memiliki pinjaman aktif atau skor kredit di bawah batas minimum), meskipun format data sudah benar.
- **500 Internal Server Error**: Menandakan adanya kendala pada integrasi ke *Core Banking System*.

## 4. Alur Integrasi (End-to-End)
Setiap permintaan yang masuk melalui API akan diterima oleh layanan **Spring Boot**. Layanan ini kemudian akan melakukan validasi identitas ke **Core Banking System** sebelum memberikan konfirmasi status pengajuan kepada pengguna. Seluruh aktivitas ini dipantau secara real-time melalui **Kibana** untuk menjaga aspek ketersediaan layanan (*high availability*).
