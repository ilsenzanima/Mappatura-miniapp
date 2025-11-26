## Cosa è possibile
- Per limiti di sicurezza della WebView di Telegram, non si può forzare l’apertura direttamente nel browser di sistema. `Telegram.WebApp.openLink(...)` apre l’in-app browser (Custom Tabs su Android, SFSafariViewController su iOS).
- Possiamo però facilitare il passaggio al browser principale con UX e azioni alternative.

## Modifiche Proposte (pagina ponte `index.html`)
1) Aggiungere un secondo pulsante “Copia link”
- Copia negli appunti l’URL assoluto di `app.html` (`navigator.clipboard.writeText(appUrl)`), con messaggio di conferma (toast/testo informativo).
- Utile se l’utente vuole incollare manualmente nel browser principale.

2) Aggiungere istruzioni contestuali
- Testo di aiuto: “Dopo l’apertura in Telegram, usa ‘Apri in Safari/Apri in Chrome’ dal menu in alto per spostarti nel browser principale”.
- Mostrare il testo sempre sotto ai pulsanti.

3) Link visibile aggiuntivo
- Inserire un `<a href="app.html" target="_blank" class="btn">Apri link</a>` come alternativa al bottone.
- Su Android, il long-press su link spesso offre l’azione “Apri con Chrome” (dipende dalla versione).

4) Haptic feedback e MainButton
- Conservare l’attuale `MainButton` (Apri nel browser) e aggiungere haptic anche per “Copia link”.

## Implementazione Tecnica
- In `index.html`:
  - Aggiungere pulsante “Copia link” e handler `clipboard`.
  - Aggiungere un `<a>` con `href` su `app.html` (absolute URL) e classe `.btn`.
  - Inserire testo di help sotto i pulsanti.
  - Mostrare un piccolo messaggio “Link copiato negli appunti” quando si copia.

## Verifica
- In Telegram: “Apri nel browser” apre l’in-app viewer; il testo guida spiega come spostarsi al browser principale; “Copia link” funziona.
- In browser: pulsanti e link aprono `app.html` normalmente.

Procedo ad aggiornare `index.html` con questi elementi?