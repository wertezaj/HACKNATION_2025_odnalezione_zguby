# Lost Items Gateway (Municipal Lost & Found Portal)

A comprehensive full-stack web application built with **React (Vite)** and **Node.js/Express**, designed to digitize and secure the reporting and public disclosure of found items. Key capabilities include automated data extraction using **OpenAI Vision API** and seamless integration with national open-data pipelines (generating standardized CSV/XML files and simulating data ingestion for `dane.gov.pl`).

---

## 🔑 Key Features & Technical Highlights

* **AI Vision Integration:** Automated parsing of item attributes (category, color, name) directly from uploaded images to pre-populate the reporting form.
* **Multilingual Support:** Automated translation of item descriptions (PL, EN, UA) via external API integration.
* **Open-Data Standardization (CSV & XML):** 
  * Generates and maintains the master registry in **UTF-8 with BOM** CSV format to ensure accurate Polish character encoding and native Microsoft Excel compatibility.
  * Dynamically generates structured XML metadata files for automated government harvesting.
* **Data Integrity Verification (MD5 Checksums):** The backend automatically computes and updates the MD5 hash of the XML file upon every modification to guarantee data integrity during automated government ingestion.
* **Encrypted Transport (HTTPS):** Full SSL/TLS encryption across the client-server pipeline to comply with browser geolocation API requirements and government security standards.
* **Geolocation & Real-Time Validation:** Integration with the **Google Maps API** for precise GPS coordinate and address logging, coupled with dynamic, real-time input validation.
* **QR Code Labeling & Public Tracking:** 
  * One-click generation of printable physical labels featuring a unique QR code and report ID.
  * Scanning the QR code routes citizens to a public verification portal displaying item details and real-time status (`READY FOR PICKUP` / `CLAIMED`).

---

## 🚀 Getting Started

The application follows a decoupled client-server architecture consisting of the **React Frontend** and the **Node.js Backend/Gateway**. Both services must run concurrently.

### 1. Prerequisites & Security Setup
* **Node.js** (v18.0 or higher)
* **npm** (v9.0 or higher)
* **SSL/TLS Certificates:** Local X.509 certificate and private key files (`localhost.pem`, `localhost-key.pem`) must be placed inside the `lost-items-gateway` directory to initialize the HTTPS server.

### 2. Environment Configuration
Create a `.env` file in both the frontend root directory (`frontend/BRZ`) and the backend directory (`lost-items-gateway`).

| Service / Purpose | Environment Variable | Location | Description |
| :--- | :--- | :--- | :--- |
| **OpenAI API** | `VITE_OPENAI_API_KEY` | Frontend (`.env`) | API key for AI image analysis. *(Prototype configuration; see Security Notes below).* |
| **Google Maps** | `VITE_GOOGLE_MAPS_API_KEY` | Frontend (`.env`) | API key for Places Autocomplete and Maps rendering. |
| **Backend Host** | `PUBLIC_HOST` | Backend (`.env`) | Full server URL (e.g., `https://localhost:3001`), required for generating valid QR routing links. |

---

## 💻 Installation & Execution

### A. Frontend Setup (React App)
1. Navigate to the frontend directory:
   ```bash
   cd frontend/BRZ
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Start the development server:
   ```bash
   npm run dev
   ```
4. The client application will be accessible at `http://localhost:5173`.

### B. Backend Setup (Lost Items Gateway)
1. Navigate to the gateway server directory:
   ```bash
   cd lost-items-gateway
   ```
2. Install dependencies:
   ```bash
   npm install
   ```
3. Launch the Node.js HTTPS server:
   ```bash
   node server.js
   ```
4. The gateway will listen at `https://localhost:3001`. Inspect the terminal output to verify that the SSL/TLS certificates were loaded successfully.

---

## 🛡️ Security & Architecture Considerations

* **Transport Layer Security:** Enforced HTTPS on the Node.js gateway (`localhost.pem` / `localhost-key.pem`) to protect data in transit and satisfy secure-context requirements for browser Geolocation APIs.
* **File Integrity vs. Cryptographic Hashing:** The system computes **MD5** checksums strictly to comply with the legacy `dane.gov.pl` open-data ingestion specification for non-adversarial file verification. For cryptographic security or tamper-proof auditing, **SHA-256** or **HMAC** is recommended.
* **API Key Management (Prototype Trade-off):** For rapid prototyping, `VITE_OPENAI_API_KEY` is configured at the client level. In a production deployment, all third-party AI API calls should be proxied through the `lost-items-gateway` backend to prevent client-side secret exposure and enforce rate limiting.
* **Input Validation & Sanitization:** Real-time form validation ensures schema compliance prior to XML/CSV serialization, mitigating malformed data injection (such as CSV Formula Injection when opened in spreadsheet software).
