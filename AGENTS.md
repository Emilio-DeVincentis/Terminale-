# Terminale-
# AGENTS.md: Terminal (ZNT) - Lisp-Machine Philosophy

## 1. Visione del Progetto: 
 non è un semplice emulatore di terminale, ma un **ambiente di calcolo interattivo** ispirato alla filosofia del **SICP** (Structure and Interpretation of Computer Programs). Il progetto mira a trattare l'interfaccia utente e l'orchestrazione dei processi come manipolazione di strutture dati simboliche.
 e alla precisione analitica del SICM (Structure and Interpretation of Classical Mechanics).
Il progetto mira a superare il dualismo tra "programma" e "configurazione", trasformando il terminale in un'entità dinamica dove ogni operazione è una trasformazione funzionale di flussi simbolici.

L'obiettivo è l'eleganza matematica applicata ai sistemi POSIX:
- **Codice come Dati:** Ogni aspetto del terminale è una forma simbolica manipolabile a runtime.
- **Astrazione Procedurale:** Ispirato al **SICM**, il terminale deve permettere di modellare flussi complessi (pipeline di processi) come composizioni di funzioni matematiche pure.
- **Suckless Engineering:** Massima espressività con il minimo numero di linee di codice.

  Pilastri Concettuali:
Programmazione Funzionale Pura: Lo stato del terminale deve essere trattato come una successione di valori immutabili. Le trasformazioni avvengono tramite funzioni di ordine superiore.
Astrazione e Combinazione (SICP): Il sistema deve fornire poche primitive potenti (PTY, Buffer, Stream) e infiniti mezzi di combinazione (Lisp Macros, Functional Piping).
Meccanica dei Sistemi (SICM): I flussi di dati tra processi sono modellati come evoluzioni di un sistema dinamico, dove il "Lisp Layer" funge da lagrangiana del sistema.
Suckless Minimalismo: Implementazione in Zig per il core meccanico e Janet per la mente logica. Nessun software monolitico: il terminale orchestra programmi esterni (lazygit, bat, ranger) trattandoli come funzioni pure.

---

### 2. Stack Tecnologico & Paradigmi
Il progetto fonde la precisione meccanica della gestione manuale della memoria con l'eleganza della programmazione funzionale.

