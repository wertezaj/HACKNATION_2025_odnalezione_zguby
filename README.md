# 🇵🇱 Portal Urzędnika Rzeczy Znalezionych (Lost Items Gateway)

Ten projekt jest kompleksową aplikacją internetową, stworzoną w oparciu o **React (Vite)** i **Node.js/Express**, przeznaczoną do cyfryzacji procesu zgłaszania i publikacji odnalezionych przedmiotów. Kluczową funkcjonalnością jest **automatyczne przetwarzanie danych** z użyciem sztucznej inteligencji (OpenAI Vision API) oraz **integracja z systemami otwartych danych** (generowanie plików CSV/XML, symulacja publikacji do dane.gov.pl).

---

## 🚀 Uruchomienie Projektu

Aplikacja składa się z dwóch niezależnych części: **Frontend (React)** i **Backend/Gateway (Node.js)**. Obie muszą działać jednocześnie. 

### 1. Wymagania Systemowe

* **Node.js** (wersja 18 lub wyższa)
* **npm** (npm v9 lub wyższy)
* **Wymagane pliki certyfikatów** (`localhost-key.pem`, `localhost.pem`) w katalogu `lost-items-gateway` do uruchomienia serwera HTTPS.

### 2. Konfiguracja Środowiska (Klucze API)

Musisz utworzyć plik `.env` w **głównym katalogu frontendu** czyli BRZ oraz w katalogu **`lost-items-gateway`**.

| Usługa | Zmienna Środowiskowa | Lokalizacja | Opis |
| :--- | :--- | :--- | :--- |
| **OpenAI API** | `VITE_OPENAI_API_KEY` | Frontend (`.env`) | Klucz dla analizy zdjęć przez AI. |
| **Google Maps** | `VITE_GOOGLE_MAPS_API_KEY` | Frontend (`.env`) | Klucz dla Autocomplete i Map. |
| **Backend Host** | `PUBLIC_HOST` | Backend (`.env`) | Pełny URL serwera, np. `https://localhost:3001` (wymagany dla linków QR). |

---

## 💻 Instrukcja Uruchomienia

### A. Uruchomienie Frontend (React App)

1.  **Przejdź do głównego katalogu projektu (Frontend):**
    ```bash
    cd frontend\BRZ
    ```

2.  **Zainstaluj zależności:**
    ```bash
    npm install
    ```

3.  **Uruchom aplikację w trybie deweloperskim:**
    ```bash
    npm run dev
    ```
    Aplikacja będzie dostępna pod adresem **`http://localhost:5173`**.

---

### B. Uruchomienie Backend (Lost Items Gateway)

1.  **Przejdź do katalogu serwera:**
    ```bash
    cd lost-items-gateway
    ```

2.  **Zainstaluj zależności:**
    ```bash
    npm install
    ```

3.  **Uruchom serwer Node.js:**
    ```bash
    node server.js
    ```
    Serwer uruchomi się pod adresem **`https://localhost:3001`**. Sprawdź konsolę, aby zweryfikować, czy certyfikaty SSL zostały poprawnie wczytane.

---

## 🔑 Kluczowe Funkcjonalności

* **AI Vision Integration:** Automatyczne parsowanie cech przedmiotu (kategoria, kolor, nazwa) ze zdjęcia i wstępne wypełnienie formularza.
* **Obsługa Wielojęzyczna:** Generowanie tłumaczeń opisów (PL, EN, UA) z wykorzystaniem zewnętrznego API.
* **Zapis Danych Otwartych:** Generowanie i przechowywanie rekordów zgłoszeń w pliku **CSV**.
* **Walidacja:** Dynamiczna walidacja formularza.
* **Generowanie Metadanych:** Dynamiczne generowanie pliku **XML**.

## Ogólny opis funkcjonalności

Prosty, responsywny interfejs (React + Tailwind CSS) prowadzi urzędnika krok po kroku. Geolokalizacja: Integracja z Google Maps API pozwala na precyzyjne wskazanie miejsca znalezienia przedmiotu (zapis współrzędnych GPS oraz adresu). Walidacja danych w czasie rzeczywistym. Generator CSV (UTF-8 z BOM): Tworzy i aktualizuje główny rejestr rzeczy znalezionych, dbając o poprawne kodowanie polskich znaków (zgodność z Excel). Dla każdego zgłoszenia generowany jest unikalny kod QR. Moduł Druku: Urzędnik jednym kliknięciem generuje gotową do druku etykietę z kodem QR i ID zgłoszenia, którą nakleja na znaleziony przedmiot. Publiczny Podgląd: Zeskanowanie kodu QR przez obywatela przenosi go na publiczną stronę ze szczegółami przedmiotu i jego statusem (np. "DO ODBIORU" lub "ODEBRANO"). Komunikacja w pełni szyfrowana przez HTTPS (wymóg geolokalizacji i standardów rządowych). Architektura Klient-Serwer (Node.js + Express), gdzie Backend pełni rolę Gatewaya wystawiającego pliki dla importera rządowego. Suma kontrolna MD5: System automatycznie oblicza hash MD5 dla pliku XML przy każdej zmianie, co jest kluczowe dla automatu pobierającego dane.
