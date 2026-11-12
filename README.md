<div align="center">

# 📖 Quran KPI

**An offline Quran reading KPI tracker & incentive payroll calculator for Android and Web.**

<p align="center">
  <a href="https://github.com/sayedmdsafwan/QuranKPI/releases/tag/v0.0.1">
    <img alt="Latest Release" src="https://img.shields.io/badge/release-v0.0.1-15493C?style=flat-square&logo=github">
  </a>
  <img alt="Platform" src="https://img.shields.io/badge/platform-Android-3DDC84?style=flat-square&logo=android&logoColor=white">
  <img alt="min SDK" src="https://img.shields.io/badge/min%20SDK-29%20(Android%2010)-15493C?style=flat-square">
  <img alt="target SDK" src="https://img.shields.io/badge/target%20SDK-34-15493C?style=flat-square">
  <img alt="Zero Permissions" src="https://img.shields.io/badge/permissions-0%20(No%20Internet)-2FC48D?style=flat-square">
  <img alt="Offline" src="https://img.shields.io/badge/works-100%25%20offline-8C8371?style=flat-square">
  <a href="LICENSE">
    <img alt="License" src="https://img.shields.io/badge/license-MIT-15493C?style=flat-square">
  </a>
</p>

<br>

<p align="center">
  <a href="https://github.com/sayedmdsafwan/QuranKPI/releases/download/v0.0.1/QuranKPI-v0.0.1.apk">
    <img alt="Download APK" src="https://img.shields.io/badge/⬇%20Download%20APK%20(v0.0.1)-15493C?style=for-the-badge&logo=android&logoColor=white" height="42">
  </a>
  &nbsp;&nbsp;
  <a href="https://sayedmdsafwan.github.io/QuranKPI/">
    <img alt="Live Web Demo" src="https://img.shields.io/badge/🌐%20Try%20Live%20Demo-B9821A?style=for-the-badge&logo=googlechrome&logoColor=white" height="42">
  </a>
</p>

<p align="center">
  <a href="https://sayedmdsafwan.github.io/QuranKPI/"><strong>🌐 Open Web Version</strong></a> • 
  <a href="https://github.com/sayedmdsafwan/QuranKPI/releases"><strong>📦 All Releases</strong></a> • 
  <a href="https://github.com/sayedmdsafwan/QuranKPI/issues"><strong>🐛 Report an Issue</strong></a>
</p>

</div>

---

## 🌟 Overview

**Quran KPI** is a calm, zero-dependency, privacy-first application built to track Quran reading consistency and calculate performance-linked incentives or monthly stipends for students, children, and hifz mentors.

Traditional spreadsheets are cumbersome to update on mobile devices, while cloud-based habit apps leak personal routines and require account logins. Quran KPI solves this with a purpose-built calendar engine:
- **Tracks daily paras read** with Saturday-to-Friday academic week structures (Friday off).
- **Automatically scales targets** for partial weeks at the beginning or end of a month.
- **Evaluates weekly performance** against configurable attendance and volume thresholds.
- **Calculates pro-rated payables** (salary + performance bonus - deductions for failed weeks).
- **Renders publication-ready monthly statements** on a high-resolution 2× canvas for instant image sharing to WhatsApp or mentors.
- **Guarantees 100% data sovereignty**: operates strictly offline with zero trackers, zero ads, zero external CDNs, and zero internet permissions.

---

## ✨ Features

### 📅 Smart Weekly KPI Engine
- **Saturday–Friday Calendar Structure**: Tailored for Islamic schooling and study weeks (Friday marked as default day off).
- **Partial Week Pro-Rating**: Weeks that start mid-month or end mid-month dynamically scale their required days and paras based on the ratio of working days (`working_days / 6`).
- **Real-Time Evaluation**: Weeks are tagged dynamically:
  - `PASS` (Green): Met both minimum days and paras requirements.
  - `FAIL` (Red): Closed week where attendance or volume fell short.
  - `PAID (OVERRIDE)` (Gold): Discretionary approval by mentor/parent to award base salary.
  - `IN PROGRESS` / `PENDING`: Ongoing week awaiting completion.

### 💰 Payroll & Incentive Accounting
- **Transparent Formula**: Pro-rates base monthly salary and bonus across working days of the month.
- **Bonus Target Tracking**: Rewards students when exceeding standard targets within a passed week.
- **Failed Week Deductions**: Clearly highlights deducted amounts for weeks not passed.
- **Mentor Override ("Pay anyway")**: Allows awarding base salary under special circumstances (e.g., sickness or travel) while preserving bonus target integrity.

### 🖼️ High-Res Statement Generator & Sharing
- **Built-in 2× Canvas Renderer**: Generates crisp, clean PNG statements without any heavy third-party graphics libraries.
- **Native Android Bridge**: In the native app, statements are saved directly to `Pictures/QuranKPI` and shared instantly via Android's native share sheet (`Intent.ACTION_SEND`).
- **Browser Fallback**: When run on GitHub Pages, gracefully falls back to the Web Share API or direct browser download.

### 🛡️ 100% Offline & Private
- **Zero Permissions**: The native app requests **0 runtime permissions**. It does not even ask for `android.permission.INTERNET`.
- **Pure Local Storage**: All entries, customized rules, and salaries remain strictly in the device's local storage (`localStorage`).
- **One-Tap JSON Backup & Restore**: Export the entire database into a timestamped `.json` file anytime, and restore it on any new device or browser.

