# SuperIntent Telegram Bot

Telegram bot untuk SuperIntent AI platform dengan fitur automation untuk daily check-in, quest claiming, dan broadcast admin.

## 📋 Fitur Utama

### User Features
- **📊 My Stats** - Lihat poin, streak, referral code, dan progress kamu
- **🎯 Quests** - Lihat daftar quest yang tersedia dengan kategori (Social, Referral, General)
- **✅ Check In** - Daily check-in untuk bonus poin dan streak
- **📤 Referral** - Dapatkan dan share referral code kamu untuk earn bonus points

### Admin Features
- **📢 Broadcast** - Kirim pesan/media ke semua users dengan preview dan konfirmasi
- **👥 View Users** - Lihat total users dan daftar user IDs
- **⚙️ Automation Control** - Run manual automation task atau check automation status
- **📊 Status Monitor** - Monitor daily check-in status dan poin terkini

## 🤖 Automation Features

- **Auto Daily Check-in** - Bot otomatis claim check-in setiap hari (jika belum dilakukan)
- **Auto Quest Claim** - Bot cerdas menganalisis quest dan skip yang membutuhkan aksi manual
- **Smart Scheduler** - Berjalan otomatis setiap 1 jam atau manual via menu
- **Exponential Backoff Retry** - Automatic retry dengan exponential backoff (2s, 4s, 8s) untuk handle network issues
- **Connection Stability** - IPv4-only forcing, keep-alive HTTPS agents, dan timeout handling

## 🚀 Cara Menggunakan

### Untuk Users
1. Tambahkan bot ke Telegram dan klik `/start`
2. Pilih menu untuk mengakses fitur
3. Bot akan otomatis berjalan setiap jam untuk check-in dan claim quest

### Untuk Admin
1. Set ADMIN_IDS di environment variables dengan Telegram ID kamu
2. Akses **Settings** menu untuk admin features
3. Gunakan **Broadcast** untuk mengirim pesan ke semua users
4. Monitor automation status dan jalankan manual tasks sesuai kebutuhan

## ⚙️ Setup & Konfigurasi

### Environment Variables

```env
TELEGRAM_BOT_TOKEN=your_bot_token_here
SUPERINTENT_API_URL=https://bff-root.superintent.ai
SUPERINTENT_AUTH_COOKIE=your_auth_cookie
DATABASE_URL=your_postgres_url
ADMIN_IDS=your_telegram_id (comma-separated untuk multiple admins)
DEBUG=false
```

### Persyaratan

- **Node.js** (v14+)
- **PostgreSQL** (untuk store user data)
- **Telegram Bot Token** (dari @BotFather)
- **SuperIntent Auth Cookie** (dari mission.superintent.ai)

### Installation & Running

```bash
# Install dependencies
npm install

# Set environment variables
# Create .env file dengan config di atas

# Run bot
npm start
```

## 📦 Dependencies

- **telegraf** - Modern Telegram Bot API framework dengan error handling lebih baik
- **pg** - PostgreSQL client untuk store user data
- **node-fetch** - HTTP client untuk SuperIntent API
- **dotenv** - Environment variables management

## 🔐 Security & Stability

### Connection Stability
- **IPv4-only forcing** - Menghindari IPv6 timeout issues
- **Keep-alive agents** - Persistent HTTPS connections
- **Exponential backoff retry** - Automatic retry dengan smart delays untuk handle network failures
- **Request timeout** - 30 second timeout untuk prevent hanging requests

### Data Safety
- User data disimpan di PostgreSQL dengan proper indexing
- Admin operations require authentication via ADMIN_IDS
- Broadcast operations show preview dan require confirmation sebelum kirim

## 📊 Database Schema

Bot menggunakan PostgreSQL dengan tabel:
- **users** - Store user data (id, telegram_id, username, stats)
- **user_quests** - Track completed quests
- **referrals** - Track referral relationships

## 🔗 Links

- **Website** - https://mission.superintent.ai/?referralCode=8NFpzrvTzS
- **Telegram Channel** - https://t.me/garapanairdrop_indonesia

## 💡 Tips & Tricks

- Bot secara otomatis skip quests yang require social actions (follow, like, etc)
- Daily check-in runs at top of every hour
- Broadcast dengan media (foto/video) akan keep formatting original
- Admin dapat view semua users dan tracking broadcast success rate

## ❌ Troubleshooting

**Bot tidak merespons?**
- Pastikan TELEGRAM_BOT_TOKEN valid
- Check database connection (DATABASE_URL)
- Lihat console logs untuk error details

**Automation tidak berjalan?**
- Check automation scheduler active via Settings menu
- Pastikan SUPERINTENT_AUTH_COOKIE masih valid
- Run manual automation task via Settings untuk debug

**Broadcast error?**
- Ensure semua target users sudah add bot (tidak block)
- Check media file size (Telegram has size limits)
- Admin dapat lihat failed users di broadcast report

## 💰 Support & Donasi

Jika bot ini membantu kamu, silakan support kami:

**EVM Wallet:**
```
0xaFf68fFd9B57720018ea1e34b7B37637C022FACe
```

**TON Wallet:**
```
UQAN1eZ0Myj6JZ5nKREDKJjPJiZgRivPFwHpS19vuCa5CXy2
```

## 📝 License

Open source untuk SuperIntent community

## 👨‍💻 Support & Feedback

Pertanyaan atau feedback? Hubungi kami di Telegram channel: https://t.me/garapanairdrop_indonesia

---

**Last Updated:** December 2024  
**Bot Status:** Production Ready ✅
