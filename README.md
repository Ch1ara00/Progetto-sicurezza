# Progetto – Password Manager Sicuro

## Obiettivo del progetto

L'applicazione permette di gestire in modo sicuro le proprie credenziali personali. Ogni password salvata viene cifrata localmente nel browser dell’utente utilizzando tecniche crittografiche avanzate. La struttura dell’app è completamente client-side: non vengono effettuate comunicazioni esterne né vengono salvati dati in chiaro o su server remoti. Questo approccio zero-knowledge garantisce un livello di sicurezza molto elevato.

## Sicurezza e Crittografia

Per proteggere le informazioni, sono state implementate le seguenti tecniche:

- **AES-256-GCM** per la cifratura simmetrica delle password salvate.
- **PBKDF2** con 100.000 iterazioni per derivare la chiave a partire dalla password master dell’utente.
- **SHA-256** per l’hashing sicuro della password master (che non viene mai salvata in chiaro).

Le operazioni crittografiche sono gestite tramite la **Web Crypto API**, utilizzando le funzionalità di sicurezza native del browser.

Ogni voce salvata è cifrata singolarmente, utilizzando un Initialization Vector (IV) casuale. In questo modo, anche password simili o identiche risultano cifrate in modo completamente diverso.

## Funzionalità implementate

L’applicazione permette di:

- Accedere con una password master configurata dall’utente alla prima apertura.
- Aggiungere, modificare ed eliminare password in modo sicuro.
- Visualizzare le password cifrate solo dopo l'autenticazione.
- Generare password robuste tramite un generatore personalizzabile.
- Valutare la forza di ogni password tramite la libreria **zxcvbn** sviluppata da Dropbox.
- Cercare in tempo reale tra le credenziali salvate grazie a una funzione di ricerca dinamica.

Inoltre, per evitare la perdita dei dati, è presente un sistema di **backup locale** (esportazione sicura dei metadati), anche se le password vere e proprie non vengono mai esportate né salvate in chiaro.

## Interfaccia utente e accessibilità

L’interfaccia è stata progettata con attenzione all’esperienza d’uso. Sono stati utilizzati elementi semantici di **HTML5**, con supporto alla navigazione da tastiera, nel rispetto delle linee guida **WCAG** sull’accessibilità.

L’interazione è semplice e intuitiva. L’app fornisce feedback visivi immediati (messaggi di stato, indicatori di forza delle password, icone per il copia/incolla e toggle visibilità).

## Architettura e struttura del codice

Il progetto è strutturato in moduli JavaScript indipendenti, ognuno con responsabilità ben definite:

- **Modulo crittografico**: contiene le funzioni di hashing, derivazione chiave, cifratura e decifratura.
- **Modulo di autenticazione**: gestisce il login, il logout automatico dopo 30 minuti e la prima configurazione.
- **Modulo password**: si occupa della gestione operativa delle credenziali (aggiunta, eliminazione, copia, visualizzazione).
- **Modulo generatore**: produce password sicure, analizza la loro robustezza e aggiorna gli indicatori grafici.

Per semplicità, nella demo è stato utilizzato il **localStorage** del browser per salvare temporaneamente i dati. Nei commenti del codice sono evidenziate le differenze rispetto a un’implementazione reale, in cui sarebbe necessario un backend sicuro con database cifrato.

## Aspetti di sicurezza aggiuntivi

Oltre alla crittografia, sono state integrate alcune misure difensive:

- Logout automatico dopo inattività prolungata (30 minuti).
- Validazione e sanitizzazione degli input, per prevenire attacchi **XSS**.
- Gestione sicura degli errori, in particolare nelle operazioni crittografiche (es. chiavi errate o dati corrotti).

## Limiti e sviluppi futuri

Trattandosi di una demo, l'applicazione presenta alcune limitazioni:

- I dati sono salvati in locale e non sono sincronizzabili tra dispositivi.
- L’app non è ancora protetta da **HTTPS**, essenziale in ambienti di produzione.
- Manca un sistema di autenticazione a due fattori (**2FA**).
- Non è ancora presente una gestione avanzata delle sessioni (es. **WebAuthn** o accesso biometrico).

Miglioramenti futuri previsti:

- Sviluppare un backend sicuro con salvataggio cifrato.
- Aggiungere sincronizzazione multi-dispositivo.
- Integrare autenticazione biometrica con WebAuthn.
- Introdurre un sistema di audit trail per il monitoraggio delle attività.
