# BLU-to-MQTT-BRIDGE

**App Android gratuita per trasformare un telefono in un bridge Bluetooth Low Energy (BLE) → MQTT.**

Legge i dati dei dispositivi energetici compatibili, visualizza misure e storico e pubblica i dati su un broker MQTT, per integrarli in **Home Assistant** e altri sistemi domotici.

Supporta anche una **modalità ricevitore MQTT**, che permette di visualizzare su un secondo telefono i dati raccolti dal trasmettitore.

## Compatibilità

Il progetto è rivolto principalmente al monitoraggio di dispositivi **Victron Energy compatibili** tramite Bluetooth Low Energy.

La compatibilità dipende dal modello, dai dati BLE disponibili e dalle funzionalità implementate nell’app. Il riferimento a Victron Energy **non implica il supporto di tutti i prodotti del marchio**.

## Funzionalità

- **Scansione BLE** e configurazione dei dispositivi supportati.
- **Monitoraggio** delle misure, dello stato online/offline e dello storico.
- **Pubblicazione MQTT** con topic configurabili per dispositivo.
- **Modalità ricevitore** per visualizzare i dati raccolti da un altro telefono.
- **Allarmi configurabili** con notifiche Android.
- **Monitoraggio in background** per continuare la raccolta dei dati e la gestione degli allarmi.
- **Configurazione tramite QR** per collegare trasmettitore e ricevitore.
- **Importazione ed esportazione** dei dati e delle impostazioni dell’app.

## Come funziona

```text
Dispositivo energetico compatibile
              │
         Bluetooth BLE
              ▼
     Telefono Android
     Modalità trasmettitore
              │
             MQTT
              ▼
         Broker MQTT
              │
       ┌──────┴─────────┐
       ▼                ▼
 Home Assistant    Secondo telefono
 o altri sistemi   Modalità ricevitore
 domotici
```

Il trasmettitore raccoglie i dati via BLE e li pubblica sul broker MQTT. Gli altri sistemi possono utilizzare i dati configurando la ricezione dei topic e l’interpretazione dei messaggi.

## Requisiti

- Un telefono Android con Bluetooth per la modalità trasmettitore.
- Un dispositivo energetico compatibile con le funzionalità dell’app.
- Un broker MQTT raggiungibile per l’invio e la ricezione dei dati tramite MQTT.
- Gli eventuali parametri di autenticazione del broker.
- La chiave di decrittazione BLE del dispositivo, quando richiesta.

## Prima configurazione

1. **Concedi i permessi richiesti.**  
   Abilita Bluetooth e concedi i permessi Bluetooth e posizione richiesti dall’app e dalla versione di Android. Per la raccolta a schermo spento, consenti anche la posizione in background, se richiesta.

2. **Configura MQTT.**  
   Apri **Impostazioni MQTT** e inserisci host, porta, credenziali se necessarie, topic base e intervallo di invio.

3. **Aggiungi un dispositivo.**  
   Avvia una scansione BLE, seleziona il dispositivo compatibile e inserisci la chiave di decrittazione, quando necessaria.

4. **Avvia il monitoraggio.**  
   Apri il dispositivo per consultare le misure, visualizzare lo storico e configurare gli allarmi.

> I nomi delle voci possono variare leggermente in base alla versione dell’app.

## Trasmettitore e ricevitore

### Modalità trasmettitore

Il telefono trasmettitore legge i dispositivi tramite BLE e pubblica i dati sul broker MQTT configurato.

Nelle impostazioni MQTT di ciascun dispositivo puoi scegliere un nome leggibile per il topic. Se non imposti un nome, viene utilizzato l’indirizzo MAC del dispositivo.

### Modalità ricevitore MQTT

Il ricevitore permette di consultare i dati raccolti da un altro telefono.

1. Apri **Impostazioni MQTT**.
2. Attiva la **modalità ricevitore**.
3. Configura la connessione al broker e salva le impostazioni.

Il ricevitore si collega al broker e visualizza i dati pubblicati dal trasmettitore. Non effettua la scansione BLE dei dispositivi remoti.

### Configurazione tramite QR

