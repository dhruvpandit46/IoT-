# 💡 Remote ESP32 LED Control

![HTML](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-ES6-yellow?style=for-the-badge&logo=javascript)
![Firebase](https://img.shields.io/badge/Firebase-Realtime%20DB-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)
![ESP32](https://img.shields.io/badge/Hardware-ESP32-E7352C?style=for-the-badge&logo=espressif&logoColor=white)
![Status](https://img.shields.io/badge/Status-Completed-success?style=for-the-badge)

**Remote ESP32 LED Control** is a minimal web remote for toggling an LED wired to an ESP32. Two buttons write `"ON"` or `"OFF"` directly to a **Firebase Realtime Database** path via its REST API — the ESP32 listens on that same path and flips the physical LED accordingly.

---

# 📑 Table of Contents

- Features
- Live Demo
- Technologies
- Project Structure
- How It Works
- ESP32 Setup
- Configuration
- Installation
- Security Notes
- Future Improvements
- Contributing
- License
- Author

---

# ✨ Features

✅ One-Click LED Control — simple green **Turn ON** / red **Turn OFF** buttons

✅ ☁️ Firebase Realtime Database Sync — writes state via a plain REST `PUT` request, no SDK required

✅ 📡 Hardware-Ready — designed to pair directly with an ESP32 listening on the same database path

✅ 🪶 Minimal Footprint — a single HTML file with inline styles and script, nothing else to load

✅ Zero Backend Code — Firebase Realtime Database handles all sync logic between web and device

✅ Live Status Feedback — the page displays the raw response after each button press

---

# 🚀 Live Demo

https://dhruvpandit46.github.io/IoT-/

---

# ⚙ Technologies Used

- HTML5 (inline CSS, no external stylesheet)
- JavaScript (Vanilla, ES6, `fetch` API)
- Firebase Realtime Database — REST API (no SDK)
- ESP32 microcontroller (paired hardware, not included in this repo)

---

# 📂 Project Structure

```
IoT-/
│
├── index.html
└── README.md
```

The entire web remote — markup, styling, and logic — lives in a single `index.html` file.

---

# ⚡ How It Works

1. Clicking **Turn ON** or **Turn OFF** calls `setLed(state)`, which sends an HTTP `PUT` request to:
   ```
   {FIREBASE_URL}/ledStatus.json
   ```
   with the body `"ON"` or `"OFF"`.
2. Firebase's REST API accepts the `PUT` and overwrites the `ledStatus` value in the Realtime Database.
3. An ESP32 sketch (see below), listening on that same `/ledStatus` path, receives the update and switches its connected LED accordingly.
4. The page displays Firebase's raw response text so you can confirm the write succeeded.

---

# 🔧 ESP32 Setup

This repo contains only the **web remote**. To complete the system, your ESP32 needs firmware that:

1. Connects to Wi-Fi.
2. Either **polls** `GET {FIREBASE_URL}/ledStatus.json` on an interval, or uses Firebase's **streaming** REST endpoint to receive updates in real time.
3. Parses the returned value (`"ON"` / `"OFF"`) and sets the LED pin `HIGH` or `LOW` accordingly.

A typical Arduino-style flow (using `HTTPClient` and `ArduinoJson`) would fetch the JSON value, strip the surrounding quotes, and drive `digitalWrite(LED_PIN, value == "ON" ? HIGH : LOW)`.

---

# 🔧 Configuration

The database URL is set at the top of the inline script in `index.html`:

```js
const FIREBASE_URL = "https://your-project-default-rtdb.firebaseio.com";
```

Replace it with your own Firebase Realtime Database URL, and make sure your ESP32 firmware points at the **same** URL and the **same** `ledStatus` path.

---

# 📦 Installation

Clone the repository

```bash
git clone https://github.com/dhruvpandit46/IoT-.git
```

Go inside the project

```bash
cd IoT-
```

Run

Simply open `index.html` in your browser — no build step, no dependencies. Make sure `FIREBASE_URL` points at a real, reachable Firebase Realtime Database.

---

# 🔒 Security Notes

- This page writes to your database using a **plain, unauthenticated REST `PUT` request** — there is no Firebase Authentication or API key involved in the write itself.
- **Your Realtime Database security rules are the only thing standing between this URL and anyone on the internet.** If `ledStatus` (or your whole database) is left on open rules (`.read: true, .write: true`), anyone who finds this repo or your `FIREBASE_URL` can flip your LED — or write arbitrary data — with zero authentication.
- Before deploying this publicly, lock down your rules to require authentication (even simple anonymous auth) for writes, and scope them narrowly to the `ledStatus` path rather than the whole database.

---

# 🎯 Future Improvements

- Firebase Authentication before allowing writes
- Live LED state read-back (reflect actual hardware status, not just last button press)
- Support for multiple devices/LEDs from one dashboard
- Scheduling / timer-based automation
- Connection-lost indicator if the ESP32 hasn't checked in recently
- Nicer UI (toggle switch instead of two buttons)

---

# 🤝 Contributing

Contributions are welcome.

1. Fork the repository
2. Create your feature branch
3. Commit your changes
4. Push your branch
5. Open a Pull Request

---

# 📜 License

Licensed under the **MIT License**.

MIT © 2026 Dhruv Pandit.

See the [LICENSE](LICENSE) file for full license details.

---

# 👨‍💻 Author

**Dhruv Pandit**

GitHub — https://github.com/dhruvpandit46

LinkedIn — https://linkedin.com/in/dhruv-pandit-755786326

Instagram — https://instagram.com/dhruv_pandit2007

---

# ⭐ Support

If you found this project useful, please consider giving it a ⭐ on GitHub.
It helps support future development.