Architettura Ispirata al SICP
A. Core Engine (Il "Linguaggio Base" in Zig)
Il core agisce come la "Virtual Machine" del sistema. Deve essere invisibile e infallibile.
Determinismo: Gestione della memoria esplicita. Ogni buffer di testo è una struttura dati immutabile che viene "evoluta" tramite trasformazioni puramente funzionali esposte allo strato superiore.
Zero-Latency I/O: Gestione asincrona delle PTY e rendering basato su frame deterministici.
B. The Scripting Layer (L'Interprete Janet)
Lo strato Janet non è un semplice linguaggio di configurazione, ma il cuore pulsante del sistema:
Purezza Funzionale: Incoraggiare l'uso di strutture dati immutabili (tuples, structs) per gestire lo stato dei pannelli e dei buffer.
Effetti Algebrici via Fibers: Utilizzare i Fiber di Janet per gestire la concorrenza dei processi Linux come se fossero flussi di dati sincroni.
Metaprogrammazione: Uso intensivo di Macro per estendere la sintassi del terminale, permettendo all'utente di creare nuovi linguaggi di dominio (DSL) per l'orchestrazione di tool come lazygit, bat, o ranger.
4. Integrazione "Linux-as-a-Library" (Suckless Style)
In linea con la filosofia Unix, ZNT non reinventa la ruota ma compone programmi esistenti:
Processi come Funzioni: Ogni programma esterno (grep, sed, fzf) è trattato come una funzione pura: (input) -> output.
Pipeline Evolute: Janet funge da "colla" funzionale, permettendo di passare dati tra processi senza file temporanei, utilizzando pipe asincrone gestite come stream di dati infiniti.
5. Istruzioni per l'Agente (Best Practices)
Filosofia Funzionale
Evitare lo Stato Globale: Ogni componente deve ricevere lo stato come argomento e restituire un nuovo stato (Pattern: State Monad concettuale).
Recursion over Looping: Utilizzare la ricorsione e le funzioni di ordine superiore per manipolare i buffer di testo.
Immutabilità: Nel layer Janet, i dati del buffer devono essere trattati come immutabili finché non raggiungono il layer di rendering in Zig.
Integrazione e Strumenti
Quando implementi una nuova feature, chiediti: "Esiste un tool Linux che lo fa già?". Se sì, usa Janet per creare un wrapper funzionale attorno ad esso.
Il terminale deve supportare il caricamento di file .janet che ridefiniscono le funzioni core a runtime (Hot-swapping stile Lisp Machine).
Riferimento SICP/SICM
Progetta le API cercando di massimizzare il Linguaggio di Combinazione: poche primitive potenti che possono essere combinate all'infinito.
Ogni modulo deve avere una chiara separazione tra Astrazione e Implementazione.
Nota finale per l'Agent: Tratta questo progetto come se stessi scrivendo un capitolo moderno del SICP. La bellezza del codice e l'eleganza dell'astrazione sono importanti quanto le performance
. Architettura Tecnica
A. The Engine (Zig)
Il core agisce come lo strato fisico. Deve essere invisibile e privo di effetti collaterali non controllati.
PTY Management: Gestione dei processi figli tramite forkpty e segnali POSIX.
Deterministic Rendering: Rendering della griglia di caratteri ottimizzato per latenza zero.
Janet Bridge: Esposizione delle primitive di sistema a Janet tramite una FFI (Foreign Function Interface) pulita e tipizzata.
The Functional Layer (Janet)
Il vero terminale vive qui. Seguendo il SICP, il sistema è costruito tramite:
Immutabilità: Uso di tuple e struct per rappresentare lo stato dei pannelli.
Fibers: Gestione della concorrenza non bloccante per leggere l'output dei processi in parallelo senza thread pesanti.
PEG (Parsing Expression Grammars): Analisi funzionale dei flussi ANSI e dell'output dei programmi senza l'uso di Regex imperative.
4. Filosofia Linux-Style & Suckless
ZNT non reinventa la ruota, ma la fa girare meglio:
Composizione di Processi: Un "plugin" in ZNT è spesso solo un comando Linux esistente. Janet funge da colla funzionale:
(ls) -> (filter) -> (fzf) -> (render).
Everything is a Buffer: I file, l'output dei processi e le shell sono tutti astratti in "Buffers" manipolabili funzionalmente.
5. Linee Guida per il Coding Agent
Implementazione Funzionale
Evitare lo Stato Globale: Ogni funzione deve idealmente essere pura. Lo stato del terminale deve fluire attraverso il sistema (Pattern: State Passing).
Recursion over Iteration: Prediligere la ricorsione (Tail-Call Optimized in Janet) per la gestione dei nodi del layout.
Dati come Verità: Lo stato della finestra deve essere descritto da una struttura dati simbolica (S-Expression).
Regole di Codifica
Zig Core: Non allocare memoria nel loop di rendering. Usa std.mem.Allocator solo per la creazione iniziale di buffer.
Janet Logic: Tratta l'input dell'utente come un evento che genera una nuova versione dello stato dell'applicazione.
Suckless Integration: Se una funzione può essere delegata a grep, sed, o bat, scrivi un wrapper Janet invece di implementarla in Zig.
Riferimento per l'Agente
"Il codice deve essere scritto per essere letto dagli esseri umani e solo incidentalmente per essere eseguito dalle macchine." (SICP)
Tratta questo terminale come un sistema dinamico del SICM: ogni interazione dell'utente è una perturbazione che il sistema risolve tramite funzioni di transizione eleganti e pure.
