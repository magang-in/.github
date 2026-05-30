# Magang-in

**Platform Skill Matching Berbasis Kecerdasan Buatan untuk Pencarian Magang**

Magang-in adalah platform berbasis website yang membantu mahasiswa menemukan posisi magang teknologi yang paling sesuai dengan skill mereka. Sistem menggunakan **Siamese Neural Network** untuk mencocokkan skill user dengan requirement lowongan secara otomatis — baik melalui input manual maupun ekstraksi langsung dari CV (PDF).

---

## Latar Belakang

- **77%** perusahaan kesulitan menemukan talenta dengan keterampilan yang sesuai (ManpowerGroup)
- **80%** lulusan perguruan tinggi Indonesia bekerja di bidang yang tidak sesuai jurusannya
- Indonesia membutuhkan **3 juta talenta digital** pada 2030
- Hanya **44,8%** tenaga kerja Indonesia bekerja sesuai bidang kompetensinya

---

## Fitur Utama

| Fitur | Deskripsi |
|-------|-----------|
| **Smart Skill Matching** | Siamese Neural Network menghitung similarity score antara skill user dan requirement lowongan |
| **CV Skill Extraction** | Upload PDF CV, sistem deteksi skill otomatis via OCR (PyMuPDF + Tesseract) + fuzzy matching |
| **Skill Normalization** | Menstandarkan format penulisan skill (react.js = ReactJS = React) menggunakan RapidFuzz |
| **Skill Gap Analysis** | Menunjukkan skill yang kurang + referensi roadmap belajar dari roadmap.sh |

---

## Arsitektur Sistem

```
[User/Browser]
    -> Frontend (React + TypeScript + Vite)
        -> Backend (Node.js / JavaScript)
            -> AI Service (Python / FastAPI)
                |-- Siamese Neural Network (TensorFlow)
                |-- OCR Engine (PyMuPDF + Tesseract)
                |-- Skill Normalizer (RapidFuzz)
                |-- Dataset: 201 lowongan magang tech Pulau Jawa
```

**Deployment:** Docker + Cloudflare Tunnel | **Live:** `https://api.magangin.online/docs`

---

## Performa Model

| Metric | Nilai |
|--------|-------|
| Accuracy | **97.38%** |
| Precision | 97.88% |
| Recall | 96.85% |
| F1-Score | 97.36% |

**Scoring System:**
```
final_score = 0.4 x model_score + 0.6 x coverage_score

Strong Match  : coverage >= 60%
Partial Match : coverage 30-59%
Low Match     : coverage < 30%
```

**A/B Testing:** Top 5 skill menghasilkan +15.94% final score dan +54.86% coverage score dibanding Top 3 skill (p < 0.05).

---

## Data Pipeline

```
Scraping (Playwright)              Dataset Global (Kaggle)
  |-- Glints                              |
  |-- Kalibrr                             v
  |-- JobStreet                    Cleaning + Filter Tech
        |                                 |
        v                                 |
  Preprocessing                           |
  |-- Skill extraction (SKILLS_DB)        |
  |-- Role mapping (roadmap.sh)           |
  |-- Location detection                  |
        |                                 |
        v                                 v
  Dataset Indo (201 lowongan)      Comparative Analysis
  80 skill vocabulary              Indo vs Global skill demand
```

**Dataset Indo:** 201 lowongan magang tech di Pulau Jawa | **Skill Vocabulary:** 80 tech skills | **Training Pairs:** 2864 (balanced)

---

## Struktur Repository

| Repository | Deskripsi | Tech |
|------------|-----------|------|
| [`ai-magang-in`](https://github.com/Magang-In/ai-magang-in) | Siamese Neural Network + FastAPI Service | Python, TensorFlow, FastAPI |
| [`ds-magang-in`](https://github.com/Magang-In/ds-magang-in) | Scraping, preprocessing, EDA, comparative analysis | Python, Jupyter, Playwright |
| [`backend-magang-in`](https://github.com/Magang-In/backend-magang-in) | Backend utama aplikasi | JavaScript, Node.js |
| [`frontend-magang-in`](https://github.com/Magang-In/frontend-magang-in) | Frontend web application | TypeScript, React, Vite |

---

## API Endpoints

| Method | Endpoint | Fungsi |
|--------|----------|--------|
| GET | `/api/health` | Health check & system status |
| GET | `/api/skills` | Daftar 80 skill yang dikenali |
| POST | `/api/match` | Ranking lowongan berdasarkan skill user |
| POST | `/api/predict` | Prediksi kecocokan user vs 1 job |
| POST | `/api/extract-cv` | Ekstrak skill dari CV (PDF) |
| POST | `/api/normalize` | Normalisasi format skill |

---

## Quick Start

```bash
# AI Service (Docker)
git clone https://github.com/Magang-In/ai-magang-in.git
cd ai-magang-in && docker compose up --build -d
# -> http://localhost:8001/docs

# Frontend
git clone https://github.com/Magang-In/frontend-magang-in.git
cd frontend-magang-in && npm install && npm run dev

# Backend
git clone https://github.com/Magang-In/backend-magang-in.git
cd backend-magang-in && npm install && npm run dev
```

---

## Tim

**ID Tim Capstone:** CC26-PSU387

| Nama | Role | Tanggung Jawab |
|------|------|----------------|
| TBD | AI Engineer | Siamese Neural Network, skill matching, API service |
| TBD | Data Scientist | Scraping, preprocessing, EDA, skill taxonomy |
| TBD | Full Stack Developer | Backend, frontend, deployment |

---

## Referensi

- ManpowerGroup Talent Shortage Survey
- Media Indonesia — Kekurangan talenta digital Indonesia
- JurnalPost — Qualification Mismatch di Indonesia
- ILO — Skills Mismatch Study
- Kaggle — Job Descriptions 2025: Tech & Non-Tech Roles

---

## Lisensi

[MIT License](LICENSE) | Coding Camp DBS Foundation 2026
