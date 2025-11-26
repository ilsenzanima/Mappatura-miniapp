## Problema e ipotesi
- In Telegram, l’uso di input nascosti/label e click programmati viene spesso trattato come non-azione utente, portando alla galleria o al blocco.
- Nel codice attuale, l’input file principale ha `pointer-events: none` e i bottoni nativi del file-input sono nascosti: questo può impedire l’“attivazione utente” richiesta dalla WebView.

## Modifiche proposte
1) Ripristinare l’input nativo visibile per la fotocamera
- Rimuovere `pointer-events: none` dal file input e non nascondere più i bottoni nativi del file input.
- Impostare attributi UA-specifici:
  - iOS: `accept="image/*;capture=camera"` e `capture="camera"`
  - Android: `accept="image/*"` e `capture="environment"`
- Mostrare un solo input fotocamera (“Scatta o seleziona”) che l’utente può toccare direttamente (azione utente certa).

2) Abilitare overlay `getUserMedia` anche in Telegram
- Rimuovere la condizione che lo disabilita in Telegram e provarlo prima: se otteniamo lo stream, non chiede permessi ad ogni scatto (finché la pagina resta aperta).
- Aggiungere un pulsante “Scatta un’altra” nell’overlay per catture successive senza nuovo permesso.

3) Fallback pulito
- Se `getUserMedia` fallisce (permesso negato o non supportato), lascio l’input nativo visibile come fallback.
- Mantengo la pipeline di resize/EXIF esistente.

## Dettagli tecnici (file interessati)
- `app.html` (ex `index.html`):
  - CSS: rimuovere regole che disabilitano interazione con `input[type=file]`.
  - HTML: lasciare un input file fotocamera visibile con `accept/capture` corretti.
  - JS: tentare overlay `getUserMedia` anche in Telegram; se stream ok, permettere scatti multipli senza chiedere permesso ogni volta; altrimenti usare input.

## Verifica
- Telegram iOS/Android: tocco sull’input apre fotocamera ove supportato; se non supportato, overlay getUserMedia tenta lo stream; altrimenti galleria.
- Browser: overlay attivo, niente richieste ripetute finché resta aperto.

## Risultato atteso
- Maggiore compatibilità in Telegram grazie all’input visibile (azione utente riconosciuta) e al tentativo overlay universale.
- Meno prompt permessi in browser con stream persistente.

Confermi che procedo con queste modifiche mirate?