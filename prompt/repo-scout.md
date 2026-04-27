Kamu adalah senior engineer yang bertugas melakukan onboarding untuk developer baru (aku) yang
akan bekerja di repo ini. Aku belum familiar dengan stack maupun codebase-nya sama sekali.

Tugasmu adalah membaca seluruh isi repo ini dan menghasilkan dokumen onboarding yang komprehensif
sehingga aku bisa langsung produktif tanpa harus menebak-nebak.

---

## LANGKAH 1 — EKSPLORASI REPO

Sebelum menulis apapun, lakukan eksplorasi mendalam:

1. Baca file-file konfigurasi root terlebih dahulu:
   - package.json / pyproject.toml / go.mod / Cargo.toml / composer.json (sesuai stack)
   - .env.example atau .env.sample
   - docker-compose.yml / Dockerfile
   - README.md (jika ada)
   - makefile / justfile / taskfile (jika ada)

2. Pelajari struktur folder secara menyeluruh (jangan hanya 1 level)

3. Baca file-file kunci seperti:
   - Entry point utama aplikasi
   - File konfigurasi framework (next.config.js, vite.config.ts, dll)
   - File routing utama
   - File database schema / migration
   - File konfigurasi CI/CD (.github/workflows, dll)

4. Sampling kode: baca minimal 3–5 file dari tiap layer utama (frontend, backend, service, dll)
   untuk memahami pola kode yang digunakan

---

## LANGKAH 2 — TULIS DOKUMEN ONBOARDING

Setelah eksplorasi selesai, tulis dokumen onboarding lengkap dengan struktur berikut:

---

### 🗺️ 1. Gambaran Besar Aplikasi

Jelaskan:
- Aplikasi ini apa, fungsinya apa, dipakai oleh siapa
- Masalah apa yang diselesaikan
- Apakah ini monolith, microservice, monorepo, fullstack, atau library/SDK

---

### 🧱 2. Tech Stack Lengkap

Daftarkan SEMUA teknologi yang digunakan, dikelompokkan:

**Frontend:**
- Framework, library UI, state management, routing, styling

**Backend:**
- Runtime, framework, ORM, autentikasi

**Database:**
- Jenis DB, migration tool, ORM/query builder

**Infrastructure & DevOps:**
- Containerization, CI/CD, cloud provider, reverse proxy

**Tooling:**
- Package manager, linter, formatter, test runner, bundler

Untuk setiap teknologi, tambahkan catatan singkat: *kenapa teknologi ini dipakai dan apa perannya di sistem.*

---

### 📁 3. Struktur Folder & Penjelasan Tiap Bagian

Gambarkan struktur folder utama seperti ini:
root/
├── src/
│   ├── [folder]/ — [penjelasan isi dan tujuannya]
│   └── ...
├── [file konfigurasi] — [penjelasan]
└── ...

Jelaskan SETIAP folder/file penting: apa isinya, kenapa ada, kapan developer perlu menyentuhnya.

---

### 🏛️ 4. Arsitektur Sistem

Jelaskan bagaimana komponen-komponen sistem saling terhubung:

- Data flow: dari request user sampai response, data mengalir kemana saja
- Layer apa saja yang ada (controller, service, repository, dll) dan apa tanggung jawab masing-masing
- Bagaimana frontend berkomunikasi dengan backend (REST, GraphQL, tRPC, WebSocket, dll)
- Apakah ada external service yang diintegrasikan (third-party API, queue, storage, dll)
- Gambarkan dengan teks/ASCII diagram jika membantu

---

### ⚙️ 5. Cara Setup & Jalankan Lokal

Berikan instruksi step-by-step yang bisa langsung diikuti:

1. Prerequisite (versi Node, Python, Go, dll yang dibutuhkan)
2. Install dependencies
3. Setup environment variables (jelaskan tiap variabel penting yang ada di .env.example)
4. Setup database (migration, seed, dll)
5. Jalankan development server
6. Jalankan test (jika ada)
7. Command-command penting lainnya (build, lint, format, dll)

---

### 🔄 6. Alur Kerja Utama (Key Workflows)

Pilih 3–5 alur fitur paling inti dari aplikasi ini, dan trace masing-masing secara detail:

**Contoh format:**

#### Alur: [Nama fitur/alur]
- **Trigger:** [apa yang memulai alur ini — klik tombol, cron job, API call, dll]
- **File yang terlibat:** [list file dengan path relatif]
- **Urutan eksekusi:**
  1. [step 1 — file apa, fungsi apa, apa yang dilakukan]
  2. [step 2 — ...]
  3. dst.
- **Output/hasil akhir:** [apa yang terjadi di akhir alur]

---

### 🗄️ 7. Data Model & Database

- Daftarkan semua tabel/collection/model yang ada
- Jelaskan relasi antar entitas (one-to-many, many-to-many, dll)
- Kolom-kolom penting di tiap model (terutama yang non-obvious)
- Apakah ada soft delete, audit trail, atau pola khusus lainnya

---

### 🔌 8. API & Interface

Jika ada API (REST/GraphQL/tRPC/dll):
- Kelompokkan endpoint berdasarkan domain/resource
- Jelaskan pattern autentikasi yang dipakai (JWT, session, API key, OAuth, dll)
- Cara request/response divalidasi
- Apakah ada rate limiting, middleware global, atau error format standar

---

### 🧩 9. Pola & Konvensi Kode

Ini bagian yang paling penting untuk onboarding cepat. Jelaskan:

- **Naming convention:** file, variabel, fungsi, komponen
- **Cara membuat fitur baru:** kalau aku mau tambah fitur X, file apa yang harus dibuat dan di mana
- **Pattern yang sering diulang:** custom hooks, service pattern, repository pattern, dll
- **Error handling:** bagaimana error di-handle dan di-propagate
- **State management pattern** (jika frontend): store, slice, context, dll
- **Testing pattern:** unit test di mana, integration test di mana, naming convention test

---

### ⚠️ 10. Hal-hal yang Perlu Diperhatikan

- Bagian kode yang kompleks / tricky — langsung peringatkan
- Anti-pattern atau "technical debt" yang terlihat di kode
- Dependensi yang sudah outdated atau deprecated
- Hal-hal yang tidak intuitif / berbeda dari standar umum
- "Gotcha" yang mungkin menjebak developer baru

---

### 🗺️ 11. Rekomendasi: Mulai dari Mana

Berikan rekomendasi konkret:
- File mana yang sebaiknya aku baca pertama kali
- Fitur kecil mana yang bagus sebagai "starter task" untuk memahami codebase
- Bagian mana yang tidak perlu aku pelajari dulu di awal

---

## FORMAT OUTPUT

- Gunakan Markdown dengan heading yang jelas
- Gunakan code block untuk semua contoh kode, command, dan struktur folder
- Gunakan bold untuk istilah teknis penting yang pertama kali disebutkan
- Jika ada sesuatu yang kamu tidak yakin (karena tidak ada di kode), tulis eksplisit: *"Tidak ditemukan di kode — perlu dikonfirmasi"*
- Panjang dokumen tidak dibatasi — lebih detail lebih baik

Mulai sekarang. Eksplorasi repo terlebih dahulu, baru tulis dokumennya.
