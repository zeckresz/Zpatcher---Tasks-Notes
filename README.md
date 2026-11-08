# Zpatcher - Asisten Pengingat Jadwal Pribadi Pintar (Android Native)

Aplikasi Android native modern berbasis **Kotlin** dan **Jetpack Compose** untuk pengingat jadwal pribadi berbasis AI offline-first. Aplikasi ini bertindak sebagai asisten pribadi 1-on-1 (mirip ChatGPT khusus jadwal) yang memahami bahasa Indonesia percakapan sehari-hari, mampu mengekstrak agenda dan waktu secara otomatis, meminta konfirmasi interaktif, serta memberikan notifikasi tepat waktu dengan aksi langsung.

---

## ✨ Fitur Utama

### 1. 💬 Chat AI Percakapan & Ekstraksi Jadwal Otomatis
- **Tampilan Mirip ChatGPT**: Chat bubble modern, avatar AI, avatar pengguna, status online AI, dan input text fleksibel.
- **Natural Indonesian Language Understanding (NLP)**:
  - Memahami istilah waktu lokal: *"besok"*, *"lusa"*, *"kemarin"*, *"nanti sore"*, *"nanti malam"*, *"hari ini"*, *"hari senin"*, *"tanggal 15"*, dll.
  - Memahami format jam: *"jam 9 pagi"*, *"jam 1 siang"*, *"jam 14:30"*, *"setengah 8 pagi"*, *"pukul 20.00"*.
  - **Multi-Ekstraksi dalam 1 Pesan**: Contoh: *"Besok jam 9 meeting Zoom sama client, siang jam 1 ke dokter"* langsung dideteksi menjadi **2 agenda terpisah**!
- **Konfirmasi Interaktif Sebelum Simpan**: AI menampilkan ringkasan kartu jadwal di dalam bubble chat dengan tombol **"✓ Simpan Jadwal"** dan **"Batal"**.
- **Kategori Otomatis**: Otomatis mendeteksi kategori Pekerjaan (*Work*), Kesehatan (*Health*), Belajar (*Study*), Keuangan (*Finance*), Pribadi (*Personal*), atau Lainnya.
- **Tanya Jawab Jadwal**: AI bisa menjawab pertanyaan seperti *"Besok ada apa aja?"* atau *"Jadwal hari ini apa?"*.

### 2. 📅 Manajemen Jadwal Fleksibel
- **Filter Waktu**: Tab cepat untuk melihat jadwal **Hari Ini**, **Besok**, **Minggu Ini**, dan **Semua**.
- **Status Jadwal**: **Akan Datang** (*Upcoming*), **Selesai** (*Completed*), dan **Dibatalkan** (*Cancelled*).
- **Aksi Cepat**: Centang selesai langsung dari kartu jadwal, edit, atau hapus.
- **Form Tambah/Edit Manual**: Tersedia form lengkap dengan DatePickerDialog, TimePickerDialog native, dan multi-select pengingat.

### 3. ⏱️ Sistem Reminder Multilevel & Custom
- Pengguna bisa memilih **lebih dari satu pengingat** untuk satu jadwal:
  - 📆 **H-1 hari** (24 jam sebelum)
  - ⏳ **3 jam sebelum**
  - ⏰ **1 jam sebelum**
  - ⏱️ **30 menit sebelum**
  - 🔔 **15 menit sebelum**
  - 🎯 **Tepat waktu** (*At event time*)
  - ⚙️ **Pengingat Default** dapat diatur di menu *Pengaturan*.

### 4. 🔔 Notifikasi Akurat (Exact Alarm) & Aksi Langsung
- Menggunakan `AlarmManager.setAlarmClock()` dan `setExactAndAllowWhileIdle()` untuk menjamin notifikasi berbunyi **tepat waktu** bahkan saat aplikasi ditutup atau perangkat dalam kondisi *Doze Mode*.
- **Tombol Aksi Interaktif**:
  - **"✓ Selesai"**: Menandai jadwal sebagai selesai di Room Database langsung dari bilah notifikasi.
  - **"⏱ Tunda 15 mnt"**: Menunda alarm 15 menit ke depan secara otomatis (*Snooze*).
  - **"Buka"**: Membuka aplikasi ke jadwal terkait.
- **Support Device Reboot**: Memiliki `BootCompletedReceiver` sehingga alarm otomatis didaftarkan ulang saat HP baru dinyalakan.
- **Heads-up / Urgent Reminder**: Pengingat prioritas tinggi dengan saluran notifikasi khusus dan getaran kuat.

### 5. 🎙️ Voice Support (STT & TTS Native)
- **Speech-to-Text (STT)**: Tombol mikrofon di chat untuk berbicara langsung menggunakan API `SpeechRecognizer` native Android (Bahasa Indonesia).
- **Text-to-Speech (TTS)**: AI dapat membalas chat dengan suara ramah berbahasa Indonesia menggunakan native `TextToSpeech`.
- **Pengaturan Suara**: Opsi on/off balasan suara, pengaturan nada suara (*pitch*), dan kecepatan bicara (*speech rate*).

### 6. 🎨 Material 3 Design & Dark Mode
- Palet warna modern dengan dukungan **Mode Gelap (Dark Mode)**, **Mode Terang (Light Mode)**, dan **Ikuti Sistem**.
- Onboarding 3 slide informatif saat pertama kali aplikasi dibuka.
- Personalisasi nama panggilan dan gaya komunikasi AI (*Ramah & Santai*, *Profesional*, *Singkat & Padat*).

