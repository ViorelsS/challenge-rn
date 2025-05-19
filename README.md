
# JEToP Mobile Dev Challenge ’25

**Data di consegna:** Martedì 3 Giugno 2025, ore 12:00  
**Presentatore / Referente:** Viorel Strogoteanu – Software Developer @ deda.bit  
**Canale di supporto:** Telegram @viorelss (tutte le sere fino alla consegna)

---

## 📖 Descrizione

Questa challenge invita gli associati JEToP a creare, in **React Native** con **Expo SDK 5**, un’app utility **cross-platform** che metta alla prova competenze di:

- Navigazione  
- Gestione dello stato  
- Integrazione AI (Ollama + modelli open source)   

L’obiettivo minimo è fornire un’app **funzionante** con **CRUD** (su eventi) e **navigazione** base.

---

## 🚀 Tech Stack & Tooling

- **Framework:** React Native + Expo SDK 53  
- **State management:** Zustand (o Redux / Context API)  
- **Autenticazione:** OAuth Google via Clerk + Role Based Access Control su database  
- **Backend (a scelta):** Firebase, Supabase, Convex o custom con database sql/mongo (node+express, nest.js, bun, deno, ts, o anche in C se volete divertirvi) 
- **Formattazione:** Prettier  
- **DB locale (opzionale):** container Docker Supabase o db classici sql 

---

## 📦 Setup & Installazione

1. **Prerequisiti**  
   - Node.js (v21+)  
   - Expo CLI (`npm install –g expo-cli`)  

2. **Inizializza una nuova app**

   ```bash
   npx rn-new@latest
   ```
Questo tool permette di creare un'applicazione Expo con nativewind (tailwind per react native), zustand, navigation e molto altro già configurato. Vi permette di risparmiare tempo e bug vari. [Qui🔗](https://docs.rn.new/en/introduction) link alla documentazione.

Potreste avere problemi di compatibilità di node, quindi consiglio di installare *nvm* in modo da poter installare e configurare la versione LTS più recente di node.

Una volta partito il wizard per il progetto potete selezionare le opzioni di stile, gestione stato, autenticazione, etc tramite comandi da CLI.

