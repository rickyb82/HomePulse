# HomePulse

App e integrazione FamilOS pubblicate da **RickyB82**.

HomePulse collega direttamente un Oura Gen3 via Bluetooth, accoda i record sul dispositivo fino alla conferma del proprio server FamilOS. Il cardio registrato viene elaborato sull’iPhone anche senza rete, senza account, API o app ufficiale Oura.

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

In Allenamento selezionare l’attività e premere **Avvia cardio registrato offline**. L’app acquisisce nuove tracce e aggiorna il calcolo locale. **Termina** chiude la sessione. Se in Impostazioni è abilitato il cardio anche fuori allenamento, il monitoraggio continua separatamente senza creare sessioni nel diario; altrimenti viene richiesto lo stop nell’anello. I risultati vengono conservati nel server personale. Sul telefono rimangono soltanto la coda non sincronizzata e il buffer cardio corrente: dopo la conferma i payload vengono liberati, senza cache persistenti dei grafici. La sincronizzazione riprende quando il server personale torna raggiungibile.

Emily può leggere i punti intermedi per metrica e intervallo con `allenamento_campioni`, seguendo il cursore tra pagine complete senza ridurre la serie. Il post allenamento richiede una scelta esplicita.

Ogni risultato conserva versione del modello, qualità e stato provvisorio/finale. FamilOS aggiorna atomicamente la stessa registrazione: un invio fuori ordine non sovrascrive revisioni più recenti. Grafici, statistiche, CSV ed Emily riconoscono la provenienza delle stime. I punti mancanti non vengono inventati e l’ora del download non sostituisce quella dei record. `allenamento_grafico` crea il grafico offline nella chat personale con tutte le metriche disponibili e tabella paginata; `allenamento_energia` espone la stima giornaliera con base, attività, totale parziale e copertura. La stima usa MET acquisiti e fasi esplicite senza doppio conteggio; i periodi mancanti non diventano attività inventata. Il cardio esteso è attivo come impostazione iniziale e una disattivazione esplicita viene rispettata.

WHR U2.5.1 è sperimentale sul firmware Gen3 attuale. L’esecuzione è verificata anche su iPhone fisico; qualità e comportamento durante allenamenti reali richiedono verifica separata. L’uscita interpolata a un secondo non equivale a nuove misure indipendenti a quella frequenza. SpO₂, temperatura, respiro e sonno seguono i propri record e percorsi di elaborazione.

## Sonno Gen3 e ripristino

Lo storico viene elaborato in blocchi limitati con18ore di sovrapposizione e checkpoint persistenti. Il formato firmware Gen3 0x76 recupera il periodo a letto usando il mapping nativo dello stesso avvio; fasi e sonno totale rimangono nulli finché la pipeline SleepNet non è integrata. Non vengono stimati da soglie di movimento.

L’avvio cardio prepara il nuovo contesto prima dell’ACK senza sostituire la cattura corrente; un allineamento lungo riprende automaticamente. Il riavvio del segmento non spegne la preferenza di monitoraggio continuo.

I record Gen3 0x6a alimentano la serie respiratoria storica e il riepilogo del periodo Sonno (media dei campioni disponibili, estremi e conteggio). Il mapping UTC resta quello originale del motore. I grafici FamilOS mostrano scale numeriche, unità, date per periodi lunghi e dettagli dei punti. Le serie complete restano disponibili alle analisi.

Emily consulta il sonno con `allenamento_sonno`: orari, durate, fasi disponibili, SpO₂ e respiro del proprio periodo. La data indica la fine del periodo nel fuso Europe/Rome. Il tool è riservato al proprietario in chat non condivise; gli strumenti esplicitamente spenti e i permessi restano rispettati.

## Dati privati

Le fixture pubbliche sono sintetiche. Non sono inclusi immagini della configurazione personale, archivi sanitari, credenziali, token, associazioni o dati di un anello specifico. I componenti esterni mantengono autori e licenze originali; il runtime PyTorch viene fornito separatamente con la propria licenza nella build fisica.
