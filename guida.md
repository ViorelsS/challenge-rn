# 🚀 Guida Step-by-Step – JEToP Mobile Dev Challenge 2025

Benvenuto nella guida ufficiale per la realizzazione dell'app per la JEToP Mobile Dev Challenge 2025.

---

🗺️ Mappa dei Passaggi

[🏁 1. Inizializzazione del progetto](#-passo-1--inizializzazione-del-progetto)  
• Installazione tool di sviluppo (Node, Expo, nvm, etc.)  
• Creazione progetto con `npx rn-new@latest`  
• Scelta delle opzioni per stile, autenticazione, stato

[⚙️ 2. Struttura del progetto](#-passo-2--struttura-del-progetto)  
• Organizzazione cartelle e file principali

[🛠️ 3. Setup strumenti principali](#-passo-3--setup-strumenti-principali-tailwind-zustand-navigation)  
• TailwindCSS (NativeWind), Zustand, Expo Router

[🔐 4. Autenticazione con Clerk](#-passo-4--autenticazione-con-clerk)  
• Setup iniziale con OAuth Google  
• Protezione schermate

[🧠 5. Integrazione AI con Ollama](#-passo-5--integrazione-ai-con-ollama-generatore-di-nickname)  
• Setup Ollama in locale  
• API e generazione nickname AI

[🎲 6. Mini‑gioco dei dadi](#-passo-6--mini%E2%80%91gioco-dei-dadi)  
• Configurazione dadi  
• Lancio e visualizzazione risultati

[📅 7. Eventi con Supabase](#-passo-7--eventi-crud-prenotazione-e-distanza)  
• CRUD eventi  
• Prenotazioni  
• Calcolo distanza

[🎟️ 8. Prenotazioni e QR Code](#-passo-8--prenotazione-eventi-e-qr-code)  
• Tabella bookings  
• Trigger Supabase  
• QR Code evento

[💅 9. Design, immagini e asset](#-passo-9--design-immagini-asset-e-presentazione-finale)  
• Icone, immagini, UI/UX  
• Generazione automatica asset  
• UI kit e cloni da app esistenti

[💳 10. Pagamenti con Stripe Checkout](#-10--pagamenti-in-app-stripe-checkout)  
• Backend per sessione checkout  
• Integrazione con Expo WebBrowser  
• UI e schermata di pagamento

[📸 11. Preparazione presentazione finale](#-11--preparazione-presentazione-finale)  
• Slide e screenshot  
• Repository e invio

[✅ 12. Testing, Debug & Rifinitura Finale](#-12--testing-debug--rifinitura-finale)  
• Verifica funzionalità  
• Pulizia codice e struttura repo  
• Test cross-device

📚 Bonus  
[📘 Bonus 1 – Documentare bene il codice](#-bonus-1--documentare-bene-il-codice)  
[🔐 Bonus 2 – Autenticazione alternativa con `better-auth`](#-bonus-2--per-i-pi%C3%B9-temerari-autenticazione-con-better-auth)  
[⚡️ Bonus 3 – Tips & Tricks React Native + Expo](#-bonus-3--tips--tricks-per-react-native--expo)  
[🎨 Bonus 4 – UX/UI Tips Pro](#-bonus-4--uxui-tips-pro)

---

# 🏁 Passo 1 – Inizializzazione del Progetto

## 🎯 Obiettivo

Creare un nuovo progetto React Native con Expo SDK 53 già configurato con:

- **NativeWind** per lo styling (TailwindCSS per RN)
- **Zustand** per la gestione dello stato
- **React Navigation**
- **Clerk** per l'autenticazione con OAuth Google
- Prettier, ESLint e supporto TypeScript

---

## ⚙️ 1.1 – Installa Node.js (usando NVM)

Per evitare problemi di compatibilità, usa **NVM** per installare Node.js in versione LTS.

### ➤ Linux / macOS

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash
```

Poi chiudi e riapri il terminale, quindi esegui:

```bash
nvm install --lts
nvm use --lts
```

### ➤ Windows

Usa nvm-windows e poi apri un terminale PowerShell o CMD:

```bash
nvm install lts
nvm use lts
```

## 📦 1.2 – Inizializza il progetto con `rn-new`

Usa il tool `rn-new@latest`, ottimizzato per Expo SDK 53:

```bash
npx rn-new@latest
```

Segui il wizard e scegli le seguenti opzioni:

| Prompt                   | Risposta consigliata                      |
| ------------------------ | ----------------------------------------- |
| **App Name**             | `jetop-app` (o un nome a piacere)         |
| **Stack**                | Expo + NativeWind + Zustand + Navigation  |
| **Autenticazione**       | Clerk (OAuth Google)                      |
| **Linter/Formatter**     | ESLint + Prettier                         |
| **Database (opzionale)** | Supabase o nessuno                        |
| **Testing**              | Nessuno (non necessario per la challenge) |

> 🧭 **Nota sulla Navigazione:**  
> Ti verrà chiesto di scegliere tra **React Navigation** e **Expo Router**.
>
> - **Expo Router** è la scelta consigliata per questa challenge perché utilizza una struttura **file-based routing** simile a Next.js, più intuitiva e moderna.
> - **React Navigation** è un’ottima alternativa classica, ma richiede più configurazione manuale per definire stack e tab.  
>   Se sei alle prime armi o vuoi una DX più fluida, scegli **Expo Router**.

> 🧱 **Nota sul tipo di Navigazione (stack/tabs/drawer):**  
> Dopo aver selezionato **Expo Router**, ti verrà chiesto il tipo di navigazione da usare:
>
> - **Stack** – consigliato per iniziare: semplice, lineare, ideale per flussi CRUD e schermate principali.
> - **Tabs** – utile se hai più sezioni (es. Eventi, Gioco, Profilo), mostra le icone in basso.
> - **Drawer + Tabs** – combinazione più complessa, con menu laterale + tab bar, da usare solo se necessario.
>
> 👉 Per questa challenge, **Stack** o **Tabs** sono le opzioni consigliate a seconda del layout che immagini per l'app.

Il comando finale è `npx rn-new@latest jetop-app --expo-router --tabs --nativewind --zustand --supabase`

## 🚀 1.3 – Avvia l’app

Vai nella cartella del progetto e installa le dipendenze:

```bash
cd jetop-app
bun install   # oppure: npm install / yarn install
```

Avvia Expo:

```bash
bunx expo       # oppure: npx expo start
```

Nota: su windows potresti avere problemi a collegarti ad Expo go con il qr code: in questo caso conviene installare subito le dipendenze di ngrok con `bun install @expo/ngrok` e runnare l'applicazione con `bunx expo start --tunnel` (Per qualche strana ragione se inserisci il flag _--tunnel_ dentro lo script nel package.json non funziona). Potrebbero volerci un paio di riavvii dell'applicazione prima di riuscire a collegarsi.

> 📱 Apri l’app su **Expo Go** scansionando il QR code, oppure esegui su emulatore.

---

# ⚙️ Passo 2 – Struttura del Progetto

## 🗂️ 2.1 – Organizzazione delle Cartelle

Dopo aver inizializzato il progetto, ti troverai con una struttura di base simile a questa:

```
jetop-app/
├── app/                    # Pagina e navigazione file-based (Expo Router)
├── assets/                 # Immagini, font, media statici
├── components/             # Componenti riutilizzabili (UI e logica)
├── node_modules/           # Dipendenze installate
├── store/                  # Store Zustand
├── utils/                  # Funzioni di utilità, formatter, helpers
├── .env                    # Variabili d’ambiente (non committare in Git!)
├── .gitignore              # File ignorati da Git
├── app-env.d.ts            # Tipizzazione variabili d’ambiente
├── app.json                # Configurazione base per Expo
├── babel.config.js         # Configurazione Babel
├── bun.lock                # Lockfile per gestore Bun
├── cesconfig.json          # Configurazione Clerk
├── eslint.config.js        # Configurazione linting
├── expo-env.d.ts           # Definizioni ambiente Expo
├── global.css              # Stili globali condivisi
├── metro.config.js         # Config Metro bundler
├── package-lock.json       # Lockfile (npm)
├── package.json            # Manifesto progetto con dipendenze
├── prettier.config.js      # Configurazione Prettier
├── tailwind.config.js      # Configurazione Tailwind (NativeWind)
└── tsconfig.json           # Configurazione TypeScript
```

Puoi personalizzarla come preferisci, ma è importante mantenere:

- **Modularità**: separa logica, componenti e storeƒ
- **Chiarezza**: nomi chiari per ogni cartella e file
- **Coerenza**: mantieni lo stile tra file simili

---

## ✨ 2.2 – File principali

| File                 | Ruolo                                                            |
| -------------------- | ---------------------------------------------------------------- |
| `app/`               | Cartella principale per le schermate e la navigazione file-based |
| `components/`        | Componenti riutilizzabili per UI o logica                        |
| `store/`             | Store Zustand e stato globale                                    |
| `utils/`             | Funzioni di utilità condivise (formattazione, validazioni, ecc.) |
| `.env`               | Variabili d’ambiente locali (non va committato)                  |
| `app-env.d.ts`       | Tipizzazioni TypeScript per le env variables                     |
| `global.css`         | Stili CSS condivisi (per NativeWind)                             |
| `tailwind.config.js` | Configurazione di Tailwind/Navigazione con NativeWind            |
| `app.json`           | Configurazione generica del progetto Expo                        |
| `babel.config.js`    | Configurazione per Babel                                         |
| `metro.config.js`    | Configurazione per il bundler Metro                              |
| `package.json`       | Manifesto del progetto con le dipendenze                         |
| `prettier.config.js` | Regole di formattazione Prettier                                 |
| `tsconfig.json`      | Configurazione TypeScript                                        |

---

---

# 🛠️ Passo 3 – Setup strumenti principali (Tailwind, Zustand, Navigation)

## 🌬️ 3.1 – Verifica e personalizza Tailwind (NativeWind)

Apri `tailwind.config.js` e assicurati che contenga:

```js
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./app/**/*.{js,ts,jsx,tsx}", "./components/**/*.{js,ts,jsx,tsx}"],
  theme: {
    extend: {
      colors: {
        primary: "#4f46e5", // Indigo
        secondary: "#ec4899", // Pink
        background: "#f9fafb", // Light gray
        accent: "#10b981", // Emerald
        muted: "#6b7280", // Gray
      },
      spacing: {
        "screen-padding": "1.5rem",
        "header-height": "4rem",
      },
      fontFamily: {
        sans: ["Inter", "System"],
        display: ["Poppins", "sans-serif"],
      },
      borderRadius: {
        xl: "1rem",
        "2xl": "1.5rem",
        btn: "0.75rem",
      },
    },
  },
  plugins: [],
};
```

---

### 🎨 Palette colori personalizzata

Puoi definire colori custom come `primary`, `accent`, `muted`, ecc.  
Utilizzali nei tuoi componenti tramite `className`:

```tsx
<Text className="text-primary">Titolo principale</Text>
<View className="bg-background p-4 rounded-xl">...</View>
<Button className="bg-accent text-white px-4 py-2 rounded-btn">Azione</Button>
```

---

### 📏 Spaziature e dimensioni custom

Puoi definire valori per margini, padding e dimensioni (es. header, footer):

```tsx
<View className="pt-screen-padding pb-header-height">...</View>
```

---

### 🅰️ Font personalizzati

Puoi estendere i font globali nel file `tailwind.config.js` come sopra.  
Per usarli in React Native, caricali con `expo-font`:

```tsx
import { useFonts } from "expo-font";

const [fontsLoaded] = useFonts({
  Inter: require("./assets/fonts/Inter-Regular.ttf"),
  Poppins: require("./assets/fonts/Poppins-SemiBold.ttf"),
});
```

Poi usa:

```tsx
<Text className="font-display text-lg">Titolo stiloso</Text>
```

---

✅ Ora Tailwind è pronto e personalizzato per la tua app!

---

## 📦 3.2 – Zustand Store

Apri o crea `store/index.ts` e definisci un semplice store:

```ts
import { create } from "zustand";

type State = {
  count: number;
  increase: () => void;
};

export const useCounter = create<State>((set) => ({
  count: 0,
  increase: () => set((state) => ({ count: state.count + 1 })),
}));
```

Puoi usare `useCounter()` in qualsiasi componente per accedere e aggiornare lo stato.

---

## 🧭 3.3 – Navigazione con Expo Router

La cartella `app/` è già configurata con **file-based routing**.

Per creare nuove schermate:

- `app/index.tsx` → Home
- `app/events.tsx` → Schermata Eventi
- `app/game.tsx` → MiniGioco
- `app/profile.tsx` → Profilo Utente

Naviga tra le schermate usando:

```ts
import { Link } from "expo-router";

<Link href="/events">Vai agli eventi</Link>;
```

> 📘 Consulta la guida ufficiale: [Expo Router Docs](https://expo.github.io/router/docs)

# 🔐 Passo 4 – Autenticazione con Clerk

## 🔑 4.1 – Setup iniziale

Clerk è già configurato se hai scelto l’opzione `--clerk` nel comando `rn-new`. Altrimenti, puoi aggiungerlo manualmente seguendo questi passaggi:

1. Registra un account su [https://clerk.com](https://clerk.com) e crea un nuovo progetto.
2. Vai nella dashboard > API Keys > copia la **Publishable Key**.
3. Aggiungila nel file `.env`:

```env
CLERK_PUBLISHABLE_KEY=pk_test_xxx
```

4. In `app/_layout.tsx` (oppure in `app/layout.tsx`), avvolgi l’app con il provider:

```tsx
import { ClerkProvider } from "@clerk/clerk-expo";
import Constants from "expo-constants";

export default function Layout() {
  return (
    <ClerkProvider
      publishableKey={Constants.expoConfig?.extra?.clerkPublishableKey}
    >
      <Slot />
    </ClerkProvider>
  );
}
```

---

## 🧩 4.2 – Configura il provider Clerk

Modifica `app.config.js` o `app.config.ts` per passare la variabile d’ambiente:

```ts
import "dotenv/config";

export default {
  name: "jetop-app",
  slug: "jetop-app",
  extra: {
    clerkPublishableKey: process.env.CLERK_PUBLISHABLE_KEY,
  },
};
```

        | Extra tip: se non si vuole configurare tutte le chiavi/secrets dentro app.config.* si può anteporre EXPO_PUBLIC... davanti alla chiave.

---

## 👤 4.3 – Login e accesso utente

Aggiungi la schermata di autenticazione (se non generata automaticamente):

```tsx
import { SignedIn, SignedOut, useUser } from "@clerk/clerk-expo";

export default function Home() {
  const { isLoaded, isSignedIn, user } = useUser();

  if (!isLoaded) return null;

  return (
    <>
      <SignedIn>
        <Text>Benvenuto, {user?.firstName}!</Text>
      </SignedIn>
      <SignedOut>
        <Text>Per favore, accedi all'app.</Text>
      </SignedOut>
    </>
  );
}
```

---

✅ A questo punto hai un’autenticazione funzionante con Clerk!  
🔜 Prossimo step: integrazione AI con Ollama e generatore di nickname.

---

# 🧠 Passo 5 – Integrazione AI con Ollama (generatore di nickname)

## 🎯 Obiettivo

Integrare un generatore di nickname basato su AI usando **Ollama**, con un modello locale open-source. L'utente inserisce un contesto o una descrizione e riceve una lista di nickname.

---

## 💻 5.1 – Setup di Ollama

### ➤ Requisiti

- Node.js già installato
- Bun installato
- Docker **non necessario** (Ollama funziona nativamente)

### ➤ Installa Ollama

Scarica e installa Ollama da:

📦 [https://ollama.com/download](https://ollama.com/download)

Avvia Ollama:

```bash
ollama serve
```

Poi, carica un modello:

```bash
ollama pull llama3
```

Puoi anche usare `mistral`, `gemma`, `phi`, ecc.

---

## 🧪 5.2 – Testa Ollama in locale

Crea un file `scripts/test-ollama.ts` per testare una chiamata semplice:

```ts
import { spawn } from "child_process";

const prompt =
  "Genera 5 nickname originali per un'app per studenti universitari";

const ollama = spawn("ollama", ["run", "llama3"], {
  stdio: ["pipe", "pipe", "inherit"],
});

ollama.stdin.write(prompt);
ollama.stdin.end();

ollama.stdout.on("data", (data) => {
  console.log("Output:", data.toString());
});
```

Esegui con:

```bash
bun scripts/test-ollama.ts
```

Dovresti vedere l’output del modello nella tua console.

---

## 🌐 5.3 – Esporre un’API locale per la mobile app

    | In un backend separato realizziamo facciamo questi passaggi

Crea un file `api/ollama.ts` con un piccolo server (usando `express` o `elysia`):

```ts
import { spawn } from "child_process";
import express from "express";
import cors from "cors";

const app = express();
app.use(cors());
app.use(express.json());

app.post("/nickname", (req, res) => {
  const prompt = req.body.prompt || "Genera 5 nickname originali";
  const ollama = spawn("ollama", ["run", "llama3"], {
    stdio: ["pipe", "pipe", "inherit"],
  });

  ollama.stdin.write(prompt);
  ollama.stdin.end();

  let output = "";
  ollama.stdout.on("data", (data) => {
    output += data.toString();
  });

  ollama.stdout.on("end", () => {
    res.json({ response: output });
  });
});

app.listen(3001, () => {
  console.log("API Ollama attiva su http://localhost:3001");
});
```

---

## 📱 5.4 – Collegare la mobile app a Ollama

Crea un file `lib/ollama.ts`:

```ts
export async function generateNicknames(prompt: string): Promise<string[]> {
  const res = await fetch("http://localhost:3001/nickname", {
    method: "POST",
    headers: { "Content-Type": "application/json" },
    body: JSON.stringify({ prompt }),
  });

  const data = await res.json();
  return data.response.split("\n").filter((line) => line.trim() !== "");
}
```

E usa la funzione in una schermata `app/nickname.tsx`:

```tsx
import { useState } from "react";
import { View, TextInput, Button, Text, ScrollView } from "react-native";
import { generateNicknames } from "@/lib/ollama";

export default function NicknameScreen() {
  const [prompt, setPrompt] = useState("");
  const [results, setResults] = useState<string[]>([]);

  const handleGenerate = async () => {
    const names = await generateNicknames(prompt);
    setResults(names);
  };

  return (
    <ScrollView className="p-4">
      <TextInput
        placeholder="Descrivi lo stile dei nickname"
        value={prompt}
        onChangeText={setPrompt}
        className="border p-2 mb-2 rounded"
      />
      <Button title="Genera" onPress={handleGenerate} />
      {results.map((name, index) => (
        <Text key={index} className="mt-2 text-lg">
          {name}
        </Text>
      ))}
    </ScrollView>
  );
}
```

---

✅ Ora puoi generare nickname da mobile grazie all’integrazione con Ollama!

---

# 🎲 Passo 6 – Mini‑gioco dei dadi

## 🎯 Obiettivo

Implementare un minigioco offline in cui l’utente può configurare il numero di dadi da lanciare (default: 2) e visualizzarne il risultato.

---

## 🧠 6.1 – Stato locale con Zustand

In `store/dice.ts`, crea uno store per gestire il numero di dadi:

```ts
import { create } from "zustand";

type DiceStore = {
  diceCount: number;
  setDiceCount: (count: number) => void;
};

export const useDiceStore = create<DiceStore>((set) => ({
  diceCount: 2,
  setDiceCount: (count) => set({ diceCount: count }),
}));
```

---

## 🧪 6.2 – Logica di lancio casuale

In `utils/rollDice.ts`, aggiungi una funzione per simulare i lanci:

```ts
export function rollDice(n: number): number[] {
  return Array.from({ length: n }, () => Math.floor(Math.random() * 6) + 1);
}
```

---

## 🎮 6.3 – Schermata di Gioco

Crea `app/game.tsx`:

```tsx
import { useState } from "react";
import { View, Text, Button, TextInput } from "react-native";
import { useDiceStore } from "@/store/dice";
import { rollDice } from "@/utils/rollDice";

export default function GameScreen() {
  const { diceCount, setDiceCount } = useDiceStore();
  const [results, setResults] = useState<number[]>([]);

  const handleRoll = () => {
    const values = rollDice(diceCount);
    setResults(values);
  };

  return (
    <View className="p-4">
      <Text className="text-xl mb-2">Numero di dadi:</Text>
      <TextInput
        keyboardType="numeric"
        value={diceCount.toString()}
        onChangeText={(text) => setDiceCount(Number(text))}
        className="border px-2 py-1 mb-4 rounded"
      />

      <Button title="Lancia i dadi!" onPress={handleRoll} />

      <View className="mt-6">
        <Text className="text-lg font-semibold">Risultati:</Text>
        {results.map((val, i) => (
          <Text key={i}>
            🎲 Dado {i + 1}: {val}
          </Text>
        ))}
      </View>
    </View>
  );
}
```

---

## ✅ Checklist

- [x] Stato configurabile per numero dadi
- [x] Generazione numeri casuali da 1 a 6
- [x] Visualizzazione risultati
- [x] Offline-ready (nessuna chiamata esterna)

---

# 📅 Passo 7 – Eventi: CRUD, prenotazione e distanza

## 🎯 Obiettivo

Creare una funzionalità per la gestione degli eventi:

- Visualizzazione eventi
- Creazione evento (con permessi)
- Prenotazione eventi (con limite massimo)
- Calcolo distanza tra utente e luogo evento
- Conteggio prenotazioni attive

---

## 🧰 7.1 – Setup di Supabase

### 📌 Crea un account Supabase

1. Vai su [https://supabase.com](https://supabase.com) e registrati.
2. Clicca su **New Project**.
3. Inserisci:
   - Nome del progetto (es. `jetop`)
   - Password (salvala!)
   - Regione
4. Una volta creato, vai su **Project Settings → API** e copia:
   - **Project URL**
   - **Anon/public key**

### ⚙️ Configura l’SDK nella tua app

Installa il client Supabase nel progetto:

```bash
bun add @supabase/supabase-js
```

Crea `lib/supabase.ts`:

```ts
import { createClient } from "@supabase/supabase-js";

export const supabase = createClient(
  process.env.EXPO_PUBLIC_SUPABASE_URL!,
  process.env.EXPO_PUBLIC_SUPABASE_ANON_KEY!
);
```

Nel file `.env`:

```
EXPO_PUBLIC_SUPABASE_URL=https://xxx.supabase.co
EXPO_PUBLIC_SUPABASE_ANON_KEY=eyJ...
```

> 🟡 Assicurati che le chiavi inizino con `EXPO_PUBLIC_` così Expo può accedervi.

---

## 🛠️ 7.2 – Crea la tabella `events`

1. Vai nella sezione **Table Editor** → **New Table**
2. Nome: `events`
3. Colonne consigliate:

| Nome         | Tipo      | Extra                          |
| ------------ | --------- | ------------------------------ |
| id           | UUID      | Primary key, gen_random_uuid() |
| title        | Text      | required                       |
| location     | Text      |                                |
| latitude     | Numeric   |                                |
| longitude    | Numeric   |                                |
| date         | Timestamp |                                |
| owner_id     | UUID      |                                |
| created_at   | Timestamp | default: now()                 |
| max_guests   | Integer   | Numero massimo partecipanti    |
| booked_count | Integer   | Default: 0                     |

4. Salva la tabella.

---

## 🧾 7.3 – CRUD degli eventi (client)

Crea un file `lib/events.ts`:

```ts
import { supabase } from "./supabase";

export async function getEvents() {
  const { data, error } = await supabase.from("events").select("*");
  if (error) throw error;
  return data;
}

export async function createEvent(event: {
  title: string;
  location?: string;
  latitude?: number;
  longitude?: number;
  date?: string;
  owner_id?: string;
}) {
  const { error } = await supabase.from("events").insert([event]);
  if (error) throw error;
}
```

Esempio di utilizzo in `app/events.tsx`:

```tsx
import { useEffect, useState } from "react";
import { Text, ScrollView } from "react-native";
import { getEvents } from "@/lib/events";

export default function EventList() {
  const [events, setEvents] = useState<any[]>([]);

  useEffect(() => {
    getEvents().then(setEvents);
  }, []);

  return (
    <ScrollView className="p-4">
      {events.map((e) => (
        <Text key={e.id} className="mb-4 text-lg">
          {e.title} - {e.date}
        </Text>
      ))}
    </ScrollView>
  );
}
```

---

## 📍 7.4 – Calcolo distanza (opzionale)

Installa il pacchetto geodist:

```bash
bun add geolib
```

E usa così:

```ts
import { getDistance } from "geolib";

const distance = getDistance(
  { latitude: userLat, longitude: userLng },
  { latitude: eventLat, longitude: eventLng }
);
```

---

✅ Ora puoi gestire gli eventi con Supabase, CRUD completo e distanza!
🔜 Prossimo step: prenotazione eventi e QR code!

---

# 🎟️ Passo 8 – Prenotazione eventi e QR Code

## 🎯 Obiettivo

Permettere agli utenti autenticati di prenotare eventi, con limite massimo di partecipanti, aggiornare il contatore e generare un QR code univoco per ciascuna prenotazione.

---

## 🧰 8.1 – Crea la tabella `bookings` su Supabase

1. Vai su **Table Editor** → **New Table**
2. Nome: `bookings`
3. Colonne consigliate:

| Nome       | Tipo      | Extra                           |
| ---------- | --------- | ------------------------------- |
| id         | UUID      | Primary key, gen_random_uuid()  |
| event_id   | UUID      | Riferimento alla tabella events |
| user_id    | UUID      | ID dell’utente autenticato      |
| created_at | Timestamp | default: now()                  |

4. Aggiungi una **riga di policy** (Row Level Security) che consenta agli utenti loggati di creare e leggere solo le proprie prenotazioni:
   - Policy: `SELECT/INSERT WHERE user_id = auth.uid()`

---

## ⚙️ 8.2 – Aggiungi le funzioni di prenotazione in `lib/bookings.ts`

```ts
import { supabase } from "./supabase";

export async function createBooking(eventId: string) {
  const user = await supabase.auth.getUser();
  const user_id = user.data.user?.id;

  if (!user_id) throw new Error("Utente non autenticato");

  const { error } = await supabase.from("bookings").insert([
    {
      event_id: eventId,
      user_id,
    },
  ]);

  if (error) throw error;
}
```

---

## 📱 8.3 – Interfaccia prenotazione in `app/events.tsx`

Modifica la card evento per includere un pulsante “Prenota”:

```tsx
<Button title="Prenota" onPress={() => createBooking(e.id)} />
```

> ℹ️ Puoi mostrare un badge "Prenotato" se l'utente ha già prenotato l'evento.

---

## 🔢 8.4 – Incrementa automaticamente `booked_count`

In Supabase, crea una **Function SQL** con trigger:

```sql
create or replace function increment_booking()
returns trigger as $$
begin
  update events
  set booked_count = booked_count + 1
  where id = new.event_id;
  return new;
end;
$$ language plpgsql;

create trigger booking_created
after insert on bookings
for each row
execute procedure increment_booking();
```

---

## 📷 8.5 – Generazione del QR Code

Installa la libreria QR:

```bash
bun add react-native-qrcode-svg
```

E nel file `app/booking/[id].tsx`:

```tsx
import QRCode from "react-native-qrcode-svg";

<QRCode value={user.id + "_" + booking.id} size={200} />;
```

> 📎 Puoi mostrare questo QR dopo una prenotazione riuscita o dalla pagina profilo.

---

✅ A questo punto puoi:

- Prenotare un evento (se c’è posto)
- Vedere un badge “Prenotato”
- Generare e visualizzare un QR code personale

🔜 Passo successivo: ottimizzazioni, UI, immagini e presentazione finale.

---

# 💅 Passo 9 – Design, immagini, asset e presentazione finale

## 🎯 Obiettivo

Rendere l’app visivamente gradevole, professionale e coerente senza perdere tempo su Figma o asset manuali, grazie a risorse già pronte o strumenti assistiti.

---

## 🎨 9.1 – Icone

Usa una libreria di icone già integrata e compatibile con React Native:

- [Lucide Icons](https://lucide.dev/icons/) → moderno e semplice
- [Phosphor Icons](https://phosphoricons.com/)
- [Heroicons](https://heroicons.com/)

Installa (es. lucide):

```bash
bun add lucide-react-native
```

E usa nelle schermate:

```tsx
import { CalendarDays, MapPin } from "lucide-react-native"

<CalendarDays color="#000" size={20} />
<MapPin color="#000" size={20} />
```

---

## 🖼️ 9.2 – Immagini ed eventi

Per ottenere immagini gratuite e belle per eventi o profili:

- [Unsplash](https://unsplash.com) → foto reali ad alta qualità
- [Pexels](https://pexels.com) → alternativa ottima
- [Lorem Picsum](https://picsum.photos) → immagini placeholder random
- [Generated Photos](https://generated.photos/) → volti AI

Se vuoi generare con AI:

- [DALL·E su Bing](https://www.bing.com/images/create)
- [Playground AI](https://playgroundai.com/)
- [Leonardo AI](https://app.leonardo.ai/)

---

## 🎁 9.3 – Generazione automatica asset grafici

Vuoi un logo semplice per l’app?

- [https://logojoy.com](https://logojoy.com)
- [https://looka.com](https://looka.com)
- [https://app.brandmark.io/](https://app.brandmark.io/)
- [https://www.logomaster.ai/](https://www.logomaster.ai/)

Per generare **icone app, splashscreen, favicon**:

- [https://appicon.co/](https://appicon.co/)
- [https://easyappicon.com/](https://easyappicon.com/)
- [https://www.figma.com/community/plugin/791989143595060195/Iconify](https://www.figma.com/community/plugin/791989143595060195/Iconify)

---

## ✂️ 9.4 – Clona design da altre app

Vuoi replicare un’app famosa?

- Vai su [https://reactnativeelements.com/](https://reactnativeelements.com/)
- Cerca componenti simili a quelli che vuoi ricreare
- Usa [https://mobbin.com/](https://mobbin.com) o [https://collectui.com/](https://collectui.com) per ispirazione
- Copia layout e struttura visiva su base Tailwind/NativeWind

---

## 🖌️ 9.5 – Bonus: UI kit preconfigurati

Per evitare di partire da zero puoi usare:

- [https://ui.shadcn.dev/](https://ui.shadcn.dev/) – ottimo su Web, alcuni componenti adattabili in RN
- [https://gluestack.io/](https://gluestack.io/) – design system accessibile per React Native
- [https://nativebase.io/](https://nativebase.io/) – componenti ben fatti, plug & play

---

---

## 💳 10 – Pagamenti in-app (Stripe Checkout)

Stripe è il metodo consigliato per gestire **pagamenti sicuri** tramite browser. In questa guida implementerai un flusso di pagamento semplice usando **Stripe Checkout**, senza dover gestire carte, token o compliance.

> 📄 Documentazione ufficiale: https://docs.stripe.com/mobile/digital-goods

---

### 🎯 Obiettivo

- Mostrare un pulsante “Acquista”
- Aprire una pagina esterna con Stripe Checkout
- Ritornare in app dopo il pagamento (opzionale)

---

## 🧰 Backend (sessione di pagamento)

Per usare Stripe Checkout hai bisogno di un endpoint che crei una sessione. Esempio Express:

```ts
import express from "express";
import Stripe from "stripe";
import dotenv from "dotenv";
import cors from "cors";

dotenv.config();

const stripe = new Stripe(process.env.STRIPE_SECRET_KEY!, {
  apiVersion: "2023-10-16",
});

const app = express();
app.use(cors());
app.use(express.json());

app.post("/create-checkout-session", async (req, res) => {
  try {
    const session = await stripe.checkout.sessions.create({
      line_items: [
        {
          price_data: {
            currency: "eur",
            product_data: {
              name: "Prenotazione evento JEToP",
            },
            unit_amount: 500, // €5.00
          },
          quantity: 1,
        },
      ],
      mode: "payment",
      success_url: "https://jetop-app.com/success",
      cancel_url: "https://jetop-app.com/cancel",
    });

    res.json({ url: session.url });
  } catch (err) {
    res.status(500).json({ error: "Errore creazione sessione" });
  }
});

app.listen(3000, () => console.log("Server Stripe su http://localhost:3000"));
```

---

## 📱 Frontend (React Native)

### 1. Installa il modulo Expo per il browser:

```bash
bun add expo-web-browser
```

### 2. Funzione di pagamento `lib/payment.ts`:

```ts
import * as WebBrowser from "expo-web-browser";

export async function startPayment() {
  const response = await fetch(
    "http://localhost:3000/create-checkout-session",
    {
      method: "POST",
      headers: {
        "Content-Type": "application/json",
      },
    }
  );

  const { url } = await response.json();
  if (url) {
    await WebBrowser.openBrowserAsync(url);
  }
}
```

### 3. Schermata pagamento `app/payment.tsx`:

```tsx
import { View, Button } from "react-native";
import { startPayment } from "@/lib/payment";

export default function PaymentScreen() {
  return (
    <View className="p-6">
      <Button title="Paga con Stripe" onPress={startPayment} />
    </View>
  );
}
```

---

## 🎨 UI suggerita

```tsx
import { View, Text, Button } from "react-native";

export default function PaymentScreen() {
  return (
    <View className="flex-1 justify-center items-center p-6">
      <Text className="text-2xl font-bold mb-4">Biglietto Evento JEToP</Text>
      <Text className="mb-8 text-gray-700">Prezzo: 5€</Text>
      <Button title="Procedi al pagamento" onPress={startPayment} />
    </View>
  );
}
```

---

## ✅ Verifica finale

- [x] Backend attivo su localhost (o deploy su Render/Vercel)
- [x] Link Checkout ricevuto correttamente
- [x] Pagamento gestito tramite browser
- [x] Ritorno all’app con link di successo

---

🔐 Puoi abbinare la prenotazione evento (`bookings`) al completamento del pagamento tramite webhook Stripe o in base alla `success_url`.

## 📸 11 – Preparazione presentazione finale

1. Fai **screenshot** delle schermate principali:

   - Home / login
   - Eventi / dettaglio evento
   - Gioco dadi
   - Generazione nickname
   - Prenotazione evento con QR

2. Inserisci in (almeno) 2–3 slide:

   - Obiettivo app
   - Tecnologie usate
   - Screenshot e funzionalità chiave

3. Invia tutto via:
   - Email: `viorelss@icloud.com`
   - Oppure repo GitHub + PDF slide su telegram [@viorelss](https://t.me/viorelss)

---

---

# ✅ Passo 12 – Testing, Debug & Rifinitura Finale

## 🎯 Obiettivo

Rifinire l'app per consegna: correggere bug, migliorare UX, testare flussi e verificare che tutte le funzionalità richieste siano implementate correttamente.

---

## 🧪 12.1 – Test manuali funzionali

Verifica che tutte le funzionalità “must” funzionino correttamente:

- [ ] Login e logout con Google (Clerk)
- [ ] Generazione nickname da AI (Ollama)
- [ ] Lancio dadi con numero configurabile
- [ ] Visualizzazione eventi
- [ ] Prenotazione evento + badge "Prenotato"
- [ ] Calcolo distanza evento (se implementato)
- [ ] QR Code generato e leggibile

---

## 🔍 12.2 – Debug con Expo Go

Avvia il progetto con:

```bash
bunx expo start --tunnel
```

Apri l’app su **Expo Go** e:

- Testa ogni schermata su dispositivo reale
- Usa `console.log()` ovunque ti serva debug
- Attiva DevTools con `d` nel terminale
- Premi `r` per refresh, `m` per menu

---

## 📲 12.3 – Test cross-device

Verifica su:

- iOS (iPhone reale o simulatore)
- Android (dispositivo o emulator)

Controlla che gli stili siano coerenti, i font leggibili, e le icone visibili.

---

## 🧹 12.4 – Pulizia codice

- Rimuovi console inutili
- Commenta funzioni complesse
- Rinomina variabili poco chiare
- Esegui `bun format` o `npx prettier . --write`
- Controlla che `tailwind.config.js` abbia solo cartelle rilevanti

---

## 📁 12.5 – Struttura repository

Verifica che il tuo progetto sia ordinato:

```
📁 jetop-app/
├── app/
├── components/
├── store/
├── utils/
├── lib/
├── assets/
├── .env.example   # se usi variabili d’ambiente
├── README.md      # (puoi includere istruzioni base d’uso)
└── slides.pdf     # per la consegna
```

---

## 🚀 12.6 – Check finale di consegna

- [ ] App avviabile da zero (`bun install` + `bunx expo start`)
- [ ] Slide di presentazione completata
- [ ] Link repo
- [ ] Testata su almeno 1 device fisico

---

# 📚 Bonus 1 – Documentare bene il codice

Documentare il codice è fondamentale per facilitare la lettura, la manutenzione e il riutilizzo da parte tua o di altri membri del team. Ecco alcune buone pratiche per una documentazione efficace.

## ✏️ 1. Commenti nei file

- Scrivi commenti **chiari e concisi** sopra funzioni complesse, hook personalizzati o blocchi logici articolati.
- Usa commenti **JSDoc** per descrivere parametri, tipi, ritorni e comportamento delle funzioni:

```ts
/**
 * Genera una lista di nickname in base al prompt fornito
 * @param prompt - stringa di input descrittiva dello stile
 * @returns Array di stringhe con i nickname generati
 */
export async function generateNicknames(prompt: string): Promise<string[]> { ... }
```

## 🧩 2. Commenta i componenti

Ogni componente principale (pagina, modulo, UI riutilizzabile) dovrebbe avere un breve commento in cima che spieghi **cosa fa** e **quando viene usato**.

```tsx
// Componente che mostra un QR code univoco per la prenotazione
// Viene mostrato dopo una prenotazione andata a buon fine
export default function QRCodeScreen() { ... }
```

## 🧪 3. Documenta lo stato globale

Nel file `store/`, ogni store Zustand dovrebbe essere **tipizzato**, e commentato per spiegare cosa rappresenta e come si usa:

```ts
// Store per gestire il numero di dadi selezionati nel minigioco
type DiceStore = {
  diceCount: number;
  setDiceCount: (count: number) => void;
};
```

## 📁 4. README parziale nei sotto-folder

Se una cartella (`components/`, `store/`, `utils/`) contiene molti file, puoi aggiungere un piccolo file `README.md` locale che elenca e spiega brevemente i contenuti.

## 🧼 5. Convenzioni di nomi

Mantieni nomi coerenti e descrittivi:

- `lib/ollama.ts` → contiene chiamate API a Ollama
- `store/dice.ts` → Zustand store per il gioco dei dadi
- `components/EventCard.tsx` → UI riutilizzabile per la card evento

---

---

# 📚 Bonus 2 – Per i più temerari: Autenticazione con `better-auth`

Se vuoi sostituire Clerk con una soluzione completamente self-hosted e open-source, puoi usare [`better-auth`](https://www.better-auth.com/), un kit di autenticazione leggero e sicuro basato su JWT e OAuth.

## 🚧 Requisiti

- Un backend custom (es. con Express, Bun, Fastify o Hono)
- Un database (es. PostgreSQL o SQLite) per salvare utenti e sessioni (ti ricordo che Supabase è un PostgreSQL)
- Deployment (opzionale) su piattaforme come Vercel, Render, Railway o self-host

---

## ⚙️ Setup backend `better-auth`

1. Crea un nuovo progetto Node/Bun:

```bash
bun init better-auth-backend
cd better-auth-backend
bun add better-auth express cors dotenv zod
```

2. Crea un file `.env` con:

```env
JWT_SECRET=my-very-secure-key
DATABASE_URL=postgresql://...
```

3. Aggiungi la logica di base nel tuo backend (`index.ts` o `server.ts`):

```ts
import express from "express";
import cors from "cors";
import { authRouter, createBetterAuth } from "better-auth";

const app = express();
app.use(cors());
app.use(express.json());

const auth = createBetterAuth({
  secret: process.env.JWT_SECRET!,
  providers: [
    {
      id: "google",
      clientId: process.env.GOOGLE_CLIENT_ID!,
      clientSecret: process.env.GOOGLE_CLIENT_SECRET!,
      redirectUri: "http://localhost:3000/auth/callback/google",
    },
  ],
});

app.use("/auth", authRouter(auth));

app.listen(3000, () => console.log("Auth server running"));
```

> 🔐 Puoi aggiungere anche login con email/password, magic link o GitHub modificando i provider.

---

## 📱 Setup client React Native

1. Rimuovi Clerk dal progetto.
2. Crea una schermata `app/login.tsx` con redirect al backend:

```tsx
import { Button } from "react-native";
import * as WebBrowser from "expo-web-browser";

export default function Login() {
  const handleLogin = async () => {
    await WebBrowser.openBrowserAsync(
      "http://localhost:3000/auth/login/google"
    );
  };

  return <Button title="Login con Google" onPress={handleLogin} />;
}
```

3. Dopo il login, salva il JWT restituito nel `AsyncStorage` ed usalo nelle richieste autenticati.

---

## 🧠 Vantaggi & svantaggi

✅ Full control (zero lock-in)  
✅ Estensibile, open source, backend-friendly  
❌ Più complesso rispetto a Clerk  
❌ Richiede gestione manuale sessioni, sicurezza e storage

---

> ⚠️ Questo setup è consigliato solo a chi ha esperienza con backend e gestione auth in ambienti reali.

---

## 🔓 Logout e gestione token

Per eseguire il logout, ti basta rimuovere il token JWT salvato nel `AsyncStorage`:

```ts
import AsyncStorage from "@react-native-async-storage/async-storage";

export async function logout() {
  await AsyncStorage.removeItem("auth_token");
}
```

---

## 🔐 Role-Based Access Control (RBAC)

Se vuoi differenziare tra utenti normali e admin:

1. Nella tabella `users` (che gestisci tu nel backend), aggiungi una colonna `role` (`'user'`, `'admin'`, ecc.).
2. Al momento del login, nel backend aggiungi il ruolo nel payload JWT:

```ts
const token = auth.createJWT({ sub: user.id, role: user.role });
```

3. Nel client, decodifica il token e accedi al ruolo:

```ts
import jwtDecode from "jwt-decode";

type Decoded = {
  sub: string;
  role: "user" | "admin";
};

const token = await AsyncStorage.getItem("auth_token");
const decoded = jwtDecode<Decoded>(token!);
```

4. Usa `decoded.role` per proteggere schermate o azioni admin.

---

## 🛡️ Middleware di protezione (es. per le API)

Nel tuo backend, puoi usare il middleware incluso in `better-auth` per proteggere le route:

```ts
app.get("/admin-only", auth.requireRole("admin"), (req, res) => {
  res.send("Accesso solo per admin");
});
```

---

## 📥 Recuperare l’utente loggato

Nel client:

```ts
const token = await AsyncStorage.getItem("auth_token");
const decoded = jwtDecode<{ sub: string; email: string }>(token!);
```

Nel backend:

```ts
app.get("/me", auth.requireAuth, (req, res) => {
  res.json({ user: req.user });
});
```

---

# 📚 Bonus 3 – Tips & Tricks per React Native + Expo

## ⚡️ 10+ consigli pratici per migliorare lo sviluppo

1. **Usa `expo-router` per organizzare meglio il codice**

   - Navigazione basata sui file, simile a Next.js → meno boilerplate.

2. **Abilita tunnel Expo per testing da dispositivi reali**

   - `bunx expo start --tunnel` risolve problemi di rete su Windows o reti universitarie.

3. **Utilizza `useSafeAreaInsets` per gestire bene i margini su iPhone**

   - Importa da `react-native-safe-area-context` per evitare contenuti tagliati.

4. **Installa e configura `expo-dev-client` per testing con moduli nativi**

   - Se usi librerie native (non supportate in Expo Go), puoi creare il tuo dev client.

5. **Organizza la UI con Tailwind via NativeWind**

   - Usa classi CSS-like e componenti responsive senza scrivere stili JS.

6. **Evita crash su Android per immagini remote**

   - Ricorda di impostare `resizeMode`, larghezza e altezza fissa in `<Image>`.

7. **Stato globale semplice? Usa Zustand**

   - Più leggero e leggibile di Redux, meno setup rispetto a Context.

8. **Usa `expo install` invece di `bun add` per pacchetti legati a Expo**

   - Questo installa automaticamente la versione compatibile con il tuo SDK.

9. **Ricarica automatica? Attiva il fast refresh**

   - Premi `r` nel terminale (dev server) oppure shake device → Fast Refresh.

10. **Testa in modalità produzione**

    - Avvia l'app con `bunx expo start --no-dev --minify` per simulare un comportamento reale.

11. **Analizza le performance con React DevTools**

    - Puoi collegarlo anche a un’app RN con `expo-devtools` e vedere la struttura componenti.

12. **Preload font & immagini con `useFonts` e `Asset.loadAsync`**

    - Così eviti glitch visivi all’avvio.

13. **Disabilita warning gialli in dev se ti disturbano troppo**

    ```ts
    import { LogBox } from "react-native";
    LogBox.ignoreAllLogs();
    ```

14. **Ottimizza le FlatList**

    - Usa `initialNumToRender`, `removeClippedSubviews`, `maxToRenderPerBatch`, `windowSize`.

15. **Salva token e dati con `@react-native-async-storage/async-storage`**

    - È la soluzione standard e persistente per piccoli dati utente.

16. **Semplifica la UI con regole 8pt grid**

    - Usa margini/padding in multipli di 8 (es. `p-4`, `mt-2`, `gap-8`) per coerenza visiva.

17. **Evita di nidificare troppi `View`**

    - Tieni la struttura piatta e chiara. Troppe nested view rallentano il rendering.

18. **Sezioni scrollabili? Usa `ScrollView` con `keyboardShouldPersistTaps="handled"`**

    - Così l’utente può chiudere la tastiera toccando fuori.

19. **Carica schermate complesse con `lazy()`**

    - Usa `const EventScreen = lazy(() => import('@/app/events'))` e `Suspense` per lazy loading.

20. **Usa Zustand con slice modulari**

    - Crea file separati per slice (`authSlice`, `eventSlice`) e componi lo store per scalabilità.

21. **Evita re-render non necessari**

    - Usa `useShallow` di Zustand o `React.memo()` sui componenti pesanti.

22. **Carica dati con `useEffect` solo se serve**

    - Evita fetch globali in `layout.tsx` o `index.tsx` se non usati ovunque.

23. **Ottimizza immutabilità**

    - Non modificare direttamente array o oggetti nello stato: sempre creare nuove copie.

24. **Evita animazioni bloccanti**

    - Usa `react-native-reanimated` per animazioni fluide e asincrone.

25. **Modularizza le UI in componenti piccoli**

    - Ogni blocco UI dovrebbe stare in un proprio file, anche se è solo una `Card` o una `Button`.

26. **Definisci una palette colore globale**

    - Dentro `tailwind.config.js`, imposta `colors: { primary: "#xyz", accent: "#abc" }`.

27. **Crea componenti di layout standard**

    - Es. `PageContainer`, `Section`, `Header` per evitare ripetizioni di stili.

28. **Mantieni consistenza nei font**

    - Usa una libreria come `expo-font` per caricare e applicare un font uniforme all’intera app.

29. **Disattiva logging in produzione**

    - Usa `if (__DEV__) console.log()` oppure strumenti come `debug` per evitare leak in release.

30. **Misura le performance con Profiler**
    - Usa il DevTools Profiler (con React DevTools o Flipper) per capire dove avvengono i render più pesanti.

---

# 📚 Bonus 4 – UX/UI Tips Pro

## 🧠 Migliora l’esperienza utente e il design della tua app

1. **Usa animazioni microinterattive**

   - Piccoli movimenti migliorano la percezione dell'app: usa `react-native-reanimated` o `moti`.

2. **Feedback visivo su ogni azione**

   - Loader durante fetch (`ActivityIndicator`), toast o alert al termine di azioni (es. prenotazione).

3. **Bottoni chiari e leggibili**

   - Padding abbondante (`py-3 px-6`), font leggibile (`text-base` o `text-lg`), sfondo/colore contrastato.

4. **Modal per interazioni veloci**

   - Usa modal per modifiche rapide o contenuti temporanei, evitando navigazioni inutili.

5. **Skeleton Placeholder o Blurhash durante il loading**

   - Alternativa più elegante allo spinner vuoto.

6. **Gestione avanzata dei form**

   - Valida con `react-hook-form` + `zod/yup` e mostra errori direttamente sotto i campi.

7. **Tap targets larghi**

   - Almeno `44x44pt` per bottoni, link e icone cliccabili.

8. **Colori accessibili**

   - Rispetta il contrasto WCAG AA o superiore. Usa uno strumento come `contrast-ratio.com`.

9. **Dark mode integrata**

   - Usa `useColorScheme()` per adattare automaticamente palette, sfondi e testi.

10. **Stati vuoti significativi**

    - Illustrazioni, icone e messaggi motivazionali per liste vuote.

11. **Navigazione chiara e coerente**

    - Header con titoli comprensibili, tab bar chiara, breadcrumb se necessario.

12. **Scroll visivamente intuibile**

    - Per scroll orizzontali, mostra parte del contenuto successivo come invito all’interazione.

13. **Onboarding o welcome screen**

    - Mostra un’introduzione solo al primo avvio con `AsyncStorage`.

14. **Gestione esplicita degli errori**

    - Nessun errore deve passare silenziosamente: log, messaggio o fallback UI.

15. **Stile neutro e moderno**
    - `rounded-xl`, `shadow-md`, `bg-white/90`, `text-gray-700`, `gap-4`, emoji o micrografica per calore visivo.



----

## 🪅 UI KIT
- [Componenti iOS 18](https://www.figma.com/design/OJQ4Buz60ScFlQBFeT1AMo/iOS-18-and-iPadOS-18--Community-?node-id=221-56229&p=f&t=YFLOxku6fGcn7BFP-0)
- [Chat app](https://www.figma.com/design/JyfxE8NkkW1j1ZrJc9MSs5/Simple-Chat-App-Design--Community---Copy-?node-id=1-7&p=f&t=YFLOxku6fGcn7BFP-0)
- [Minimal](https://www.figma.com/design/28D5Q9IFheQnB8rx7q6dQb/Minima-App-Design--Community---Copy-?node-id=0-1&p=f&t=YFLOxku6fGcn7BFP-0)