3. **Backend**
Avete piena libertà di usare cosa più vi piace. Alcune soluzioni che mi sento di consigliare:
- Soluzioni con free-tier e hostate in cloud: [Convex🔗](https://www.convex.dev/), [Firebase🔗](https://firebase.google.com/), [Supabase](https://supabase.com/)
- Soluzioni self-hosted o locali: node/bun/deno/nest/etc con database locale (mysql, mongo, supabase, etc) oppure con database in cloud [Neon🔗](https://neon.tech/).

Potete usare più database per l'applicazione (ad. es, per le info generali/utenti/eventi usate un db relazionale in locale o neon, mentre per la parte di AI ed embedding usate supabase, dal sito ufficiale o self-hostato).

4. (Opzionale) **Avvia Supabase in Docker**
Se non volete usare un database con il free-tier online, potete usare i container con docker. Qui sotto un esempio con supabase.
   ```bash
   cd infra/supabase
   docker-compose up -d
   ```

5. **Gestione immagini**
   - Cloudinary (free tier)
   - Self-host di una qualsiasi tipologia di bucket
   - Bucket/container cloud
   - base64 nel db
---

## 🎯 Funzionalità “Must”

1. **Mini-game Dadi**

   * Numero di dadi configurabile
   * Lancio casuale e visualizzazione risultato

2. **Generatore di Nickname AI**

   * Chiamata a Ollama con modelli aperti
   * Mostra nickname generati
   * Valutate voi se avere un sistema di autenticazione all'app per salvare i dati dell'utente e non doverli reinserire sempre o se preferite evitare e chiedere di inserire sempre tutte le info.

3. **Lista Eventi JEToP**

   * Fetch da API/backend degli eventi del network (deve esserci la possibilità di crearli, con permessi speciali per chi lo ha creato - modifica, cancellazione)
   * Pulsante “Pre-iscriviti” per ogni evento
   * Distanza dalla posizione attuale dell'utente (finta o geolocalizzata)

---

## ✨ Features “Nice to Have” (facoltative)

* Impostazioni profilo (nome, foto)
* Geolocalizzazione per calcolo distanza evento
* Pagamenti tramite Stripe (web-checkout). Questo nel caso di pre-iscrizione ad un evento che prevede il pagamento di una quota.
* Generazione QR code per eventi prenotati. (Quando ci si prenota ad un evento, bisogna avere un badge sulla card dell'evento con scritto "Prenotato". Inoltre premendo sulla card dell'evento, si può visualizzare il qr code (che idealmente gli organizzatori possono scannerizzare e verificare che sia valido)

---

## 📋 Specifiche & Requisiti

| Caratteristica           | Descrizione                         | Obbligatoria |
| ------------------------ | ----------------------------------- | ------------ |
| Mini-game Dadi           | Dadi configurabili + lancio casuale | ✔            |
| Generatore Nickname AI   | Integrazione Ollama                 | ✔            |
| Lista Eventi + Pre-iscr. | Fetch e pulsante pre-iscrizione     | ✔            |
| Profilo utente           | Foto & settings                     | ✗            |
| Geolocalizzazione        | Calcolo distanza                    | ✗            |
| Pagamenti Stripe         | Web-checkout                        | ✗            |
| QR Code eventi           | Generazione e condivisione          | ✗            |

---

## 🏆 Criteri di Valutazione

* **Funzionalità (30%)** – Copertura dei “must”, usabilità e completezza dei flussi
* **Performance & Stato (30%)** – Efficienza nel management dello stato e reattività
* **Qualità del Codice (20%)** – Architettura, componenti riusabili, leggibilità
* **Design & UX (10%)** – Coerenza, accessibilità e aderence alla brand identity JEToP
* **Documentazione (10%)** – README esaustivo + commenti nei componenti principali

---

## 📅 Timeline & Supporto

| Fase                    | Cosa                                          | Data / Orario               |
| ----------------------- | --------------------------------------------- | --------------------------- |
| **Kickoff & Briefing**  | Consegna brief + Q\&A                         | Oggi                        |
| **Development Sprint**  | Prototype e implementazione funzionalità base | 1–2 giorni dal kickoff      |
| **Check-in One-to-One** | Supporto individuale su Telegram              | Ogni sera fino al 26 Maggio |
| **Consegna Finale**     | Repo + slide con screenshot + demo            | Lunedì 26 Maggio, ore 12:00 |

**Consegna:**

* Repository pubblico o condiviso via email a `viorelss@icloud.com`
* Slide (2–3 max) con screenshot, descrizione delle feature e link al repo

---

## 📚 Risorse Utili

* **Expo Docs:** [https://docs.expo.dev/](https://docs.expo.dev/)
* **React Native Docs:** [https://reactnative.dev/](https://reactnative.dev/)
* **Ollama Integration Guide:** [Video Youtube](https://www.youtube.com/watch?v=kaK3ye8rczA) - [Sito ufficiale](https://ollama.com/) - [Guida nella repo](https://github.com/ViorelsS/challenge-rn/blob/main/SETUP-OLLAMA.md)
* **Esempi & Tutorial:** YT (c'è materiale in abbondanza)

---

## 🤝 Contribuire & Contatti

Per ogni domanda o supporto:

* **Telegram:** [@viorelss](https://t.me/viorelss)
* **Email:** [viorelss@icloud.com](mailto:viorelss@icloud.com)
* **LinkedIn:** [Viorel](https://www.linkedin.com/in/viorelss/)

---

## 🎁 Premio

Buono Amazon da **€ 50** + possibile pubblicazione sugli store ufficiali.

---