Puoi trasferire la configurazione dal trasmettitore al ricevitore tramite un codice QR.

1. Sul trasmettitore, apri **Impostazioni MQTT** e mostra il QR di configurazione.
2. Sul ricevitore, apri **Impostazioni MQTT**, seleziona la scansione QR e inquadra il codice.
3. Dopo l’importazione, il ricevitore si connette al broker e riceve i dati MQTT del trasmettitore.

> **Attenzione:** il QR contiene i parametri del broker e la configurazione dei dispositivi, comprese le chiavi BLE e, se configurate, le credenziali MQTT. Condividilo esclusivamente con dispositivi e persone fidati.

Le misure e lo storico non sono contenuti nel QR: vengono ricevuti tramite MQTT.

## MQTT e integrazione domotica

Il **topic base** è configurabile. Per ciascun dispositivo viene utilizzato il nome MQTT scelto nelle relative impostazioni oppure, se il campo è vuoto, il suo indirizzo MAC.

Per ricevere correttamente i dati, il telefono ricevitore e gli altri sistemi devono collegarsi al broker utilizzato dal trasmettitore e ascoltare i topic corretti.

I dati pubblicati possono essere utilizzati in **Home Assistant** e in altri sistemi che supportano MQTT, configurando l’integrazione in base ai topic e ai messaggi trasmessi dall’app.

## Allarmi e funzionamento in background

Per continuare a raccogliere dati e ricevere allarmi quando l’app non è in primo piano:

- Lascia attivo il **servizio di monitoraggio**.
- Concedi il **permesso per le notifiche**.
- Se il telefono limita l’attività in background, escludi l’app dall’**ottimizzazione della batteria**.

Durante il monitoraggio, Android mostra una notifica persistente.

La chiusura della schermata o la rimozione dell’app dai recenti non equivale necessariamente all’arresto del servizio. Per interrompere completamente il monitoraggio, usa **Esci** nell’app.

> Il funzionamento in background e la ricezione delle notifiche dipendono anche dai permessi concessi e dalle limitazioni applicate dal produttore del telefono.

## Risoluzione dei problemi

| Problema | Controlli consigliati |
| --- | --- |
| Il dispositivo BLE non appare | Verifica che il dispositivo sia compatibile, attiva Bluetooth e posizione quando richiesta, concedi i permessi e avvicina il telefono al dispositivo. |
| Le misure non si aggiornano | Controlla la connessione BLE sul trasmettitore. Per gli aggiornamenti via MQTT, verifica anche il broker, la connessione di rete e che l’intervallo di invio sia maggiore di zero. |
| Il ricevitore non mostra dati | Verifica che trasmettitore e ricevitore usino lo stesso broker e i topic corretti e che l’invio MQTT sia abilitato per il dispositivo. |
| Gli allarmi non arrivano a schermo spento | Controlla il permesso notifiche, le regole di allarme, il servizio di monitoraggio e le impostazioni della batteria. |
| Il QR non viene letto | Aumenta la luminosità dello schermo del trasmettitore, mantieni il codice intero nell’inquadratura e prova a variare la distanza. |

## Privacy e sicurezza

Le **credenziali MQTT**, le **chiavi BLE**, i **codici QR di configurazione** e gli eventuali archivi esportati che le contengono sono informazioni sensibili.

- Proteggi l’accesso ai telefoni utilizzati.
- Non pubblicare QR, credenziali o chiavi nella repository, nelle segnalazioni di problemi o negli screenshot.
- Condividi configurazioni e archivi esportati solo con persone fidate.
- Utilizza un broker protetto da autenticazione e, quando supportato dalla configurazione utilizzata, un trasporto cifrato.

## Marchi e indipendenza del progetto

**BLU-to-MQTT-BRIDGE è un progetto indipendente, non affiliato, sponsorizzato o approvato da Victron Energy.**

Il nome **Victron Energy** è utilizzato esclusivamente per identificare i dispositivi ai quali si riferisce la documentazione. I marchi citati appartengono ai rispettivi titolari.

La menzione di un marchio non implica una certificazione, una collaborazione ufficiale o la compatibilità con tutti i suoi prodotti.
