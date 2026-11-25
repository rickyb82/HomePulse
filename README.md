# HomePulse

App e integrazione FamilOS pubblicate da **RickyB82**.

HomePulse collega direttamente un Oura Gen3 via Bluetooth, conserva i record sul dispositivo e sincronizza con il proprio server FamilOS. Il cardio registrato viene elaborato sull’iPhone anche senza rete, senza account, API o app ufficiale Oura.

## Componenti

- `ios/HomePulse`: app SwiftUI con Bluetooth, SQLite, allenamenti, diario offline, timer, batteria e notifiche locali.
- `android/HomePulse`: prototipo Android del ponte e dell’interfaccia. Il nuovo calcolo locale WHR è attualmente implementato su iPhone.
- `familos/backend`: servizi, API, strumenti Emily e test del modulo Allenamento.
- `familos/frontend`: dashboard, grafici, schede, calendario, profilo e interfaccia Sonno.
- `familos/extras`: worker storico privato e integrazione dell’installatore Orbita.

Nessun profilo, palestra o appuntamento personale è preconfigurato. Ogni proprietario imposta server e dati nell’app.

## Compilazione e installazione

Le istruzioni iPhone sono in [ios/HomePulse/README.md](ios/HomePulse/README.md). Il simulatore verifica logica e interfaccia. Per il calcolo su iPhone occorre fornire privatamente il modello WHR U2.5.1 e il runtime PyTorch descritti nel documento. APK, librerie proprietarie, pesi, chiavi, certificati e archivi personali sono esclusi dal repository. Nessuna chiave modello viene distribuita nell’app.

Il modulo richiede [FamilOS](https://github.com/rickyb82/FamilOS/tree/codex/allenamento). [familos/SOURCE.json](familos/SOURCE.json) identifica la revisione completa e i file corrispondenti: `familos/backend`, `familos/frontend` e `familos/extras` mantengono i percorsi del progetto ospitante. Allenamento si abilita come modulo opzionale. Autenticazione e registrazione dei moduli appartengono alla base FamilOS completa nel ramo indicato.

## Cardio registrato

In Allenamento selezionare l’attività e premere **Avvia cardio registrato offline**. L’app acquisisce nuove tracce e aggiorna il calcolo locale. **Termina** chiude la sessione e richiede lo stop nell’anello. I risultati rimangono nel diario del telefono; la sincronizzazione riprende quando il server personale torna raggiungibile.

Ogni risultato conserva versione del modello, qualità e stato provvisorio/finale. FamilOS aggiorna atomicamente la stessa registrazione: un invio fuori ordine non sovrascrive revisioni più recenti. Grafici, statistiche, CSV ed Emily riconoscono la provenienza delle stime. I punti mancanti non vengono inventati e l’ora del download non sostituisce quella dei record.

WHR U2.5.1 è sperimentale sul firmware Gen3 attuale. L’esecuzione è verificata anche su iPhone fisico; qualità e comportamento durante allenamenti reali richiedono verifica separata. L’uscita interpolata a un secondo non equivale a nuove misure indipendenti a quella frequenza. SpO₂, temperatura, respiro e sonno seguono i propri record e percorsi di elaborazione.

## Dati privati

Le fixture pubbliche sono sintetiche. Non sono inclusi immagini della configurazione personale, archivi sanitari, credenziali, token, associazioni o dati di un anello specifico. I componenti esterni mantengono autori e licenze originali; il runtime PyTorch viene fornito separatamente con la propria licenza nella build fisica.
