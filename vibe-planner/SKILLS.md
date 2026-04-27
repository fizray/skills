---
name: vibe-planner
description: >
  Gunakan skill ini setiap kali user ingin mengubah ide, braindump, cerita bebas, atau file (Excel/Word/PDF)
  tentang aplikasi web menjadi file plan.md yang siap dipakai oleh AI agent (Google AI Studio, Claude Code,
  Cursor, Bolt, Lovable, v0, dll) untuk membuat prototype atau MVP full stack secara one-shot.
  Trigger skill ini ketika user menyebut kata-kata seperti: "bikin plan", "buatkan prompt untuk agent",
  "jadikan plan.md", "mau prototype", "mau MVP", "vibecoding", "saya punya ide aplikasi", "tolong planin",
  atau ketika user menjelaskan fitur aplikasi yang ingin dibuat. Juga trigger ketika user upload file
  Excel/Word/PDF yang tampaknya adalah data atau template yang ingin dijadikan aplikasi.
---

# Vibe Planner — Brain-to-plan.md Converter

Skill ini mengubah ide mentah, braindump, atau file referensi menjadi `plan.md` yang sangat detail dan
eksplisit — sehingga AI agent bisa langsung membuat full stack MVP dalam satu shot tanpa perlu balik
tanya-tanya lagi.

---

## FASE 0 — Baca Input & Tentukan Jalur

Setelah user memberikan input (cerita, braindump, atau file), tentukan jalur:

### Jalur A: Input teks / cerita bebas
Langsung ke **Fase 1 — Klarifikasi Minimal**.

### Jalur B: Ada file Excel / Word / PDF terupload
1. Baca file tersebut (gunakan bash/python jika perlu untuk Excel/Word, atau baca langsung jika teks/PDF).
2. Ekstrak:
   - Struktur kolom (untuk Excel → calon schema DB/tabel)
   - Isi konten (untuk Word/PDF → calon fitur, alur, atau form fields)
3. Tampilkan summary singkat ke user: *"Saya mendeteksi [X tabel/sheet] dengan kolom [...]  — ini akan saya jadikan dasar schema database."*
4. Lanjut ke **Fase 1** sambil membawa konteks file tersebut.

---

## FASE 1 — Klarifikasi Minimal (Wajib Tanya Sebelum Generate)

Sebelum menulis plan.md, **wajib tanyakan** hal-hal berikut yang belum jelas dari input user.
Kumpulkan semua pertanyaan dalam SATU pesan, jangan bertahap.

### Pertanyaan wajib jika belum terjawab di input:

1. **Siapa penggunanya?** (internal perusahaan, publik umum, siswa, admin only, dll)
2. **Auth diperlukan?** (login/register? role-based? atau no-auth?)
3. **Stack preference?**
   - Pilihan A: Next.js + Firebase (cocok untuk Google AI Studio)
   - Pilihan B: Next.js + Supabase (portable, cocok untuk agent lain)
   - Pilihan C: Bebas — sebutkan preferensimu
4. **Agent yang akan dipakai?** (Google AI Studio / Claude Code / Cursor / Bolt / lainnya)
5. **Ada fitur yang TIDAK boleh ada di MVP?** (biar scope jelas)
6. **Deadline atau constraint teknis lain?** (opsional)

> Jika user sudah menjawab sebagian di braindump-nya, skip pertanyaan yang sudah terjawab.
> Kalau user bilang "terserah kamu" atau "bebas", tentukan sendiri dengan memberikan asumsi yang jelas di plan.md.

---

## FASE 2 — Tulis plan.md

Setelah semua info terkumpul, generate file `plan.md` dengan struktur berikut.
**Tulis sedetail mungkin** — AI agent harus bisa langsung build tanpa tanya balik.

Simpan ke: `/mnt/user-data/outputs/plan.md`

---

### TEMPLATE STRUKTUR plan.md

