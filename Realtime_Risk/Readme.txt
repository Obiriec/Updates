// -------------------------------------------------------------------------------
// Realtime % Risk Monitor (by Fury Team)
// Versione: 3.1.4
// Data di rilascio: 2025-05-20
// Autore: Armando Brecciaroli – Fury Team ©
//
// Descrizione completa:
//     Sistema avanzato di monitoraggio realtime del rischio operativo per trading live.
//     Funzionalità principali:
//     - Equity Risk % e Drawdown % aggiornati in tempo reale.
//     - Calcolo e visualizzazione dinamica dell'efficienza operativa (P/L % ÷ Risk %).
//     - HUD grafico configurabile (modalità Full o Mini).
//     - 4 Slider dinamici colorati: Risk %, Drawdown %, Efficiency, Profit/Loss %.
//     - Bordo lampeggiante automatico su Warning/Danger.
//     - Sfondo HUD rosso trasparente in caso di pericolo (Danger).
//     - Timer visivo di durata rischio attivo.
//     - Sistema sonoro di alert configurabile e dinamico.
//     - Visualizzazione dinamica P/L % rispetto a Balance / Equity / Free Margin (selezionabile).
//     - Equity Liquidator intelligente con trailing profit e gestione Stoploss target dinamico.
//     - Visualizzazione HUD del Liquidator: Equity Profit Target %, Loss Target %, Max Equity raggiunto, Trailing SL attivo.
//     - Slider Liquidator con stato percentuale, emoji dinamiche e colore variabile in progressione realistica.
//     - Aggiornamento frequenza slider liquido e transizioni gradienti fluide verso il Gold.
//     - Versione ultra-stabile ottimizzata per reale operatività live.
//     - Prodotto completo, sicuro e aggiornato 2025.
//
// Changelog:
//     v1.0.0 - Prima versione base Risk Monitor (solo Risk%).
//     v1.5.0 - Aggiunta gestione Warning/Alert e HUD grafico dinamico.
//     v1.6.0 - Introdotta modalità Mini HUD + filtri colore slider dinamici.
//     v1.7.0 - Integrazione bordo lampeggiante Warning/Danger + test posizione automatica.
//     v1.8.0 - Migliorie HUD, controllo errori parametri, suoni alert, slider Risk % perfezionato.
//     v1.9.0 - Introduzione Drawdown %, Risk/Drawdown Ratio, sfondo Danger, timer rischio attivo.
//     v2.0.0 - Nuovo calcolo Efficienza (P/L % ÷ Risk %), 4 slider dinamici, HUD migliorato.
//     v2.1.0 - Aggiunta soglie Efficiency Warning/Danger, Last Risk Spike visibile, MiniHUD completo.
//     v2.1.1 - Audio dinamico configurabile da menu, gestione file .wav sicura.
//     v2.1.2 - Migliorie finali Audio Alert Cooldown: reset automatico flag e ripetizione su rischio ricorrente.
//     v3.0.0 - INTEGRAZIONE EQUITY LIQUIDATOR:
//              - Modulo opzionale Equity Liquidator con Trailing Profit
//              - Target Equity % configurabile
//              - Loss Target % su trailing configurabile
//              - Rimozione Pending Orders
//              - HUD compatibile
//              - Performance ottimizzata
//              - Risk Monitor e Liquidator separati
//     v3.1.0 - (28.04.2025):
//              - Rivisitazione completa Liquidator HUD
//              - Introduzione stato dinamico Equity Profit Target %, Loss Target %, Max Equity, Trailing Stop
//              - Aggiornamento slider Liquidator con Emoji dinamiche
//              - Progressione reale e scalata colore con gradienti fluide
//              - Rimozione effetto Glow
//              - Ottimizzazione realistica delle percentuali target (massimo 100% reale)
//              - Miglioramento testo menu parametri Liquidator per chiarezza assoluta
//
//     v3.1.1 - (19.05.2025):
//              - Ottimizzazione stabilità generale del sistema
//              - Miglioramento transizioni grafiche dello slider Liquidator
//              - Risoluzione bug di visualizzazione in miniHUD mode
//              - Aggiornamento label dinamiche con maggiore precisione
//              - Migliore gestione del trailing stop loss
//              - Ottimizzazione del consumo di memoria
//              - Etichetta "Best Edition" per versione premium
//
//     v3.1.2 - (19.05.2025):
//              - Miglioramento visibilità slider con larghezza minima garantita
//              - Riorganizzazione layout con slider posizionati sopra le etichette
//              - Visualizzazione Real Profit/Loss anche con Liquidator disattivato
//              - Ottimizzazione interfaccia grafica per migliore leggibilità
//              - Perfezionamento visualizzazioni HUD in modalità mini
//
//     v3.1.3 - (20.05.2025):
//              - Separazione completa degli HUD: Risk Monitor e Equity Liquidator
//              - Posizionamento intelligente degli HUD nei lati opposti del chart
//              - Calcolo preciso dei volumi per chiusure parziali multi-livello
//              - Integrazione gestione volumi minimi specifici per simbolo
//              - Arrotondamento preciso dei lotti secondo regole broker
//              - Avvisi utente per percentuali di chiusura non valide
//              - Logging dettagliato delle operazioni di chiusura posizioni
//              - Maggiore robustezza nella gestione dei casi limite
//
//     v3.1.4 - (20.05.2025):
//              - Correzione layout Liquidator HUD per migliore visualizzazione
//              - Riorganizzazione dello slider del Liquidator per corretta visualizzazione
//              - Separazione più precisa dei componenti dell'interfaccia
//              - Miglioramento delle etichette dinamiche del Liquidator
//              - Ottimizzazione del posizionamento degli elementi grafici
//              - Aggiornamento struttura Multi-Level Protection
//              - Risoluzione problemi di visualizzazione grafica
//
//     v3.1.5 - (20.05.2025):
//              - Implementazione sistema avanzato di statistiche di rischio
//              - Monitoraggio statistico configurabile: giornaliero, sessione, settimanale, mensile
//              - Visualizzazione di valori massimi, minimi e medi per Risk, Drawdown, Efficiency e P/L
//              - HUD statistiche separato con posizionamento intelligente
//              - Posizionamento personalizzabile dell'HUD statistiche (automatico o manuale)
//              - Reset automatico o manuale delle statistiche
//              - Ottimizzazione calcolo valori statistici
//              - Supporto per profit taking con percentuali decimali (0,01%)
//              - Utilizzo di punto e virgola (;) come separatore tra livelli
//              - Migliorata visualizzazione con virgola per i decimali
//
//     v3.1.6 - (20.05.2025):
//              - Implementato sistema di verifica licenza e controllo aggiornamenti
//              - Aggiunta notifica automatica di nuove versioni disponibili
//              - Supporto per aggiornamenti obbligatori e comunicazioni urgenti
//              - Migliorata sicurezza e verifica account utente
//              - Visualizzazione dei changelog e note di rilascio
//              - Integrazione completa con server di licenze e aggiornamenti
//
//
// Note:
//     Prodotto realizzato da Armando Brecciaroli per Fury Team.
//     Per supporto o miglioramenti: a.brecciaroli@me.com | Telegram: @Obiriec
// -------------------------------------------------------------------------------
