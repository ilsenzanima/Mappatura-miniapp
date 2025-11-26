## Obiettivi
- Aggiungere lettura utente Telegram e messaggio personalizzato di benvenuto nella pagina per Telegram.
- Integrare funzioni WebApp utili (MainButton, tema, HapticFeedback, CloudStorage) nella pagina ponte.

## Implementazione (pagina ponte `index.html`)
- Includere `https://telegram.org/js/telegram-web-app.js`.
- Inizializzare WebApp:
  - `const tg = window.Telegram.WebApp; tg.ready(); tg.expand();`
  - Applicare tema: usare `tg.themeParams` per impostare variabili CSS (bg, text, button).
- Benvenuto personalizzato:
  - Leggere `tg.initDataUnsafe?.user`.
  - Mostrare “Benvenuto, <nome>!” con fallback (“Benvenuto!”) se non presente.
- Pulsante per aprire nel browser:
  - Calcolare `const absoluteUrl = new URL('./app.html', location.href).toString();`
  - UI: mantenere un pulsante HTML e configurare anche `tg.MainButton`:
    - `tg.MainButton.setText('Apri nel browser'); tg.MainButton.show();`
    - `tg.MainButton.onClick(() => tg.openLink(absoluteUrl));`
    - Fallback: se `tg` non disponibile, `window.open(absoluteUrl, '_blank')`.
- HapticFeedback:
  - Alla pressione del pulsante: `tg.HapticFeedback?.impactOccurred('soft');`
- CloudStorage (facoltativo subito, predisposto):
  - Salvare un flag “opened_app” (`tg.CloudStorage.setItem('opened_app', Date.now().toString())`) e leggere in avvio.
  - Mostrare un messaggio “Ultima apertura: …” se disponibile.
- BackButton (facoltativo):
  - `tg.BackButton.hide()` nella pagina ponte; si potrà usare quando aggiungeremo più sezioni.

## Struttura File
- Rinominare l’attuale `index.html` completo in `app.html` (resta la pagina operativa da browser).
- Nuova `index.html` (pagina ponte Telegram) con:
  - Messaggio di benvenuto personalizzato
  - Pulsante “Apri nel browser” (HTML + MainButton)
  - Tema Telegram applicato

## Verifica
- In Telegram: aprire `index.html` → visualizzare benvenuto con nome utente e tema nativo → pulsante apre il browser sull’`app.html`.
- In browser: aprire `index.html` → se `tg` non presente, mostra pulsante HTML e apre `app.html` in nuova scheda.

## Estensioni possibili
- `sendData` al bot per tracciare aperture.
- `enableClosingConfirmation()` su `app.html` quando c’è form non inviato.
- `CloudStorage` per bozza dati.
- `MainButton` dinamico (disabilitato durante operazioni).

Confermi che procedo a rinominare `index.html` → `app.html` e a creare la nuova `index.html` con benvenuto e MainButton?