### 7. 🔒 100% Offline-First (Room Database)
- Semua data jadwal dan percakapan disimpan secara lokal menggunakan **Room Database**.
- Tidak memerlukan koneksi internet untuk fungsi utama.
- Tersedia opsi memasukkan Gemini / Groq API Key di Pengaturan jika pengguna menginginkan kecerdasan LLM cloud di masa mendatang.

---

## 🏗️ Arsitektur & Teknologi

- **Bahasa**: Kotlin (100%)
- **UI Framework**: Jetpack Compose + Material 3 Design
- **Arsitektur**: MVVM (Model-View-ViewModel) + Repository Pattern + Clean Architecture
- **Dependency Injection**: Hilt (Dagger Hilt 2.51.1 + KSP)
- **Database Lokal**: AndroidX Room 2.6.1 + Coroutines Flow
- **Penyimpanan Preferensi**: AndroidX DataStore Preferences
- **Navigasi**: Jetpack Navigation Compose
- **Penjadwalan Alarm**: Android Native `AlarmManager` (Exact Alarm) + `BroadcastReceiver`
- **Audio & Suara**: Android Native `SpeechRecognizer` + `TextToSpeech`
- **JSON Serialization**: Google Gson

---

## 📁 Struktur Direktori Project

```
app/src/main/
├── AndroidManifest.xml
├── java/com/Zpatcher/ai/
│   ├── ZpatcherApp.kt                 # Application Class & Channel Init
│   ├── MainActivity.kt               # Single Activity & Theme Setup
│   ├── data/
│   │   ├── local/
│   │   │   ├── AppDatabase.kt        # Room DB & Dummy Data Seeding
│   │   │   ├── converter/            # TypeConverters (Gson List)
│   │   │   ├── dao/                  # ScheduleDao, ChatMessageDao
│   │   │   └── entity/               # ScheduleEntity, ChatMessageEntity
│   │   ├── preferences/              # UserPreferencesRepository (DataStore)
│   │   └── repository/               # ScheduleRepositoryImpl, ChatRepositoryImpl
│   ├── domain/
│   │   ├── ai/
│   │   │   ├── AiScheduleParser.kt   # Smart Indonesian Schedule NLP Parser
│   │   │   └── AiAssistantEngine.kt  # Local AI Assistant Engine & Response Formatter
│   │   ├── model/                    # Schedule, ProposedSchedule, ChatMessage, Enums
│   │   └── repository/               # ScheduleRepository & ChatRepository interfaces
│   ├── reminder/
│   │   ├── BootCompletedReceiver.kt  # Reschedule alarms on phone reboot
│   │   ├── NotificationActionReceiver.kt # Handles "Selesai" & "Tunda 15 mnt"
│   │   ├── NotificationHelper.kt     # NotificationCompat & Channels builder
│   │   ├── ScheduleAlarmManager.kt   # Exact AlarmManager scheduler
│   │   └── ScheduleAlarmReceiver.kt  # BroadcastReceiver when alarm fires
│   ├── voice/
│   │   └── VoiceHelper.kt            # Native STT & TTS Managers
│   ├── di/
│   │   └── AppModule.kt              # Hilt DI Module
│   └── ui/
│       ├── components/               # AppTopBar, AppBottomBar, ScheduleCard, ConfirmationCard, Chips
│       ├── navigation/               # Screen routes & NavGraph
│       ├── screens/
│       │   ├── chat/                 # ChatScreen & ChatViewModel
│       │   ├── schedule/             # ScheduleListScreen, ScheduleFormScreen, ScheduleViewModel
│       │   ├── settings/             # SettingsScreen & SettingsViewModel
│       │   └── onboarding/           # OnboardingScreen
│       └── theme/                    # Color, Theme, Type (Material 3)
└── res/                              # Drawables, mipmaps, values (strings, colors, themes)
```

---

## 🚀 Cara Menjalankan Project

### Melalui Android Studio:
1. Buka **Android Studio** (disarankan versi Hedgehog, Iguana, Jellyfish, Koala, atau Ladybug).
2. Pilih **File -> Open...** lalu arahkan ke folder project ini (`d:\Anti Gravity`).
3. Tunggu hingga proses **Gradle Sync** selesai secara otomatis.
4. Pilih target perangkat: **Emulator Android** (Android 8.0 Oreo / API 26 ke atas, disarankan Android 13/14 / API 33/34) atau **HP Android fisik**.
5. Klik tombol **Run** (ikon segitiga hijau ▶) atau tekan `Shift + F10`.

---

## 🧪 Contoh Kalimat Natural untuk Dicoba

Buka tab **Chat AI** dan coba kirimkan pesan-pesan berikut:

1. **Multi-Agenda sekaligus:**
   > *"Besok jam 9 meeting Zoom sama client, siang jam 1 ke dokter"*
   *(AI akan menampilkan 2 kartu konfirmasi jadwal sekaligus).*

2. **Jadwal Hari Ini:**
   > *"Nanti sore jam 16:30 jogging santai di GBK"*

3. **Format Hari & Tanggal:**
   > *"Lusa jam 10 pagi bimbingan skripsi"*
   > *"Hari senin jam 8:30 bayar tagihan listrik dan WiFi"*
   > *"Tanggal 15 November jam 14:00 seminar web"*

4. **Tanya Jadwal yang Ada:**
   > *"Besok ada jadwal apa aja?"*
   > *"Jadwal hari ini apa?"*

5. **Uji Coba Notifikasi Akurat:**
   Masuk ke tab **Pengaturan** -> klik tombol **"Uji Coba Notifikasi Akurat (5 Detik)"**, lalu kunci layar atau keluar ke home screen. Notifikasi akan berbunyi tepat dalam 5 detik dengan tombol **"Selesai"** dan **"Tunda 15 mnt"**!
