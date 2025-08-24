# Report Lavoro - Mini App Telegram

Una mini-app Telegram avanzata per la raccolta e l'invio di report di lavoro con integrazione Google Sheets e webhook N8N.

## 🚀 Caratteristiche Principali

### ✅ Funzionalità Core
- **Form Completo**: Raccolta dati con foto, piano, numero identificativo, supporto, dimensioni e attraversamenti
- **Validazione Avanzata**: Controllo in tempo reale di tutti i campi con messaggi di errore specifici
- **Integrazione Telegram**: Piena compatibilità con Telegram WebApp API
- **Invio Sicuro**: Trasmissione dati via webhook N8N con retry automatico

### 🔧 Funzionalità Avanzate
- **Sistema di Cache**: Cache intelligente per dati Google Sheets con scadenza automatica
- **Retry Logic**: Fino a 3 tentativi automatici per operazioni di rete fallite
- **Gestione Offline**: Rilevamento stato connessione e uso cache offline
- **Loading States**: Indicatori visivi per tutte le operazioni asincrone
- **Debouncing**: Prevenzione azioni multiple rapide

### ♿ Accessibilità
- **ARIA Labels**: Supporto completo per screen reader
- **Focus Management**: Navigazione ottimizzata da tastiera
- **Skip Links**: Collegamenti rapidi per navigazione
- **Messaggi Live**: Feedback in tempo reale per utenti con disabilità

## 🛠️ Configurazione

### Prerequisiti
1. Account Telegram con bot configurato
2. Google Sheets pubblici per dati dropdown
3. Webhook N8N configurato per ricezione dati

### Setup
1. Modifica la variabile `N8N_WEBHOOK_URL` in `index.html` con il tuo URL webhook
2. Verifica gli URL dei Google Sheets nelle variabili:
   - `PIANO_SHEET_URL`
   - `SUPPORTO_SHEET_URL` 
   - `ATTRAVERSAMENTI_SHEET_URL`
3. Carica il file su un server web o hosting
4. Configura il bot Telegram per puntare alla tua URL

## 📊 Struttura Dati Output

I dati vengono inviati in formato JSON. **Per ogni attraversamento viene creata una riga separata** in Google Sheets con questa struttura:

```json
{
  "photo": "data:image/jpeg;base64,...",
  "piano": "Piano selezionato",
  "numero": 123,
  "supporto": "Tipo supporto",
  "dimensioniCm": "10x20",
  "attraversamento_tipo": "Tipo attraversamento",
  "attraversamento_quantita": 5,
  "attraversamento_dimensioni": "15x25",
  "riga_numero": 1,
  "totale_attraversamenti": 3,
  "notes": "Note aggiuntive",
  "telegramUser": { /* Dati utente Telegram */ },
  "timestamp": "2024-01-01T12:00:00.000Z"
}
```

**Esempio**: Se un report ha 3 attraversamenti diversi, verranno inviati 3 payload separati, creando 3 righe in Google Sheets, ognuna con gli stessi dati base ma con attraversamento diverso.

## 🔒 Sicurezza

- Validazione client-side completa
- Controllo formato file immagine
- Gestione errori robusta
- Timeout protection per operazioni bloccate
- Sanitizzazione input utente

## 🌐 Compatibilità

- ✅ Telegram WebApp (iOS/Android)
- ✅ Browser moderni (Chrome, Firefox, Safari, Edge)
- ✅ Dispositivi mobile e desktop
- ✅ Modalità offline (con limitazioni)

## 📱 Utilizzo

1. Apri la mini-app da Telegram
2. Scatta/seleziona una foto
3. Compila tutti i campi obbligatori
4. Aggiungi attraversamenti necessari
5. Aggiungi note opzionali
6. Premi "Invia Dati"
7. L'app si chiuderà automaticamente dopo invio riuscito

## 🐛 Troubleshooting

### Errori Comuni
- **"URL webhook non configurato"**: Modifica `N8N_WEBHOOK_URL` in `index.html`
- **"Errore nel caricamento dei dati"**: Verifica connessione e URL Google Sheets
- **"Impossibile inviare: connessione offline"**: Controlla connessione internet

### Debug
Apri Developer Tools del browser per vedere log dettagliati degli errori.

## 📄 Licenza

Questo progetto è rilasciato sotto licenza MIT.

## 🤝 Contributi

I contributi sono benvenuti! Apri una issue o invia una pull request.

---

**Sviluppato con ❤️ per ottimizzare i processi di reporting sul campo**
