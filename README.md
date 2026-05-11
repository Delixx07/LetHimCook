# 🔥 LetHimCook — Recipe Recommender

**LetHimCook** adalah aplikasi mobile berbasis Flutter yang dirancang sebagai asisten masak cerdas. Aplikasi ini dapat mencari resep secara cerdas dari bahan yang kamu miliki, mengklasifikasikan tingkat kesehatan resep menggunakan *Machine Learning* secara *on-device*, dan menyimpan resep favoritmu ke dalam database lokal t!

---

## Fitur Utama

| Fitur | Deskripsi |
|---|---|
|**On-Device AI Classification** | Menggunakan **TensorFlow Lite (TFLite)** secara *offline* untuk memprediksi apakah suatu resep termasuk kategori "Sehat" atau "Kurang Sehat" berdasarkan komposisinya. |
|**Smart Recipe Matching** | Mencari resep dari database lokal menggunakan algoritma kecocokan ketat (*strict match*) & toleransi *typo* (`string_similarity`). |
|**Favorite & Local Database** | Menyimpan resep masakan favorit ke dalam penyimpanan lokal secara permanen menggunakan **SQLite** (`sqflite`). |
|**Pilih Bahan dengan Checkbox** | Tambahkan bahan ke daftar, lalu centang bahan mana saja yang ingin digunakan. AI menolak resep jika bahan kurang terlalu banyak. |
|**UI Modern & Gelap** | Tampilan *dark theme* yang elegan dengan palet warna kustom. |

---

## Alur Aplikasi

```text
InputScreen → ResultScreen → DetailScreen
                 ↳ FavoriteScreen
```

1. **InputScreen** — Pengguna mengetik dan menambahkan bahan makanan ke daftar. AI akan memilah kombinasi resep terbaik berdasarkan bahan yang dicentang.
2. **ResultScreen** — Menampilkan daftar rekomendasi resep yang relevan berdasarkan tingkat kecocokan bahan (*Missing Ingredients Filter*).
3. **DetailScreen** — Menampilkan detail resep. Di sini **TFLite** akan bekerja langsung menganalisis resep, dan pengguna bisa menekan tombol 💖 untuk menyimpannya.
4. **FavoriteScreen** — Layar khusus yang memuat semua resep favorit pengguna yang ditarik secara dinamis dari database SQLite.

---

## Cara Kerja AI

Aplikasi ini menggunakan 2 jenis kecerdasan buatan (*Artificial Intelligence*) yang berjalan 100% secara lokal di perangkat:

### 1. Smart Recipe Matching (Fuzzy Logic & Scoring)
Saat pengguna memasukkan daftar bahan, algoritma akan melakukan pencarian cerdas ke dalam database *offline* (`recipes.json`):
*   **Typo Tolerance:** Menggunakan package `string_similarity` (algoritma *Levenshtein Distance*). Jika pengguna mengetik "aym", AI tahu bahwa tingkat kemiripannya dengan "ayam" > 85% dan akan menganggapnya benar.
*   **Scoring & Filtering:** AI menghitung jumlah bahan yang cocok (*matched*) dan bahan yang kurang (*missing*). AI menggunakan **Strict Mode**, di mana resep akan otomatis didiskualifikasi jika membutuhkan lebih dari 2 bahan tambahan yang tidak dimiliki pengguna.

### 2. Klasifikasi Kesehatan (TensorFlow Lite)
Saat membuka detail resep, aplikasi menggunakan model Machine Learning yang telah dilatih secara khusus (*Custom Keras Model*) untuk menilai kesehatan makanan:
*   **Ekstraksi Fitur:** `TfliteService` membaca daftar bahan resep dan mengubahnya menjadi *Tensor* biner (kumpulan angka 0 dan 1).
*   **Inference Lokal:** Tensor dimasukkan ke dalam model `recipe_classifier.tflite` yang berjalan di CPU HP (*offline*).
*   **Output:** Model memproses data menggunakan fungsi aktivasi *Sigmoid* untuk menghasilkan skor probabilitas, kemudian melabeli makanan tersebut dengan **"🌱 Sehat"** atau **"⚠️ Kurang Sehat (Tinggi Kalori)"**.

---

##  Struktur Proyek Inti

```text
lib/
├── models/
│   └── recipe.dart              # Model data resep masakan
├── screens/
│   ├── input_screen.dart        # Layar utama (input & pemilihan bahan)
│   ├── result_screen.dart       # Layar hasil rekomendasi AI
│   ├── detail_screen.dart       # Layar instruksi masak & TFLite scanner
│   └── favorite_screen.dart     # Layar daftar resep tersimpan
└── services/
    ├── ai_service.dart          # Logika fuzzy matching & scoring
    ├── tflite_service.dart      # Handler untuk model TensorFlow Lite
    └── db_service.dart          # Handler SQLite untuk fitur favorit
assets/
├── recipes.json                 # Database offline berisi 40+ resep
└── recipe_classifier.tflite     # Model klasifikasi AI pra-latih
```

---

## Cara Menjalankan

### Prasyarat
- Flutter SDK (≥ 3.9.2)
- Karena aplikasi ini memuat *package* Native (C++/NDK) seperti `tflite_flutter` dan `sqflite`

### Langkah-langkah

1. **Clone repository**
   ```bash
   git clone https://github.com/Delixx07/LetHimCook.git
   cd LetHimCook
   ```
2. **Install dependencies**
   ```bash
   flutter pub get
   ```
3. **Jalankan aplikasi**
   ```bash
   flutter run
   ```

---

##  Dokumentasi
*(Catatan: Tangkapan layar di bawah mungkin merupakan versi UI sebelumnya)*

### Input Screen
<img width="483" height="1067" alt="image" src="https://github.com/user-attachments/assets/4c387f44-9eb2-4d16-9d78-94d6d3aa6521" />

### Result Screen
<img width="480" height="1061" alt="image" src="https://github.com/user-attachments/assets/2f89dcc2-c715-411f-89ef-933e81a6d0ab" />


### Detail Screen
<img width="495" height="1070" alt="image" src="https://github.com/user-attachments/assets/ca711dbe-c300-43ba-a517-61651edb593e" />
<img width="496" height="1063" alt="image" src="https://github.com/user-attachments/assets/b75b59d8-8159-4a32-83f3-09b47b32f4c5" />

### Favorite Screen
<img width="488" height="1056" alt="image" src="https://github.com/user-attachments/assets/1b793472-6251-449a-ba30-114703a50239" />


