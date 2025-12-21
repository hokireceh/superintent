# SuperIntent Telegram Bot

Telegram bot for SuperIntent AI platform dengan fitur automation untuk daily check-in dan quest claiming.

## 📋 Fitur Utama

- **📊 My Stats** - Lihat poin, streak, dan referral code kamu
- **🎯 Quests** - Lihat daftar quest yang tersedia
- **✅ Check In** - Daily check-in untuk bonus poin
- **📤 Referral** - Dapatkan dan share referral code
- **⚙️ Settings** - Admin tools (broadcast, view users)

## 🤖 Automation Features

- **Auto Daily Check-in** - Bot otomatis claim check-in setiap hari (jika belum dilakukan)
- **Auto Quest Claim** - Bot cerdas menganalisis quest dan skip yang membutuhkan aksi manual (social actions, referrals)
- **Scheduler** - Berjalan otomatis setiap 1 jam (atau manual via menu)

## 🚀 Cara Menggunakan

1. Tambahkan bot ke Telegram dan klik `/start`
2. Pilih menu untuk mengakses fitur
3. Bot akan otomatis berjalan setiap jam untuk check-in dan claim quest

## ⚙️ Setup & Konfigurasi

### Environment Variables

```env
TELEGRAM_BOT_TOKEN=your_bot_token_here
SUPERINTENT_API_URL=https://bff-root.superintent.ai
SUPERINTENT_AUTH_COOKIE=your_auth_cookie
DATABASE_URL=your_postgres_url
ADMIN_IDS=your_telegram_id (comma-separated)
DEBUG=false
```

### Persyaratan

- Node.js
- PostgreSQL
- Telegram Bot Token (dari @BotFather)
- SuperIntent Auth Cookie

## 📦 Dependencies

- `node-telegram-bot-api` - Telegram Bot API wrapper
- `pg` - PostgreSQL client
- `node-fetch` - HTTP client
- `dotenv` - Environment management

## 🔗 Links

- **GitHub** - https://github.com/hokireceh/superintent
- **Telegram Channel** - https://t.me/garapanairdrop_indonesia
- **Website** - https://mission.superintent.ai/?referralCode=8NFpzrvTzS

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

Open source for SuperIntent community

## 👨‍💻 Support

Pertanyaan atau feedback? Hubungi kami di Telegram channel: https://t.me/garapanairdrop_indonesia