---

## 📐 KPI Calculation Logic

Each calendar month is segmented into Saturday-to-Friday weeks:

| Parameter | Default Value | Description |
| :--- | :---: | :--- |
| **Full Week Working Days** | 6 days | Saturday through Thursday (Friday off) |
| **Min Days Read / Full Week** | 4 days | Attendance requirement |
| **Min Paras / Full Week** | 6.0 paras | Volume requirement |
| **Bonus Target / Full Week** | 9.0 paras | Quota required to unlock bonus |
| **Max Counted / Day** | 2.0 paras | Cap to prevent cramming and foster consistent daily reading |

### Scaling & Math:
1. **Working Days Weight**: For any week $i$ with $W_i$ working days:
   $$\text{Required Days}_i = \left\lceil 4 \times \frac{W_i}{6} \right\rceil, \quad \text{Required Paras}_i = 6 \times \frac{W_i}{6}$$
2. **Daily Earnings Rate**:
   $$\text{Daily Salary} = \frac{\text{Monthly Salary}}{\sum W_i}, \quad \text{Daily Bonus} = \frac{\text{Monthly Bonus}}{\sum W_i}$$
3. **Week Status**:
   $$\text{PASS} \iff \text{Days Read}_i \ge \text{Required Days}_i \land \text{Paras Read}_i \ge \text{Required Paras}_i$$
4. **Final Week Payout**:
   $$\text{Payout}_i = \begin{cases} 
   (\text{Daily Salary} \times W_i) + (\text{Daily Bonus} \times W_i) & \text{if PASS and Paras}_i \ge \text{Bonus Target}_i \\
   \text{Daily Salary} \times W_i & \text{if PASS or OVERRIDE} \\
   0 & \text{if FAIL} 
   \end{cases}$$

---

## 🏗️ Architecture & Dual Experience

Quran KPI is designed with a lightweight **dual-runtime architecture**:

```
Quran KPI
├── Web Runtime (GitHub Pages / Browser)
│   └── docs/index.html (Single-file zero-dependency HTML5 / CSS3 / Vanilla JS)
│       ├── LocalStorage Database ("quranKPI.v1")
│       ├── 2× Canvas Statement Renderer
│       └── Web Share / Blob Download fallback
│
└── Android Runtime (Native Shell)
    └── app/src/main/java/.../MainActivity.java
        ├── Hardware-accelerated Edge-to-Edge WebView
        ├── Android 13+ OnBackInvokedDispatcher lifecycle
        ├── WebChromeClient SAF onShowFileChooser (JSON restore)
        └── Native JavaScriptInterface Bridge:
            ├── Android.saveFile()   -> MediaStore.Downloads
            └── Android.shareImage()  -> MediaStore.Images + Intent.ACTION_SEND
```

- **Android App**: Fast, smooth native container under **31 KB**, compiled with Android 14 (API 34) and minimum SDK 29.
- **Web App**: Direct drop-in static page hosted on GitHub Pages that requires no build tools, bundlers, or servers.

---

## 📲 Installation

### Option 1: Android Device (APK)
1. Download the pre-built APK:  
   👉 **[Download QuranKPI-v0.0.1.apk](https://github.com/sayedmdsafwan/QuranKPI/releases/download/v0.0.1/QuranKPI-v0.0.1.apk)**
2. Open the downloaded file on your Android phone.
3. If prompted by Android, grant permission to **Install unknown apps** for your browser or file manager.
4. Tap **Install** and launch **Quran KPI**.

> *Note: Because this is a lightweight direct APK build, Google Play Protect may display a generic verification prompt. The app has zero internet permissions and its complete source code is open in this repository.*

### Option 2: Live Web Version
Prefer not to install anything? Open the full app instantly in any mobile or desktop web browser:  
👉 **[Launch Quran KPI Live Web App](https://sayedmdsafwan.github.io/QuranKPI/)**

---

## 🛠️ Building & Development

### Web App
To run or edit the web frontend locally:
1. Open [`docs/index.html`](docs/index.html) directly in any modern web browser.
2. No Node.js, npm, or build step required!

### Android App
To compile the Android APK locally:
```powershell
# In the repository root
$env:JAVA_HOME = "C:\Program Files\Android\Android Studio\jbr"
$env:Path = "$env:JAVA_HOME\bin;$env:Path"

# Compile debug APK
.\gradlew.bat assembleDebug

# Compile release APK
.\gradlew.bat assembleRelease
```
The compiled APK will be generated at:
`app/build/outputs/apk/debug/app-debug.apk`

---

## 🔐 Privacy & Security

- **0 Permissions**: No `android.permission.INTERNET`, no location, no contacts, no camera, no mic.
- **Zero Third-Party SDKs**: No Google Analytics, no Firebase, no Facebook SDK, no crash reporters.
- **Scoped Storage**: Statement images are written safely via `MediaStore` directly to `Pictures/QuranKPI`, and backups are stored in your device's `Download` folder.
- **No Remote Server**: Your data never leaves your device unless you manually share a generated statement or export a backup file.

---

## 📄 License

This project is licensed under the [MIT License](LICENSE) — see the LICENSE file for details.

Developed with care by **[Md. Safwan](https://github.com/sayedmdsafwan)**.
