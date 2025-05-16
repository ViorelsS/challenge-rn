# 🦙 Integrazione di Ollama in React Native

Questa guida fornisce tutto i basics per sfruttare i modelli di Ollama in un’app React Native, con snippet pronti all’uso.

---

> 🚀 **Obiettivi**
>
> * Configurare Ollama in locale
> * Creare e configurare un progetto React Native
> * Eseguire chiamate **complete** e **streaming**
> * Costruire un’interfaccia di chat interattiva

---

## 🛠 Prerequisiti

| Software         | Versione minima | Installazione                                                                                     |
| ---------------- | --------------- | ------------------------------------------------------------------------------------------------- |
| Node.js          | 20.x            | [https://nodejs.org/](https://nodejs.org/)                                                        |
| React Native     | 0.71+           | `npm install -g react-native-cli`                                                                 |
| Docker *(opt)*   | 20.x            | [https://www.docker.com/](https://www.docker.com/)                                                |
| Homebrew *(mac)* | /               | `/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"` |

## 1️⃣ Installazione di Ollama

```bash
# macOS
brew install ollama

# Windows/Linux
# Scarica il binario da https://ollama.com/download e posizionalo in PATH

# Verifica
ollama --version
```

> 💡 **Tip**: utilizza Docker se preferisci un container isolato:
>
> ```bash
> ```

docker run -it --rm -p 11434:11434 ollama/ollama serve

````

---

## 2️⃣ Avvio del server locale

```bash
# Avvia Ollama in modalità server
ollama serve --port 11434
````

* **Endpoint di default**: `http://localhost:11434`
* **Modalità HTTPS**: aggiungi `--tls-cert` e `--tls-key`

> ⚠️ **Attenzione**: Proteggi l’endpoint in produzione con autenticazione/token.

---

## 3️⃣ Configurazione nel progetto Expo

```bash
npm install axios eventsource
```
> * `axios`: semplifica le chiamate HTTP
> * `react-native-sse` o `eventsource`: supporto allo streaming

---

## 4️⃣ Servizio HTTP per Ollama

Crea il file `src/services/ollama.js` con il seguente contenuto:

```js
// src/services/ollama.js
import axios from 'axios';

const BASE_URL = 'http://localhost:11434/v1';

/**
 * Richiesta di completamento intera
 */
export async function complete({ prompt, model = 'llama2' }) {
  const { data } = await axios.post(
    `${BASE_URL}/completions`,
    { model, prompt },
    { headers: { 'Content-Type': 'application/json' } }
  );
  return data.choices[0].text;
}

/**
 * Completamento in streaming
 * @param {Function} onUpdate callback per ogni chunk
 */
export function streamComplete({ prompt, onUpdate, model = 'llama2' }) {
  const source = new EventSource(
    `${BASE_URL}/completions?stream=true`,
    { 
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({ model, prompt })
    }
  );

  source.onmessage = (e) => {
    const json = JSON.parse(e.data);
    if (json.choices?.[0]?.delta?.content) {
      onUpdate(json.choices[0].delta.content);
    }
  };
  source.onerror = () => source.close();
  return () => source.close();
}
```

> 🔍 **Nota**: React Native NON supporta nativamente `EventSource`. Puoi usare il pacchetto `react-native-sse`:
>
> ```bash
> npm install react-native-sse
> ```

---

## 5️⃣ Componente Chat Pronto all’uso

Crea `src/screens/ChatScreen.js`:

```jsx
// src/screens/ChatScreen.js
import React, { useState, useRef } from 'react';
import { StyleSheet, View, TextInput, Button, ScrollView, Text } from 'react-native';
import { complete, streamComplete } from '../services/ollama';

export default function ChatScreen() {
  const [prompt, setPrompt] = useState('');
  const [messages, setMessages] = useState([]);
  const abortRef = useRef(null);

  const handleSend = async () => {
    if (!prompt.trim()) return;
    const userMsg = { role: 'user', text: prompt };
    setMessages(prev => [...prev, userMsg]);
    setPrompt('');

    // Messaggio placeholder
    const assistantMsg = { role: 'assistant', text: '' };
    setMessages(prev => [...prev, assistantMsg]);

    // Streaming
    abortRef.current = streamComplete({ prompt, onUpdate: chunk => {
      assistantMsg.text += chunk;
      setMessages(prev => [...prev.slice(0, -1), assistantMsg]);
    }});
  };

  return (
    <View style={styles.container}>
      <ScrollView style={styles.chat} contentContainerStyle={styles.chatContent}>
        {messages.map((msg, idx) => (
          <View key={idx} style={[styles.bubble, msg.role === 'user' ? styles.userBubble : styles.assistantBubble]}>
            <Text style={styles.text}><Text style={styles.bold}>{msg.role}:</Text> {msg.text}</Text>
          </View>
        ))}
      </ScrollView>

      <View style={styles.inputRow}>
        <TextInput
          style={styles.input}
          value={prompt}
          onChangeText={setPrompt}
          placeholder="Scrivi un prompt..."
        />
        <Button title="Invia" onPress={handleSend} />
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, backgroundColor: '#f0f4f8' },
  chat: { flex: 1, padding: 12 },
  chatContent: { paddingBottom: 12 },
  bubble: { marginVertical: 4, padding: 10, borderRadius: 8 },
  userBubble: { backgroundColor: '#d1e7dd', alignSelf: 'flex-end' },
  assistantBubble: { backgroundColor: '#fff3cd', alignSelf: 'flex-start' },
  text: { fontSize: 16 },
  bold: { fontWeight: '600' },
  inputRow: { flexDirection: 'row', padding: 8, borderTopWidth: 1, borderColor: '#ccc' },
  input: { flex: 1, backgroundColor: '#fff', borderRadius: 4, padding: 10, marginRight: 8 }
});
```

---

## 🎨 Personalizzazioni e Best Practice

* **Autenticazione**: usa token JWT e middleware Express per proteggere l’API.
* **Caching**: implementa cache LRU per risposte frequenti.