```markdown
# [Nama Aplikasi] — MVP Plan

> **Dibuat untuk:** [nama agent target, misal: Google AI Studio]
> **Stack:** [Next.js + Firebase / Next.js + Supabase / dll]
> **Tujuan:** One-shot prototype generation

---

## 1. Overview Aplikasi

[2-4 paragraf menjelaskan aplikasi ini secara naratif: apa masalah yang diselesaikan,
siapa penggunanya, bagaimana cara kerjanya secara umum. Tulis seperti menjelaskan ke developer.]

---

## 2. Target Pengguna & Role

| Role | Deskripsi | Akses |
|------|-----------|-------|
| [role] | [deskripsi] | [halaman/fitur yang bisa diakses] |

---

## 3. Tech Stack

### Frontend
- Framework: Next.js 14+ (App Router)
- Styling: Tailwind CSS + shadcn/ui
- State management: [Zustand / React Query / Context API]
- Form handling: React Hook Form + Zod

### Backend / Database
[Pilih salah satu dan isi detail:]

**Jika Firebase:**
- Firestore (database)
- Firebase Auth (authentication)
- Firebase Storage (file upload jika perlu)
- Firebase Hosting (opsional untuk deploy)

**Jika Supabase:**
- PostgreSQL via Supabase
- Supabase Auth
- Supabase Storage (jika perlu)
- Row Level Security (RLS) enabled

### Tooling
- TypeScript (strict mode)
- ESLint + Prettier
- [Package manager: npm / pnpm]

---

## 4. Fitur MVP (In Scope)

Daftar fitur yang HARUS ada di MVP. Untuk setiap fitur, tulis detail implementasinya.

### 4.1 [Nama Fitur]
- **Deskripsi:** [apa yang dilakukan fitur ini]
- **Halaman/komponen terkait:** [list halaman]
- **Logika bisnis penting:** [rules, validasi, kondisi khusus]
- **Interaksi UI:** [apa yang terjadi saat user klik/input/submit]

[Ulangi untuk setiap fitur]

### ❌ OUT OF SCOPE (tidak perlu dibuat di MVP)
- [fitur yang sengaja dihilangkan]
- [fitur yang bisa ditambah nanti]

---

## 5. Struktur Database / Data Model

[Untuk Firebase — Firestore Collections:]

### Collection: `[nama_collection]`
```
{
  id: string (auto),
  [field]: [type] — [deskripsi],
  [field]: [type] — [deskripsi],
  createdAt: timestamp,
  updatedAt: timestamp
}
```

[Untuk Supabase — PostgreSQL Tables:]

```sql
CREATE TABLE [nama_tabel] (
  id UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  [kolom] [tipe] NOT NULL, -- [deskripsi]
  created_at TIMESTAMPTZ DEFAULT NOW(),
  updated_at TIMESTAMPTZ DEFAULT NOW()
);
```

[Jika ada file Excel yang diupload, schema ini harus mencerminkan struktur kolom dari file tersebut]

---

## 6. Struktur Halaman & Routing

```
/                          → [deskripsi halaman]
/login                     → [jika ada auth]
/dashboard                 → [halaman utama setelah login]
/[resource]                → [CRUD utama]
/[resource]/[id]           → [detail]
/[resource]/new            → [form tambah]
/[resource]/[id]/edit      → [form edit]
/admin                     → [jika ada admin panel]
```

Untuk setiap halaman penting, jelaskan:
- Komponen utama yang ditampilkan
- Data apa yang di-fetch
- Action apa yang bisa dilakukan user

---

## 7. Struktur Folder Project

```
src/
├── app/
│   ├── layout.tsx
│   ├── page.tsx
│   ├── (auth)/
│   │   ├── login/page.tsx
│   │   └── register/page.tsx
│   └── (dashboard)/
│       └── [sesuaikan dengan fitur]
├── components/
│   ├── ui/              ← shadcn components
│   └── [feature]/       ← komponen per fitur
├── lib/
│   ├── firebase.ts / supabase.ts
│   ├── utils.ts
│   └── validations/     ← Zod schemas
├── hooks/               ← custom React hooks
├── types/               ← TypeScript interfaces
└── constants/           ← nilai konstan
```

---

## 8. Alur Utama Aplikasi (User Flow)

Jelaskan step-by-step alur kerja utama:

### Alur: [Nama Alur, misal: User menambah data]
1. User membuka halaman [X]
2. User klik tombol "[Y]"
3. Form modal/halaman muncul dengan field: [list field]
4. User mengisi form dan klik "Simpan"
5. Validasi: [rules validasi]
6. Jika valid → data disimpan ke [collection/table] → redirect ke [halaman] → tampilkan toast "[pesan sukses]"
7. Jika invalid → tampilkan error inline di field terkait

[Ulangi untuk setiap alur penting]

---

## 9. Komponen UI Kritis

Daftar komponen yang harus dibangun (selain shadcn defaults):

| Komponen | Fungsi | Props Penting |
|----------|--------|---------------|
| [NamaKomponen] | [fungsi] | [props] |

**Panduan desain:**
- Warna utama: [sebutkan atau "ikuti shadcn default"]
- Mode gelap: [ya/tidak]
- Responsif: mobile-first
- Bahasa UI: [Indonesia / English]

---

## 10. Environment Variables yang Diperlukan

```env
# Firebase
NEXT_PUBLIC_FIREBASE_API_KEY=
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=
NEXT_PUBLIC_FIREBASE_PROJECT_ID=
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=
NEXT_PUBLIC_FIREBASE_APP_ID=

# Supabase (alternatif)
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
SUPABASE_SERVICE_ROLE_KEY=
```

---

## 11. Instruksi Khusus untuk AI Agent

> Bagian ini adalah instruksi langsung ke AI agent yang akan men-generate kode.

- Generate **semua file** dalam satu respons, jangan sebagian-sebagian
- Gunakan **TypeScript strict** di semua file
- Semua teks UI dalam bahasa **[Indonesia/English]**
- Gunakan **shadcn/ui** untuk semua komponen UI standar (button, input, modal, table, dll)
- Implementasikan **error handling** yang proper di setiap operasi async
- Tambahkan **loading state** di setiap aksi yang melibatkan network
- Jangan buat fitur yang tidak ada di daftar "In Scope" di atas
- [Tambahan instruksi spesifik untuk agent tertentu jika relevan]

**Urutan file yang harus dibuat:**
1. Setup konfigurasi (firebase.ts / supabase.ts, types/)
2. Layout dan komponen global
3. Halaman autentikasi (jika ada)
4. Halaman dan fitur utama (urut dari yang paling penting)
5. Komponen pendukung

---

## 12. Asumsi yang Dibuat

[Daftar semua keputusan yang dibuat atas nama user karena tidak disebutkan secara eksplisit]

- [Asumsi 1]: [alasan memilih ini]
- [Asumsi 2]: [alasan memilih ini]

---

## FASE 3 — Review & Iterasi

Setelah file plan.md dibuat:

1. Tampilkan ringkasan singkat ke user: *"plan.md sudah dibuat. Ini yang akan dibuild: [bullet 3-5 poin paling penting]"*
2. Tanya: *"Ada yang perlu diubah atau ditambah sebelum kamu kasih ke agent?"*
3. Jika ada revisi → update file plan.md yang sudah ada (jangan buat file baru)
4. Jika sudah oke → selesai

---

