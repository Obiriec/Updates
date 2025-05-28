// ════════════════════════════════════════════════════════════════════════════
// 📊 INDICATORE RSI MULTI-TIMEFRAME PROFIT SENTINEL (STANDALONE)
// Versione: 1.3.3 - Ottimizzato per affidabilità e robustezza
// Autore: Fury Team - Armando Brecciaroli
// Data: 28 Maggio 2025
// ════════════════════════════════════════════════════════════════════════════
// 
// DESCRIZIONE:
// ===========
// Profit Sentinel è un indicatore avanzato basato su RSI che analizza due 
// timeframe differenti per generare segnali di trading di alta qualità.
// Lo strumento identifica divergenze tra i valori RSI dei diversi timeframe,
// con modalità operative multiple (TrendFollowing e Contrarian) e filtri
// di qualità configurabili come persistenza, conferme su barre, 
// delta crescente e curvatura RSI.
//
// CARATTERISTICHE PRINCIPALI:
// =========================
// - Sistema Multi-Timeframe con analisi comparativa dei valori RSI
// - HUD diagnostico completo con visualizzazione dettagliata dei segnali
// - Filtri di qualità configurabili per ridurre i falsi segnali
// - Etichette grafiche e slider tecnico per visualizzazione immediata
// - Supporto per strategie sia di trend-following che contrarian
//
// CHANGELOG:
// =========
// v1.1 (11/05/2025)
// - Logica identica al cBot ProfitSentinel v1.6.9
// - Prima implementazione standalone
//
// v1.2 (18/05/2025)
// - Ottimizzato metodo AggiornaRSI() per riutilizzare gli indicatori esistenti
// - Migliorata visualizzazione dei segnali con frecce e linee verticali
// - Corretti contatori di persistenza nel metodo ResetSegnali()
// - Aggiunto controllo esplicito sulla validità dei timeframe
// - Ottimizzata gestione della memoria per migliorare le performance
//
// v1.3.0 (21/05/2025)
// - Corretta la logica del filtro "Abilita Conferme su Barre"
// - Migliorata la gestione dei contatori di conferma per supportare tutti i valori
// - Implementato tracciamento avanzato della barra di conferma
// - Completata implementazione visualizzazione segnali SHORT
// - Aggiunto debugging esteso per facilitare la diagnosi dei problemi
//
// v1.3.1 (23/05/2025)
// - Implementata gestione errori robusta con blocchi try/catch
// - Consolidata logica per segnali LONG e SHORT con il metodo GestisciSegnale()
// - Ottimizzata la diagnostica di errore per facilitare il troubleshooting
// - Migliorato sistema di logging con informazioni dettagliate sugli errori
// - Aggiunto filtro EMA per direzionalità dei segnali basata sul trend
// - Implementata visualizzazione dell'EMA sul grafico principale
// - Integrazione completa dell'EMA nell'HUD diagnostico
//
// v1.3.2 (26/06/2025)
// - Migliorato HUD EMA con supporto per posizionamento personalizzabile
// - Aggiunti parametri per controllare la distanza orizzontale e verticale dell'HUD EMA
// - Corretti problemi di visualizzazione dell'HUD EMA
// - Ottimizzato layout e formattazione delle informazioni EMA
//
// v1.3.3 (28/05/2025)
// - Corretto problema di rimozione automatica dei messaggi di licenza
// - Migliorata la gestione del timer per garantire la pulizia degli elementi grafici
// - Ottimizzato il sistema di rimozione temporizzata degli oggetti dal grafico
// - Implementata visualizzazione del messaggio di licenza nel pannello indicatore
// - Aggiunto logging avanzato per facilitare il debug dei problemi di visualizzazione
// ════════════════════════════════════════════════════════════════════════════
