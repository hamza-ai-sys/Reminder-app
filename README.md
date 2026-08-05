# 🔔 AI Voice Reminder App

An intelligent multilingual reminder application that accepts **voice and text** inputs, uses AI to extract time and task automatically, syncs with Google Calendar, and sends real-time notifications via Discord.

🌐 **Live Demo:** [https://hamza-ai-sys.github.io/Reminder-app/](https://hamza-ai-sys.github.io/Reminder-app/)

---

## ✨ Features

- 🎤 **Voice Input** — Speak your reminders naturally
- ⌨️ **Text Input** — Type reminders in any format
- 🌍 **Multilingual Support** — English, Urdu, Roman Urdu, and mixed languages
- 🤖 **AI Time Extraction** — Powered by Llama 3.3 (70B) via Groq
- 📅 **Google Calendar Sync** — Automatic event creation
- 💬 **Discord Notifications** — Real-time alerts at exact time
- 🌙 **Quiet Hours** — Silent mode configuration
- 📊 **Interactive Dashboard** — KPI tracking, history, weekly analytics
- 🔔 **Browser Push Notifications** — OS-level alerts
- ⏰ **Overdue Detection** — Smart status tracking with grace period
- 📝 **Reminder History** — View all completed and missed reminders
- 🎯 **Status Management** — Automatic pending/completed lifecycle

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| **Frontend** | HTML5, Tailwind CSS, Vanilla JavaScript |
| **Backend** | n8n Cloud (Workflow Automation) |
| **Database** | Aiven MySQL |
| **AI Model** | Groq (Llama 3.3 70B Versatile) |
| **Speech to Text** | Groq Whisper Large V3 Turbo |
| **Calendar** | Google Calendar API |
| **Notifications** | Discord Webhooks |
| **Hosting** | GitHub Pages |

---

## 🏗️ System Architecture

User speaks or types reminder in browser → GitHub Pages frontend sends to n8n webhook → AI Agent (Groq) extracts task and time → Google Calendar event created → MySQL row inserted with pending status → Cron every minute checks due reminders → Discord notification fires at exact time → Status updated to completed.

---

## 🌍 Multilingual Examples

| User Input | Extracted Task | Extracted Time |
|------------|----------------|----------------|
| "Remind me to call mom at 5pm" | Call mom | 17:00 |
| "mujhe 3 baje dawai ka yaad dilana" | Take medicine | 15:00 |
| "in 10 minutes water plants" | Water plants | Current + 10 min |
| "subah 7 baje uthna" | Wake up | 07:00 |
| "eat at 12:30pm" | Eat | 12:30 |

---

## 🚀 How It Works

1. **User speaks or types** a reminder in the interface
2. Frontend sends request to **n8n Cloud webhook**
3. AI Agent (Groq Llama 3.3) extracts clean task description and target time in PKT timezone
4. Google Calendar event is created automatically
5. Row inserted in MySQL with status `pending`
6. Cron job runs every minute checking for due reminders
7. Discord notification fires at exact time
8. Status updates to `completed` in database
9. Dashboard reflects real-time changes

---

## 📸 Screenshots

### Main Dashboard
![Dashboard](screenshots/dashboard.png)

### Reminder Preview
![Preview](screenshots/preview.png)

### Discord Notification
![Discord](screenshots/discord.png)

---

## 🔧 Setup Instructions

### Prerequisites

- n8n Cloud account (or self-hosted n8n)
- MySQL database (Aiven / PlanetScale / Local)
- Google Calendar API credentials
- Discord webhook URL
- Groq API key

### Step 1: Clone Repository

git clone https://github.com/hamza-ai-sys/Reminder-app.git
cd Reminder-app

### Step 2: Setup Database

Run this SQL in your MySQL:

CREATE TABLE calendar_reminders (
  id VARCHAR(255) PRIMARY KEY,
  title VARCHAR(255),
  reminder_text TEXT,
  execution_time DATETIME,
  status VARCHAR(50) DEFAULT 'pending',
  created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE INDEX idx_status_time ON calendar_reminders (status, execution_time);

### Step 3: Import n8n Workflows

Import the three workflow files from `/workflows` folder into your n8n instance:

- `workflow-1-creation.json` — Voice/Text processing
- `workflow-2-executor.json` — Reminder executor (cron based)
- `workflow-3-calendar-sync.json` — Google Calendar sync

### Step 4: Configure Credentials

In n8n, create these credentials:

- **MySQL** — Your database connection
- **Google Calendar OAuth2** — Google Calendar access
- **Discord Webhook** — Notification channel
- **Groq API** — AI model access
- **HTTP Header Auth (Groq Whisper)** — Speech to text API

### Step 5: Activate Workflows

Toggle all 3 workflows to **Active** in n8n.

### Step 6: Update Frontend

Open `index.html` and update the WEBHOOK_ENDPOINT constant with your n8n Cloud production webhook URL.

### Step 7: Deploy

Open `index.html` in browser OR deploy to GitHub Pages, Netlify, Vercel, etc.

---

## 📝 Database Schema

Table: calendar_reminders

- id — VARCHAR(255) PRIMARY KEY
- title — VARCHAR(255)
- reminder_text — TEXT
- execution_time — DATETIME
- status — VARCHAR(50) DEFAULT 'pending'
- created_at — TIMESTAMP DEFAULT CURRENT_TIMESTAMP

**Status Values:**
- `pending` — Not yet fired
- `completed` — Notification sent successfully

---

## 🎯 Key Engineering Decisions

### Why Database Over Google Calendar Only?

- Faster queries (MySQL vs Google Calendar API)
- Reliable status tracking (SQL vs description parsing)
- Handles overdue reminders (never miss a notification)
- No API rate limits for reads
- Single source of truth

### Why Cron Polling Over Wait Nodes?

- Server restart safe (Wait nodes lose state)
- Handles long durations (10 days works same as 10 minutes)
- Scalable (thousands of reminders without memory issues)
- Recovery built in (missed reminders fire next cycle)

### Why Grace Period?

- Prevents false overdue on newly created reminders
- Handles clock skew between servers
- User friendly UX

---

## 🎯 Future Enhancements

- [ ] WhatsApp notifications integration
- [ ] Recurring reminders (daily/weekly/monthly)
- [ ] Multi-user authentication
- [ ] Email notification support
- [ ] Snooze functionality
- [ ] Custom notification sounds
- [ ] Mobile app (React Native)
- [ ] Reminder categories and tags
- [ ] Voice output confirmation
- [ ] SMS notifications (Twilio)

---

## 🐛 Known Limitations

- Google Calendar API polling may have 1-2 minute delay
- Voice recording requires HTTPS in production
- Free tier limits on Groq/Aiven may apply
- Timezone hardcoded to Asia/Karachi (customizable in workflow)

---

## 🤝 Contributing

Contributions are welcome! Here's how:

1. Fork the repository
2. Create your feature branch (git checkout -b feature/AmazingFeature)
3. Commit your changes (git commit -m 'Add some AmazingFeature')
4. Push to the branch (git push origin feature/AmazingFeature)
5. Open a Pull Request

---

## 📄 License

This project is licensed under the MIT License — feel free to use it for learning or personal projects.

---

## 👨‍💻 Author

**Hamza**

- GitHub: [@hamza-ai-sys](https://github.com/hamza-ai-sys)
- LinkedIn: [Connect with me](https://www.linkedin.com/in/muhammad-hamza-talib-8100a6421/)

---

## 🙏 Acknowledgments

- [n8n](https://n8n.io/) — Amazing workflow automation platform
- [Groq](https://groq.com/) — Lightning fast AI inference
- [Aiven](https://aiven.io/) — Reliable managed databases
- [Tailwind CSS](https://tailwindcss.com/) — Beautiful styling
- [Google Calendar API](https://developers.google.com/calendar) — Calendar integration

---

## ⭐ Show Your Support

If you found this project helpful, please give it a **star** ⭐ on GitHub!

It motivates me to build more open source projects.

---

## 📞 Contact

Have questions or suggestions? Feel free to:

- Open an issue on GitHub
- Reach out on LinkedIn

---

**Built with ❤️ by Hamza**
