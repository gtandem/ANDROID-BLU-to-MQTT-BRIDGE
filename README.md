# Android BLE to MQTT Bridge

App Android per leggere dispositivi energetici compatibili via Bluetooth Low Energy (BLE), visualizzare misure e storico, e scambiare i dati tramite MQTT. Supporta anche una modalità ricevitore MQTT per visualizzare i dati raccolti da un altro telefono.

## Funzioni

- Scansione BLE e configurazione dei dispositivi supportati.
- Visualizzazione delle misure, dello stato online/offline e dello storico.
- Pubblicazione dei dati su MQTT con topic configurabili per dispositivo.
- Modalità ricevitore in sola lettura: riceve configurazione e dati dal broker.
- Allarmi configurabili per dispositivo, con notifiche Android.
- Monitoraggio in background per mantenere la raccolta BLE/MQTT e gli allarmi.
- Esportazione e importazione dei dati e delle impostazioni dell’app.

## Prima configurazione

1. Concedi i permessi Bluetooth e posizione richiesti da Android per la scansione BLE. Per la raccolta a schermo spento, consenti anche la posizione in background se richiesta.
2. Apri il menu **Impostazioni MQTT** e inserisci host, porta, credenziali (se richieste), topic base e intervallo di invio.
3. Avvia una scansione BLE, seleziona il dispositivo e inserisci la chiave di decrittazione quando necessaria.
4. Apri il dispositivo per consultare le misure, lo storico e configurare eventuali allarmi.

I nomi delle voci possono variare leggermente in base alla versione dell’app.

## Trasmettitore e ricevitore

### Trasmettitore

Configura il broker MQTT e registra i dispositivi BLE sul telefono trasmettitore. Nelle impostazioni MQTT del dispositivo puoi scegliere un nome leggibile per il topic; se non ne imposti uno, viene usato l’indirizzo MAC.

### Ricevitore MQTT

In **Impostazioni MQTT**, attiva la modalità ricevitore e salva la configurazione. Il ricevitore si collega al broker e visualizza i dati pubblicati dal trasmettitore. Non effettua la scansione BLE dei dispositivi remoti.

### Sincronizzazione con QR

1. Sul trasmettitore apri **Impostazioni MQTT** e mostra il QR di configurazione.
2. Sul ricevitore apri **Impostazioni MQTT**, scegli la scansione QR e inquadra il codice.
3. Dopo l’importazione, il ricevitore si connette al broker e legge i dati MQTT del trasmettitore.

Il QR include parametri del broker e configurazione dei dispositivi, comprese le chiavi BLE e, se configurate, le credenziali MQTT. Mostralo solo a dispositivi fidati. Le misure e lo storico non sono contenuti nel QR: arrivano attraverso MQTT.

## Topic MQTT

Il topic base è configurabile. Ogni dispositivo usa il nome MQTT scelto nelle sue impostazioni; se il nome è vuoto, viene usato il suo MAC address. Il ricevitore deve usare il broker e la configurazione del trasmettitore per ascoltare gli stessi topic.

## Allarmi e funzionamento in background

Per continuare a raccogliere dati e ricevere allarmi quando l’app non è in primo piano, lascia attivo il servizio di monitoraggio. Android mostra una notifica persistente mentre il servizio lavora. Concedi il permesso notifiche e, se il telefono limita l’attività in background, escludi l’app dall’ottimizzazione batteria.

La chiusura della schermata o la rimozione dai recenti non equivale necessariamente all’arresto del monitoraggio. Per interromperlo completamente usa **Esci** nell’app. Il comportamento delle notifiche dipende anche dai permessi e dalle limitazioni del produttore del telefono.

## Risoluzione problemi

- **Il dispositivo BLE non appare:** attiva Bluetooth e posizione, concedi i permessi e avvicina il telefono al dispositivo.
- **Le misure non si aggiornano:** controlla lo stato BLE sul trasmettitore, il broker MQTT e la connessione di rete; verifica che l’intervallo di invio sia maggiore di zero.
- **Il ricevitore non mostra dati:** controlla che trasmettitore e ricevitore usino lo stesso broker e topic e che il dispositivo sia abilitato all’invio MQTT.
- **Gli allarmi non arrivano a schermo spento:** verifica il permesso notifiche, le regole di allarme e le impostazioni batteria del telefono.
- **QR non letto:** aumenta la luminosità dello schermo trasmettitore, mantieni il codice intero nel riquadro e prova a variare la distanza.

## Privacy e sicurezza

Le credenziali MQTT e le chiavi BLE sono dati sensibili. Proteggi il telefono, non condividere QR o archivi esportati con persone non fidate e usa un broker protetto da autenticazione e trasporto cifrato quando disponibile.
