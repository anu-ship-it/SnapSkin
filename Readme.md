# SnapSkin 📸🔬

> AI-powered skin condition analyzer. Snap a photo, get instant insights.

---

## ⚠️ Disclaimer

**SnapSkin is not a medical diagnosis tool.** It is built for informational purposes only. Always consult a certified dermatologist or medical professional for proper diagnosis and treatment. Never make health decisions based solely on this app.

---

## What is SnapSkin?

SnapSkin lets users take or upload a photo of a skin condition and receive instant AI-powered analysis. Built with React Native (Expo) on the frontend and Node.js + Express on the backend, powered by Google Gemini Vision API.

---

## Features

- 📷 Camera capture or gallery upload
- 🤖 AI-powered skin condition analysis via Gemini Vision
- 📊 Confidence score with top 3 possible conditions
- ℹ️ Condition info — causes, symptoms, home care, when to see a doctor
- 🛡️ Rate limiting to prevent API abuse
- 🗜️ Auto image compression before upload
- ⚠️ Graceful error handling — app never silently breaks
- 📡 Sentry integration for production crash monitoring

---

## Tech Stack

| Layer | Technology |
|---|---|
| Mobile App | React Native (Expo) |
| Backend | Node.js + Express |
| AI Layer | Google Gemini Vision API |
| Image Processing | Sharp |
| Rate Limiting | express-rate-limit |
| Caching | node-cache |
| Error Monitoring | Sentry |
| Hosting | Render |

---

## Project Structure

```
SnapSkin/
├── client/                  # React Native (Expo) App
│   ├── app/
│   │   ├── index.jsx        # Home screen - camera/upload
│   │   ├── result.jsx       # Result screen - condition info
│   │   └── _layout.jsx      # Navigation layout
│   ├── components/
│   │   ├── CameraCapture.jsx
│   │   ├── ResultCard.jsx
│   │   └── Loader.jsx
│   ├── services/
│   │   └── api.js           # Backend API calls
│   └── app.json
│
├── server/                  # Node.js + Express Backend
│   ├── index.js             # Entry point
│   ├── routes/
│   │   └── analyze.js       # /analyze route
│   ├── middleware/
│   │   ├── rateLimiter.js
│   │   ├── imageValidator.js
│   │   └── errorHandler.js
│   ├── services/
│   │   ├── gemini.js        # Gemini Vision API calls
│   │   └── cache.js         # Response caching
│   └── .env.example
│
└── README.md
```

---

## Getting Started

### Prerequisites

- Node.js v18+
- Expo CLI
- Google Gemini API Key — [Get one here](https://makersuite.google.com/app/apikey)
- Sentry account (optional, for monitoring)

---

### 1. Clone the Repo

```bash
git clone https://github.com/anu-ship-it/SnapSkin.git
cd SnapSkin
```

---

### 2. Setup Backend

```bash
cd server
npm install
```

Create your `.env` file:

```env
PORT=5000
GEMINI_API_KEY=your_gemini_api_key_here
SENTRY_DSN=your_sentry_dsn_here   # optional
```

Start the server:

```bash
node index.js
```

---

### 3. Setup Mobile App

```bash
cd client
npm install
npx expo start
```

Scan the QR code with the **Expo Go** app on your phone.

Update `services/api.js` with your backend URL:

```javascript
const BASE_URL = 'http://your-local-ip:5000'; // development
// const BASE_URL = 'https://snapskin-api.onrender.com'; // production
```

---

## API Reference

### POST `/analyze`

Analyzes a skin condition from an uploaded image.

**Request:**
```
Content-Type: multipart/form-data
Body: image (file)
```

**Response:**
```json
{
  "success": true,
  "results": [
    {
      "condition": "Eczema",
      "confidence": 78,
      "description": "A chronic inflammatory skin condition...",
      "causes": ["dry skin", "allergens", "stress"],
      "contagious": false,
      "homeCare": ["moisturize regularly", "avoid harsh soaps"],
      "seeDoctor": "If rash spreads or doesn't improve in 2 weeks"
    }
  ],
  "disclaimer": "This is not a medical diagnosis. Please consult a doctor."
}
```

**Error Response (low confidence):**
```json
{
  "success": false,
  "message": "Unable to identify condition clearly. Please consult a doctor.",
  "confidence": 42
}
```

---

## Production Considerations

### Rate Limiting
Each IP is limited to **30 requests per hour** to prevent API abuse and control costs.

### Image Compression
All uploaded images are compressed to a maximum of **2MB** using Sharp before being sent to Gemini API.

### Confidence Threshold
If the AI confidence score is **below 60%**, the app will not display a condition guess. It will instead prompt the user to consult a doctor.

### Error Handling
Every API call is wrapped in try/catch. Users always receive a meaningful message — never a blank screen.

### Caching
Repeated identical image analyses are cached for **1 hour** to reduce unnecessary API calls.

---

## Deployment

### Backend (Render)

1. Push `server/` to GitHub
2. Create a new Web Service on [Render](https://render.com)
3. Add environment variables in Render dashboard
4. Deploy

### Mobile App (Expo)

For production build:
```bash
npx expo build:android
npx expo build:ios
```

Or use **EAS Build** (recommended):
```bash
npm install -g eas-cli
eas build --platform android
```

---

## Roadmap

- [ ] v1 — Core analysis with Gemini Vision API
- [ ] v1.1 — Save analysis history locally
- [ ] v1.2 — Find nearby dermatologists on map
- [ ] v2 — Custom trained model replacing Gemini API
- [ ] v2.1 — Support for 50+ skin conditions
- [ ] v2.2 — Multi-language support

---

## Known Limitations

- AI model may perform with **lower accuracy on darker skin tones** due to dataset bias in general vision models. This is an active area of improvement.
- Analysis accuracy depends heavily on **photo quality** — good lighting and a clear, close-up shot gives better results.
- Currently supports **common conditions only** — rare or complex conditions may not be identified correctly.

---

## Contributing

Pull requests are welcome. For major changes, open an issue first to discuss what you'd like to change.

1. Fork the repo
2. Create your feature branch — `git checkout -b feature/your-feature`
3. Commit your changes — `git commit -m 'Add your feature'`
4. Push to branch — `git push origin feature/your-feature`
5. Open a Pull Request

---

## License

MIT License — see [LICENSE](LICENSE) for details.

---

## Author

Built by **[Anoop Kumar]**
- GitHub: [@anu-ship-it](https://github.com/anu-ship-it/SnapSkin)

---

> SnapSkin — Snap it. Know it. Treat it.