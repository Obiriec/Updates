// ===================================================================================================
// PROFIT SENTINEL - Sviluppato da Armando Brecciaroli per Fury Team ©2023-2025
// Strategia Automatica Multi-Livello con Moduli Dinamici Recovery / TP / RSI Signal Engine
// Versione: 2.3.0 - Sistema Licenze e Controllo Aggiornamenti
// Build Date: 05-06-2025
// Sviluppato per cTrader Automate API
// © Tutti i diritti riservati - Prodotto non ridistribuibile
// ==================================================================================
//
// DESCRIZIONE:
// ProfitSentinel è un cBot avanzato per la piattaforma cTrader, progettato per la gestione automatizzata
// delle posizioni aperte tramite logiche di Trailing Profit, Take Profit fisso e Recovery Trades progressivi.
// Il sistema integra protezioni avanzate su Free Margin e Margin Level, gestione dinamica del rischio,
// Drawdown Progressivo, Cooldown movimento prezzo, e HUD grafico in tempo reale per la diagnostica operativa.
// Nella versione 1.8.0 sono stati aggiunti sistemi avanzati di analisi del rischio con Value at Risk (VaR),
// previsione della volatilità con modelli ML, riconoscimento pattern candlestick e analisi correlazione mercati.
//
// FUNZIONI PRINCIPALI:
// - Apertura iniziale automatica di trade LONG o SHORT selezionabile da menu.
// - Chiusura su Take Profit fisso con gestione opzionale di Drawdown (%).
// - Chiusura su Trailing TP dinamico con gestione Drawdown intelligente e TPStart % dinamico.
// - Sistema di Recovery trades avanzato con moltiplicatore progressivo e protezioni Free Margin e Margin Level.
// - Recovery con distanza minima da BreakEven, peggioramento progressivo del Drawdown, e movimento minimo richiesto.
// - Filtri Recovery: distanza minima pips, conferma RSI multi-condizione, movimento prezzo minimo richiesto.
// - Controllo MinProfit basato su margine usato + percentuale configurabile prima della chiusura profittevole.
// - HUD avanzato e dinamico: profitto, TP, Recovery, margini disponibili, protezioni attive, filtri operativi.
// - Diagnostica RSI Multi-Timeframe con filtri qualità: persistenza, delta crescente, curvatura, conferma a N barre.
// - Protezioni contro aperture multiple, spread e timing ingresso personalizzabile (conferma o barra successiva).
// - Massima stabilità, gestione sicura ordini e protezioni integrate anti-margin-call.
// - Analisi del rischio avanzata con Value at Risk (VaR) e modelli predittivi di volatilità.
// - Riconoscimento pattern candlestick per ottimizzazione segnali e timing entrata.
// - Sistema di monitoraggio correlazione tra mercati per analisi regime e correlazione.
//=======================================================================================================================
//
// CHANGELOG:
//
//    v1.3.6 (16 Aprile 2025)
//           - Prima versione ufficiale pubblica del ProfitSentinel v1.3.6.
//           - Sistema Recovery trades con moltiplicatore progressivo, TP fisso e Trailing TP dinamico.
//           - Controllo MinProfit su margine utilizzato.
//           - HUD grafico dinamico.
//
//    v1.3.7 (18 Aprile 2025)
//           - Correzioni formule Recovery, Break-Even, volumi.
//           - Migliorie HUD e gestione eventi posizioni chiuse.
//
//    v1.3.8 (19 Aprile 2025)
//           - Controllo corretto TP fisso e dinamico rispetto MinProfit (margine usato).
//           - Avvisi in HUD se TP insufficienti.
//           - Ottimizzazione descrizioni parametri Recovery Filters.
//
//    v1.5.0 (23 Aprile 2025)
//           - Recovery avanzato: distanza BreakEven, peggioramento DD richiesto.
//           - Introduzione Cooldown movimento minimo prezzo tra Recovery trades.
//           - Gestione riduzione lotto su Free Margin basso.
//
//    v1.5.1 (24 Aprile 2025)
//           - Protezione avanzata su Free Margin minimo (€) e Margin Level minimo (%).
//           - HUD migliorato: alert visivi su Free Margin e Margin Level.
//           - Revisione parametri sicurezza con soglie dirette in € e % reali.
//
//    v1.5.2 (24 Aprile 2025)
//           - HUD completamente aggiornato: diagnostica completa Free Margin, Margin Level e filtri Recovery.
//           - Migliorato controllo separato su Free Margin reale (€) e Margin Level (%).
//           - Ottimizzazione alert dinamici a colori in caso di rischio margine basso.
//           - Pulizia finale HUD: rimosse variabili obsolete e protezioni non più usate.
//
//    v1.5.3 (26 Aprile 2025)
//           - Recovery UltraPro: apertura recovery condizionata da nuovo swing/movimento reale + peggioramento € netto.
//           - HUD avanzato con Cooldown/Movimento/Gap Drawdown mostrati in formato leggibile.
//           - Refactoring completo in sezioni ordinate e commentate, per manutenzione rapida e sviluppo modulare.
//
//    v1.5.4 (27 Aprile 2025)
//           - Miglioramento del sistema di gestione del rischio con nuove logiche di controllo del drawdown.
//           - Ottimizzazione delle performance del cBot in condizioni di mercato ad alta volatilità.
//           - Aggiunta di nuove metriche nel HUD per una migliore visualizzazione delle condizioni di mercato.
//           - Correzione di bug minori e miglioramenti nella stabilità complessiva del sistema.
//
//    v1.5.5 (28 Aprile 2025 - Dynamic TPStart & Diagnostica MultiSimbolo Upgrade)
//           - ➕ Aggiunto Dynamic TPStart basato su % di Balance/Equity/FreeMargin.
//           - ➕ Aggiunta protezione TPStartMinimo per trailing sicuro su TPStart dinamico.
//           - ➕ Visualizzazione Dynamic TPStart nell'HUD con colori adattivi, Flash Smooth e icone di stato.
//           - ➕ Aggiunta Diagnostica Simboli attivabile da menu: verifica multi-simbolo su HUD.
//           - 🔥 Semaforo visivo 🟢🟠🔴 in tempo reale su HUD, con lampeggio rosso su errori gravi.
//           - 🛠️ Refactoring OnTick con ottimizzazione HUD Ultra-Lightweight senza spam.
//           - 🛠️ Protezione contro spam log: avviso console intelligente solo ogni 10 secondi su variazioni reali.
//           - 🔒 Compatibilità totale Live + Demo + VPS Ready.
//           - 📈 Migliorie finali su performance grafica, gestione eventi, sicurezza ordini.
//
//    v1.5.6 (29 Aprile 2025 - Migliorie HUD e Refactoring Visuale Sicuro)
//           - ✅ Rimozione parametro duplicato HUDesteso, mantenuto solo HUDCompatto (unificazione logica).
//           - ✅ Corretto stato HUD: mostra correttamente "Compatto" o "Esteso" in base al flag attivo.
//           - ✅ Barra HUD distanza BreakEven con riempimento proporzionale corretto (massimo 10 blocchi).
//           - ✅ Refactoring finale `DrawHUD()` e `DrawHUDCompatto()` per coerenza visiva e leggibilità.
//           - 🔐 Nessuna modifica alle logiche operative principali: TP/Recovery invariati.
//           - 🧱 Versione stabile pronta per distribuzione o pacchettizzazione .algo/.cbotset.
//           - 🔧 Tutte le modifiche modulari, sicure, integrate senza impatto retroattivo.
//           - 🧠 Allineato al profilo "Fury Team Release Mode – maggio 2025".
//
//    v1.5.7 (30 Aprile 2025 - Colorazione Swing HUD Integrata)
//           - 🎨 Integrazione completa modulo "HUD Color Dynamic" in DrawHUD().
//           - 🎯 Priorità di colorazione: Swing ➜ TP/Profit ➜ Recovery ➜ fallback logico (profit/loss).
//           - 🌈 Colori semitrasparenti contestuali: blu/rosso chiaro in base a Swing attuale.
//           - 🔁 Nessuna sovrascrittura di codice preesistente: blocco integrato in modalità isolata.
//           - ⚙️ Pronto per estensioni future su RGB dinamico, interpolazione colore o Flash HUD.
//           - 📌 Versione 100% modulare e aderente alle direttive: nessuna cancellazione, solo append.
//
//    v1.5.8 (2 Maggio 2025 - Miglioramenti Gestione BreakEven e Debug)
//           - 🛠️ Fix critico nel calcolo del margine usato con doppio fallback (GetEstimatedMargin + calcolo manuale)
//           - 📊 Aggiunti log di debug avanzati per il calcolo del margine e controllo MinProfit
//           - 🔍 Migliorata la diagnostica HUD per mostrare i dettagli del calcolo del margine
//           - 🧹 Pulizia codice e ottimizzazioni minori nelle funzioni di utilità
//           - 🔄 Migliorata la gestione degli errori durante il calcolo del margine
//           - 📈 Aggiunta visualizzazione del margine calcolato nell'HUD esteso
//           - 🛡️ Aggiunte ulteriori protezioni contro divisioni per zero e valori non validi
//
//    v1.5.9 (3 Maggio 2025 - Miglioramenti Visualizzazione TP Dinamico)
//           - ✅ Aggiunta visualizzazione esplicita del profitto mancante per raggiungere il TP dinamico
//           - 📌 Nell'HUD Esteso: dettaglio numerico "Stato attuale: X€ (mancano Y€ per Z€)"
//           - 📱 Nell'HUD Compatto: formato ultra-sintetico con icona ✔/✖ e importo mancante
//           - 🔄 Nessuna modifica alle logiche operative esistenti, solo miglioramenti visualizzazione
//           - 🧹 Ottimizzazione formattazione codice nelle nuove sezioni aggiunte
//           - 🛡️ Mantenute tutte le protezioni e logiche preesistenti intatte
//
//    v1.5.9-r2 (3 Maggio 2025 - Fix Completo BreakEven Visual + HUD)
//           - ✅ Corretto disegno linea BreakEven: ora usa sempre il prezzo target reale e non EntryPrice
//           - ✅ Etichetta grafica e linea BE aggiornate con prezzo esatto di uscita calcolato
//           - ✅ HUD mostra ora correttamente il prezzo target BreakEven, non più l'entry errato
//           - ✅ Slider pips distanza BreakEven usa offset effettivo dal prezzo target
//           - 🔄 Refactoring DrawBreakEvenLineSafe() e GetBreakEvenPrice() per allineamento corretto
//           - 🧠 Aggiornati OnTick(), DrawHUD() e HUD compatto per coerenza dati BE
//           - 🔧 Mantenute intatte tutte le logiche operative (TP, Recovery, MinProfitCheck)
//           - 📌 Changelog e intestazione aggiornati alla v1.5.9-r2 (Fury Team Release Mode)
//
//    v1.5.9-r3 (3 Maggio 2025 - Fix HUDCompatto BreakEven Definitivo)
//           - ✅ HUD Compatto aggiornato: ora mostra sempre il prezzo reale di chiusura BreakEven (target), non più l’EntryPrice.
//           - ✅ Corretto calcolo e visualizzazione distanza pips nello slider BE, ora allineato al target reale.
//           - ✅ Righe “BE: ... €” e “BE Target: ...” ora derivano da GetBreakEvenTargetPrice() anche nel compatto.
//           - 🔁 Pulizia uso variabile `be` nel metodo DrawHUDCompatto(): sostituito con calcolo diretto coerente.
//           - 🛠️ Nessun impatto sulle logiche operative o di chiusura reali: solo miglioramento visuale coerente.
//
//    v1.5.9-r4 (3 Maggio 2025 - Fix Finale BreakEven Visual + Calcolo Pips)
//           - ✅ Corretto il calcolo `GetBreakEvenTargetPrice()` con divisione sicura su `PipValue / 100000.0`
//           - ✅ Valore BreakEven ora coerente in HUD, linea grafica e slider pips
//           - ✅ Aggiunto log di debug `🧪 DEBUG BE: Bid=X, BE=Y, PipSize=Z` per facilitare diagnosi
//           - ✅ Eliminati calcoli errati dovuti a unità non normalizzate
//           - ✅ Funzionalità Breakeven ora 100% stabile e pronta per rilascio definitivo
//    v1.5.9-r4 (3 Maggio 2025 - Fix Finale BreakEven Visual + Calcolo Pips)
//           - ✅ Corretto il calcolo `GetBreakEvenTargetPrice()` con divisione sicura su `PipValue / 100000.0`
//           - ✅ Valore BreakEven ora coerente in HUD, linea grafica e slider pips
//           - ✅ Aggiunto log di debug `🧪 DEBUG BE: Bid=X, BE=Y, PipSize=Z` per facilitare diagnosi
//           - ✅ Eliminati calcoli errati dovuti a unità non normalizzate
//           - ✅ Funzionalità Breakeven ora 100% stabile e pronta per rilascio definitivo
//
//    v1.6.0 (04 Maggio 2025 - Rilascio Finale Stabile HUD+BE)
//           - 🧱 Versione finale stabile dichiarata: HUD Esteso e HUD Compatto definitivi
//           - ✅ Corretto duplicato versione nell’HUD compatto: ora mostra solo `BUILD_ID` coerente
//           - ✅ Restyling completo `DrawHUDCompatto()`: ora più leggibile, diretto, efficiente
//           - 🎯 Tutte le visualizzazioni BE ora coerenti: slider, target, distanza pips
//           - 📌 Versione ufficiale *Fury Team Release v1.6.0* marcata come stabile per utilizzo operativo
//           - 🚀 Pronta per compilazione `.algo` e pacchetti `.cbotset` di distribuzione
//
//    v1.6.1 (3 Maggio 2025 - BreakEven Fix: Fisso e Universale)
//           - ✅ Ripristinata logica BreakEven a soglia fissa, evitando comportamento da trailing stop
//           - ✅ `GetBreakEvenTargetPrice()` ora restituisce un valore costante basato su `fixedBreakEvenProfitThreshold`
//           - ✅ Compatibilità BreakEven estesa a simboli CFD/indice come GERMANY 40 (TopFX)
//           - ✅ Corretto `DrawBreakEvenLineSafe()` con etichetta a posizione fissa sul prezzo target
//           - ✅ Completato restyling HUDCompatto con slider BE corretto e sintesi visiva migliorata
//           - 🔄 HUD Esteso aggiornato: visualizzazione corretta pips BE e slider adattivo
//           - 🔐 Nessuna regressione sulle logiche TP dinamico, Recovery o MinProfitCheck
//           - 📌 Versione stabile universale per tutti i simboli (Forex, Indici, CFD)
//
//    v1.6.2 (6 Maggio 2025 - Revisione Completa RSI Multi-Timeframe + HUD Diagnostico)
//           - ✅ Rimosso completamente il riferimento a indicatori RSI esterni: ora la logica RSI è interna e indipendente
//           - 🔁 Refactoring completo dei metodi `IsSegnaleRSIValido()`, `AggiornaRSI()` e `DebugSegnaleRSI()` per gestione multi-timeframe pulita
//           - 📌 Valori RSI Base e Superiore gestiti separatamente con memorizzazione, aggiornamento e logging coerente
//           - 🧠 Nuova diagnostica RSI avanzata: stampa log dettagliati con cooldown, delta RSI e condizioni valide
//           - ✳️ Aggiunto supporto forzatura segnale RSI via flag `ForzaSegnaleRSI` (per test e debug)
//           - 🎯 HUD aggiornata: visualizzazione coerente dei segnali RSI, incluso disegno grafico segnale su chart
//           - 🧱 Pulizia completa codice RSI: ora modulare, testabile e compatibile con simboli diversi
//           - ✅ Consolidata la logica ordine da segnale RSI con protezioni anti-duplicazione posizione e label coerenti
//           - 📌 Versione ufficiale *Fury Team Release v1.6.3* per RSI Multi-Timeframe & HUD Diagnostico Integrato
//
//    v1.6.3 (6 Maggio 2025 - Consolidamento HUD RSI e Protezioni Iniziali)
//           - ✅ Consolidata struttura HUD con nuova sezione RSI diagnostico: visualizzazione completa LONG/SHORT live
//           - ✅ Aggiunto controllo: blocco segnale RSI se TimeFrame superiore ≤ TF base ➜ log + HUD warning dedicato
//           - ✅ Migliorata compatibilità avvio anche con SL e TP = 0 (ordini senza limiti iniziali ammessi)
//           - ⚠️ Aggiunto warning live: TP segnale troppo stretto, potenziale rischio spread > profitto
//           - ✅ Corretto errore su logica `MinProfitOverMargin`: ora valutato solo con posizioni effettive aperte
//           - ✅ SafeCloseAllPositions() ora chiude tutte le posizioni sul simbolo, ignorando label/commenti
//           - ✅ Cooldown entry RSI ora impedisce doppie aperture: verifica se esistono posizioni aperte nel symbol
//           - 🔧 Debug OnBar arricchito con diagnostica RSI dettagliata: delta, tempo ultimo segnale, stato valido
//           - 📌 Versione ufficiale *Fury Team Release v1.6.3* stabile per segnali RSI, chiusura coerente, entry sicura
//
//    v1.6.4 (6 Maggio 2025 - Trailing TP Reale + Protezione Soglia Garantita)
//           - ✅ TP Dinamico trasformato in vero trailing stop: maxTP viene aggiornato ogni volta che il profitto supera il massimo precedente
//           - ✅ La soglia minima di profitto aggiornata segue esattamente il valore massimo raggiunto con TPDrawdown% costante
//           - ✅ Protezione integrata: se attivo MinProfitCheck, la soglia minima è sempre la maggiore tra trailing e margine richiesto
//           - ✅ Corretto comportamento SafeCloseAllPositions(): ora chiude solo se il profitto netto è conforme alla soglia trailing
//           - 🧠 HUD Esteso aggiornato: visualizza soglia minima dinamica aggiornata + stato trailing real-time
//           - 🔍 Debug avanzato su ogni update di maxTP, soglia trailing e condizioni di chiusura dinamica
//           - 🛡️ Protezione aggiuntiva: nessuna chiusura autorizzata se profitto < trailingMinProfit anche se trigger avvenuto
//           - 🧱 Sincronizzato con struttura HUD v1.6.3: visualizzazione coerente, integrata e completa
//           - 📌 Versione ufficiale *Fury Team Release v1.6.4* per trailing TP sicuro e coerente in tempo reale
//
//    v1.6.5 (7 Maggio 2025 - Fix TP Dinamico & Ottimizzazioni HUD)
//           - 🛠️ Corretto bug: chiusura TP dinamico poteva avvenire anche se maxTP < TPStart
//           - 🧠 Ora il trailing TP viene attivato solo dopo il reale superamento di TPStart o TPStartPercent
//           - ✅ Aggiunto controllo coerente anche nell'HUD per riflettere solo trailing attivo se triggerato
//           - 📱 HUD Compatto aggiornato con messaggi più precisi e ordinati per TP Dinamico e BreakEven
//           - 🖥️ HUD Esteso allineato per chiarezza, rimozione duplicati, ordine logico delle informazioni
//           - 🧪 Migliorato logging: log TP dinamico e BreakEven resi più leggibili e coerenti
//           - 📌 Versione ufficiale *Fury Team Release v1.6.5* pronta per build finale
//
//    v1.6.6 (8 Maggio 2025 - Conferma Segnale RSI Multi-Barra & Timing Apertura)
//           - ✅ Introdotto filtro "Conferma Segnale RSI": richiede N barre consecutive valide prima dell'esecuzione ordine
//           - ⏱️ Aggiunta opzione di Timing apertura posizione:
//               • Alla chiusura della barra che ha confermato il segnale
//               • Alla chiusura della barra successiva (maggior prudenza)
//           - 📊 Diagnostica HUD aggiornata con stato attesa conferma + metodo di apertura configurato
//           - 🧠 Log dettagliato delle fasi: accumulo barre, conferma, esecuzione condizionata dal timing selezionato
//           - 🔁 Integrato nel blocco OnBar(), DrawHUD() e parametri generali senza alterare la struttura esistente
//           - 📌 Versione ufficiale *Fury Team Release v1.6.6* focalizzata sulla robustezza del timing d'ingresso operazioni
//
//    v1.6.7 (8 Maggio 2025 - Protezione Recovery & Uniformità Formula DD)
//           - ⛔ Bloccata l'esecuzione di Recovery se il profitto netto attuale è positivo ➜ protezione obbligatoria
//           - 🧠 Uniformato il calcolo del drawdown Recovery usando `GetRecoveryReferenceValue()` in tutte le funzioni
//           - 🔍 Verifica completa delle formule drawdown in OnTick(), ExecuteLogic(), HUD e TryRecovery
//           - 📊 HUD aggiornato: DD visualizzato in % uniforme rispetto al riferimento selezionato (Balance, Equity, FreeMargin)
//           - 🔄 Refactoring e controllo approfondito su formula TP Dinamico vs BE per evitare doppi trigger incoerenti
//           - 📦 Codice completamente validato dopo controllo integrale in 3 sezioni
//           - 📌 Versione ufficiale *Fury Team Release v1.6.7* focalizzata sulla protezione Recovery e coerenza formule
//
//    v1.6.8 (9 Maggio 2025 - Fix DD Recovery su Balance + Uniformità MinDDIncreasePercent)
//           - 🧮 Corretto il calcolo del DrawDown Recovery: ora sempre basato sul Balance corrente (non più Equity o formule cumulative errate)
//           - 🎯 Uniformata la logica `Minimo Peggioramento DD%` ➜ confronto ora su base % coerente (DD vs Balance)
//           - 🪪 Log estesi con confronto percentuale tra DD corrente e soglia richiesta ➜ visibilità piena
//           - 🔧 Integrazione diretta in `TryRecovery()` con allineamento a tutte le modalità Recovery esistenti
//           - ✅ Test e validazione superata in condizioni multiple (trigger, peggioramento, cooldown, sicurezza)
//           - 📌 Versione ufficiale *Fury Team Release v1.6.8* con correzione critica drawdown su base balance
//
//    v1.6.9 (10 Maggio 2025 - Patch Etichette RSI + Fix HUD Diagnostico TrendFollowing/Contrarian)
//           - 🟢 Corretto il modulo `DrawSegnaleGraficoRSI()` ➜ ora stampa correttamente le etichette LONG/SHORT
//             anche in modalità TrendFollowing (le etichette ora si invertono rispetto al segnale grezzo)
//           - 🟣 Uniformata la logica dell'HUD RSI ➜ Diagnostica Avanzata e Tecnica ora mostrano sempre
//             la direzione operativa coerente con la modalità (Contrarian = segnale grezzo; TrendFollowing = inverso)
//           - 🔍 Migliorata la funzione `GeneraStringaQualitaSegnale()` con coerenza interna e compatibilità etichette
//           - 🧪 Debug visuale migliorato con stampa `Print()` della modalità RSI, direzione segnale grezzo,
//             etichetta stampata e direzione del trade effettivo
//           - ✅ Validato su tutte le combinazioni operative: TrendFollowing/Contrarian, con/contro trend, e
//             tutte le condizioni di Delta/Persistenza/Filtro attivi
//
//    v1.7.0 (13 Maggio 2025 - Fix Recovery Volume & Fattore Potenziamento DD)
//           - ✅ Corretto calcolo volume recovery: ora sempre in multipli di 0.01 lots
//           - 🔧 Implementato sistema automatizzato per Fattore Potenziamento DD in base alla modalità:
//               • Standard: usa il valore configurato senza modifiche
//               • Bilanciato: richiede +50% del valore base (min +1.5%)
//               • Aggressivo: riduce al 60% del valore base (min 1%)
//           - 🧮 Rivista formula calcolo volume recovery:
//               • Normalizzazione sempre a 0.01 lots
//               • Blocco su volumi inferiori al minimo
//               • Gestione coerente moltiplicatore in base alla modalità
//           - 🛡️ Aggiunta protezione volume minimo (0.01) in tutte le funzioni che usano ExecuteMarketOrder
//           - 📊 Migliorato sistema di logging per tracciamento calcolo volumi
//           - 🔄 Centralizzata gestione normalizzazione volumi
//           - 🎯 Ottimizzato sistema di logging per DD e volumi recovery
//           - 📌 Versione ufficiale *Fury Team Release v1.7.0* con fix critici gestione volumi
//
//    v1.7.1 (14 Maggio 2025 - Fix Recovery Avanzato & DD Dinamico)
//           - ✅ Implementato sistema DD dinamico basato sul rapporto tra i lotti
//           - 🧮 Tre modalità calcolo DD:
//               • Standard: usa radice quadrata (più conservativo)
//               • Enhanced: usa radice cubica (medio)
//               • Aggressive: usa moltiplicatore diretto (più aggressivo)
//           - 🔧 Aggiunto parametro DDMultiplier per amplificare effetto (1.0-5.0)
//           - 📊 Log dettagliati calcolo DD dinamico con tutti i fattori
//           - 🛡️ Protezione aggiuntiva su recovery ravvicinati
//           - 🔄 DD richiesto aumenta progressivamente con l'aumento dei lotti
//           - 📈 Formula: DD = BaseDD × √(LotRatio) × DDMultiplier
//           - 🧪 Test approfonditi su tutte le modalità di calcolo
//           - 📌 Versione ufficiale *Fury Team Release v1.7.1* con fix critici recovery
//
//    v1.8.0 (10 Giugno 2025 - Analisi Avanzata e Sistemi Predittivi)
//           - 🧠 Implementato sistema Value at Risk (VaR) per analisi rischio avanzata
//           - 📊 Aggiunta previsione volatilità con modello semplificato
//           - 🎯 Riconoscimento pattern candlestick per conferma segnali (Doji, Engulfing, ecc.)
//           - 🔄 Analisi correlazione mercati tra simboli chiave (oro, indici, forex)
//           - 📈 HUD arricchito con visualizzazione analisi avanzate (VaR, volatilità, pattern)
//           - 📐 Centralizzazione calcolo volatilità con metodi multipli (Range, ATR, StdDev)
//           - 🎯 Migliorato sistema di visualizzazione TP Dinamico con linea grafica interattiva
//           - 📝 Aggiunto log dettagliato per calcolo TP Dinamico con parametri precisi
//           - 🔧 Ottimizzata la gestione della formattazione numerica per diverse valute
//           - 📊 Sistema log strutturato con livelli e categorie
//           - 🔍 Migliorata la precisione del calcolo del prezzo target di uscita
//           - 📌 Versione ufficiale *Fury Team Release v1.8.0* - Advanced Risk Analysis
//
//    v1.9.0 (16 Maggio 2025 - Fix Critici & Miglioramenti Stabilità)
//           - 🛠️ Corretto bug critico del TP Dinamico che chiudeva posizioni con profitto inferiore alla soglia
//           - ✅ Implementato sistema robusto di chiusura con tentativo multiplo (fino a 3 retry per posizione)
//           - 🔄 Migliorata gestione BreakEven per mantenerlo attivo anche con TP Dinamico
//           - 📊 Corretto il calcolo della tolleranza di chiusura per evitare chiusure premature
//           - 🔄 Migliorato reset di maxTP e tpDinamicoAttivato dopo chiusura di tutte le posizioni
//           - 🔍 Aggiunto logging dettagliato per diagnosi problemi di chiusura
//           - ⚡ Ottimizzata protezione spread per prevenire chiusure durante spread elevato
//           - 📈 Migliorata coerenza between TP Dinamico e BreakEven con funzionamento parallelo
//           - 🧠 Logica adattiva per la gestione delle chiusure e rilevamento orfani
//           - 👁️ Visualizzazione migliorata del target TP Dinamico sul grafico
//
//    v1.9.1 (17 Maggio 2025 - Sistema Controllo Aggiornamenti e Gestione Licenze)
//           - ✅ Implementato sistema automatico di controllo aggiornamenti
//           - 🔐 Migliorato sistema di verifica licenze con formato JSON standardizzato
//           - 🛡️ Aggiunta visualizzazione notifiche su grafico per aggiornamenti disponibili
//           - ⚠️ Gestione aggiornamenti obbligatori con blocco esecuzione
//           - 📋 Visualizzazione dettagliata changelog e note di rilascio
//           - 🔄 Verifica periodica degli aggiornamenti (ogni 24 ore)
//           - 📊 Integrazione completa con server di licenze e aggiornamenti
//           - 🔧 Correzioni minori e miglioramenti di stabilità generali
//
//    v1.9.2 (22 Maggio 2025 - Sistema Licenze Avanzato con Auto-Nascondimento)
//           - ✅ Migliorata visualizzazione informazioni licenza con formattazione avanzata e dettagliata
//           - ⏲️ Aggiunto parametro configurabile per auto-nascondere il messaggio licenza dopo X secondi
//           - 📊 Aggiunto contatore visibile su schermo per il tempo rimanente prima della rimozione
//           - 🔧 Implementata gestione automatica dello stato di visualizzazione della licenza
//           - 🛡️ Ottimizzato e centralizzato il sistema di verifica licenza con gestione eventi
//           - 🖥️ Migliorato posizionamento e formattazione delle informazioni a schermo
//
//    v1.9.3 (26 Maggio 2025 - Filtro Distanza Dinamico Avanzato)
//           - ✅ Migliorato calcolo della distanza di sicurezza con adattamento proporzionale ai lotti aperti
//           - 📊 Implementata formula logaritmica per incremento dinamico della distanza minima tra recovery
//           - 🧮 Volume totale delle posizioni aperte ora influisce direttamente sulla distanza richiesta
//           - 🛡️ Protezione incrementale: maggiore è l'esposizione, più alta è la distanza di sicurezza
//           - 🔄 Fattore di scaling volume con crescita progressiva (1.0 + Log10(1.0 + totalOpenLots))
//           - 📈 Visibilità completa nel log per tutti i fattori di calcolo della distanza dinamica
//
//    v1.9.4 (30 Maggio 2025 - Sistema Integrato di Controllo e Sicurezza)
//           - ✅ Sistema completo di verifica licenze con modalità online/offline e controllo integrità
//           - 🔄 Implementazione controllo automatico degli aggiornamenti con notifiche interattive
//           - 🔐 Sistema di blocco operatività in caso di aggiornamenti obbligatori disponibili
//           - 🛡️ Protezione avanzata contro spread anomali con campionamento storico intelligente
//           - 🔍 Gestione migliorata dei cooldown tra recovery con scaling proporzionale ai volumi
//           - 📊 Sistema diagnostico RSI potenziato con metriche di qualità e visualizzazione dettagliata
//           - ⏱️ Visualizzazioni HUD ottimizzate con auto-nascondimento informazioni non essenziali
//           - 📈 Log strutturato a categorie per tracciamento completo delle operazioni
//           - 🧠 Funzioni diagnostiche avanzate per troubleshooting in tempo reale
//           - 🔧 Ottimizzazioni prestazionali per riduzione overhead computazionale e HUD
//
//    v2.0.0 (31 Maggio 2025 - Aggiornamento Visualizzazione Recovery e Ottimizzazioni)
//           - ✅ Migliorata visualizzazione della soglia DD minima per recovery nell'HUD
//           - 📊 Aggiunta formattazione dettagliata: "Soglia DD minima: 17.23% (8.23% + 9.00%)"
//           - 🧠 Ottimizzato calcolo decimali per visualizzazione prezzi dinamici
//           - 🔄 Correzioni formattazione numerica con precisione decimali dal broker
//           - 🎯 Migliorati calcoli visualizzazione TP dinamico con decimali corretti
//           - 🛡️ Migliorate protezioni contro errori di arrotondamento prezzi
//           - 📱 HUD operativo aggiornato con informazioni più chiare sulla soglia DD
//           - 🎨 Migliorata formattazione etichette grafiche con decimali broker-specific
//
//    v2.1.0 (01 Giugno 2025 - Miglioramenti Controlli RSI e Filtri)
//           - ✅ Aggiunto controllo ON/OFF per filtro Delta Minimo RSI
//           - ✅ Aggiunto controllo ON/OFF per Barre Conferma Segnale
//           - 🔍 Migliorata diagnostica per timing aperture posizioni (chiusura barra attuale/successiva)
//           - 📊 Log dettagliati sui controlli di validazione segnali e timing esecuzione
//           - 📑 Ottimizzazione complessiva dei filtri di qualità segnali
//
//    v2.1.1 (10 Giugno 2025 - Fix Logica Contrarian)
//           - ✅ Corretto bug critico nella logica della modalità Contrarian
//           - 🔄 Risolto problema di inversione errata dei segnali RSI
//           - ⚠️ In modalità Contrarian ora:
//               - Segnale LONG (RSI ipervenduto) → apre posizione LONG
//               - Segnale SHORT (RSI ipercomprato) → apre posizione SHORT
//           - 🧪 Aggiunti test di validazione per tutte le combinazioni di modalità/segnali
//           - 📊 Migliorata diagnostica con log espliciti della direzione operativa
//
//    v2.1.2 (20 Giugno 2025 - Fix Logica TrendFollowing e HUD)
//           - ✅ Corretto comportamento della modalità TrendFollowing
//           - 🔄 Implementata corretta inversione segnali in TrendFollowing:
//               - Segnale LONG (RSI ipervenduto) → apre posizione SHORT
//               - Segnale SHORT (RSI ipercomprato) → apre posizione LONG
//           - 🎨 Corretta la visualizzazione della direzione operativa nell'HUD
//           - 📊 Aggiornato HUD diagnostico per mostrare segnali coerenti con la modalità selezionata
//           - 🔍 Migliorati log di debug con tracciamento della modalità RSI e direzione operativa
//           - 🏷️ Etichette grafiche ora mostrano correttamente la direzione effettiva dell'operazione
///
//    v2.1.3 (04 Giugno 2025 - Ottimizzazioni generali e miglioramenti performance)
//           - 🔄 Ottimizzazione del sistema di gestione memoria e visualizzazione HUD
//           - ⚡ Migliorata efficienza calcoli ripetitivi per TP dinamico e recovery
//           - 🛡️ Rafforzata la gestione degli errori e protezioni di sicurezza
//           - 🖥️ Ottimizzata visualizzazione HUD con refresh ridotti per migliori performance
//           - 📊 Raffinato sistema diagnostico RSI con riduzione overhead computazionale
//           - 🚀 Miglioramenti generali stabilità per funzionamento prolungato in VPS
//           - 📝 Aggiornato sistema licenze per migliore compatibilità e verifica
//
//    v2.2.0 (04 Giugno 2025)
//           - 🔑 Implementazione sistema completo di licenze online/offline
//           - 🛡️ Migliorato sistema sicurezza e verifica integrità licenza
//           - 🔄 Aggiunto sistema automatico di controllo aggiornamenti
//           - 📋 Gestione avanzata aggiornamenti obbligatori/opzionali
//           - 💻 Interfaccia migliorata per visualizzazione stato licenza
//           - 📂 Supporto memorizzazione locale per licenze offline
//
//    v2.3.0 (05 Giugno 2025 - Miglioramenti HUD RSI e Prestazioni)
//           - ✅ Implementata disabilitazione automatica HUD RSI durante posizioni aperte
//           - 🛡️ Prevenzione gestione segnali RSI durante trade attivi per evitare confusione
//           - 🧹 Migliorata visualizzazione HUD con messaggio chiaro sullo stato sospeso
//           - 📈 Ottimizzata gestione memoria con minore overhead durante trade attivi
//           - ⚙️ Riattivazione automatica analisi RSI dopo chiusura posizioni
//           - 🔄 Migliorata coerenza fra HUD operativo e diagnostico durante operazioni in corso
//
// ============================================================================
