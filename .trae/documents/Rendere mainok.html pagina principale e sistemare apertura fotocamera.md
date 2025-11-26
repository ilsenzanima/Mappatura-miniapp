## Obiettivi
- Impostare `mainok.html` come pagina principale.
- Correggere il comportamento del pulsante Fotocamera che apre la galleria su alcuni dispositivi.
- Applicare miglioramenti mirati al codice (struttura, affidabilità immagini, UX).

## Stato Attuale (verificato)
- HTML presenti: `index.html`, `mainok.html`, `maintest.html`, `main2.html`.
- Fotocamera e galleria sono gestite in `mainok.html` con input temporanei:
  - Fotocamera: `tempInput.accept = 'image/*'` + `tempInput.capture = 'environment'` (`c:\Users\dagos\Mappatura miniapp\mainok.html:754–758`).
  - Galleria: input senza `capture` (`c:\Users\dagos\Mappatura miniapp\mainok.html:803–809`).
- Nessun `navigator.mediaDevices.getUserMedia` né Cordova.

## Modifiche Proposte
### 1) Rendere `mainok.html` l’entry point
- Rinominare `mainok.html` in `index.html`.
- Spostare gli altri file HTML in `pages_backup/` (temporaneo): `index.html` (vecchio), `maintest.html`, `main2.html`.

### 2) Forzare apertura fotocamera sui dispositivi supportati
- Impostare attributi più compatibili:
  - Su iOS: `capture="camera"` (maggiore compatibilità con WebView/Telegram), oltre a `accept="image/*;capture=camera"`.
  - Su Android: mantenere `capture="environment"` per la camera posteriore.
- Applicare via `setAttribute` oltre all’assegnazione diretta, per massimizzare la compatibilità:
  - In `mainok.html` aggiornare la creazione dell’input fotocamera (blocco `setupCameraButtons`):
    - `tempInput.setAttribute('accept', 'image/*;capture=camera')` su iOS, altrimenti `image/*`.
    - `tempInput.setAttribute('capture', isIOS ? 'camera' : 'environment')`.
- Aggiungere rilevamento ambiente (iOS/Android + Telegram WebApp) con `navigator.userAgent` e presenza di `window.Telegram?.WebApp`.
- Messaggistica chiara: se il browser ignora `capture`, mostrare una nota all’utente e aprire la galleria come fallback.

### 3) Miglioramenti immagine
- Correzione orientamento EXIF: quando si ricampiona su canvas, l’orientamento può perdere la rotazione corretta. Migliorare `resizeImage(...)` (`c:\Users\dagos\Mappatura miniapp\mainok.html:508–586`) usando:
  - `createImageBitmap(file, { imageOrientation: 'from-image' })` quando supportato, o
  - lettura dell’orientamento EXIF e applicazione di `ctx.transform(...)`.
- Uniformare qualità/limiti: soglia ridimensionamento coerente tra fotocamera e galleria (es. >5MB) e qualità JPEG (0.8–0.85).

### 4) Pulizia e struttura del codice
- Evitare duplicazioni tra HTML varianti: mantenere solo la versione consolidata (`mainok.html`) e spostare CSS/JS in file separati (`styles.css`, `script.js`) per mantenibilità.
- Centralizzare funzioni comuni (`resizeImage`, assegnazione a `photoInput.files`, anteprima) e riusarle.
- Validazione form: assicurarsi che il campo `photo` sia presente prima dell’invio e gestire meglio lo stato pulsante submit.

## Dettagli Tecnici da Applicare
- `setupCameraButtons` in `mainok.html` (`c:\Users\dagos\Mappatura miniapp\mainok.html:742–799`):
  - Inserire rilevamento iOS/Android.
  - Applicare `setAttribute('capture', ...)` e `setAttribute('accept', ...)` in base al dispositivo.
  - Mantenere la pipeline esistente di resize → `DataTransfer` → trigger `change`.
- `setupImagePreview` già adeguato (`c:\Users\dagos\Mappatura miniapp\mainok.html:857–880`).
- `resizeImage(...)` aggiornare per orientamento.

## Verifica
- Testare su:
  - Android Chrome: pulsante Fotocamera deve aprire la camera posteriore.
  - iOS Safari/Telegram WebApp: verificare che `capture='camera'` proponga scatto, altrimenti mostrare messaggio fallback.
- Verificare invio al webhook N8N con immagini grandi e ridimensionamento.
- Controllare anteprima immagine e validazione form.

## Output Atteso
- `mainok.html` diventa la pagina principale accessibile.
- Pulsante Fotocamera apre la camera ove supportato; altrimenti fallback chiaro.
- Immagini inviate con orientamento corretto e dimensioni ragionevoli.

Confermi che posso applicare queste modifiche?