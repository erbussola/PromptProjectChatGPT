# Mini-howto
## BRIEF MASTER GitHub-first + ChatGPT Go

### 1. PREPARA IL REPOSITORY

Crea un repository GitHub privato, per esempio:

```text
chat-continuity
```

Per ogni progetto crea una directory:

```text
projects/
└── mio-progetto/
    ├── source/
    ├── work/
    ├── audit/
    └── brief/
```

Le conversazioni originali vanno in:

```text
projects/mio-progetto/source/
```

Esempio:

```text
projects/mio-progetto/source/
├── chat-001.md
├── chat-002.md
└── chat-003.md
```

**Non modificare i file sorgente per adattarli al workflow.** Devono rimanere gli archivi originali.

---

# 2. COLLEGA GITHUB A CHATGPT

Collega il repository GitHub a ChatGPT e autorizza l'accesso al repository necessario.

Per questo workflow ChatGPT deve poter:

- leggere i file;
- creare file;
- modificare file.

Usa il principio del **minimo privilegio**: se puoi autorizzare soltanto il repository interessato, è preferibile.

Non servono Work o Codex.

---

# 3. PRIMA ELABORAZIONE

Quando il progetto non possiede ancora un BRIEF:

1. apri una nuova chat;
2. inserisci il **Prompt BRIEF MASTER — GitHub-First Continuous Workflow**;
3. indica il progetto da elaborare;
4. chiedi di eseguire il workflow completo.

Puoi usare questo comando:

> Esegui il workflow BRIEF MASTER sul progetto `mio-progetto`. Utilizza come fonti primarie tutti i file presenti in `projects/mio-progetto/source/`. È una prima elaborazione. Procedi fino al completamento dell'audit finale e salva tutti gli artefatti nel repository.

ChatGPT dovrebbe quindi creare/elaborare:

```text
work/SOURCE-INVENTORY.md
work/chunks/
work/extraction/
work/CONSOLIDATED-STATE.md
work/DECISION-HISTORY.md
audit/COMPLETENESS-MATRIX.md
audit/COMPLETENESS-AUDIT.md
brief/BRIEF-MASTER.md
```

---

# 4. DOVE TROVI IL RISULTATO

Il file principale è:

```text
projects/mio-progetto/brief/BRIEF-MASTER.md
```

Questo è il file da utilizzare come **contesto di continuità per una nuova chat**.

Gli altri file sono la memoria di lavoro e la traccia di verifica.

---

# 5. AGGIUNGERE UNA NUOVA CONVERSAZIONE

Quando termini una nuova conversazione relativa allo stesso progetto:

1. esportala/salvala in Markdown;
2. assegnale un nuovo nome;
3. copiala nella directory `source/`.

Per esempio:

```text
source/
├── chat-001.md
├── chat-002.md
├── chat-003.md
└── chat-004.md    ← nuova
```

Non devi modificare manualmente il BRIEF.

---

# 6. AGGIORNARE IL BRIEF

Apri una nuova chat e usa nuovamente il prompt master.

Poi scrivi:

> Aggiorna il BRIEF MASTER del progetto `mio-progetto`. È stata aggiunta una nuova conversazione in `source/chat-004.md`. Esegui il workflow incrementale completo, verifica l'impatto sullo stato precedente e completa nuovamente l'audit di completezza.

ChatGPT deve:

1. individuare il nuovo file;
2. aggiornare l'inventario;
3. analizzare la nuova conversazione;
4. confrontarla con lo stato precedente;
5. individuare modifiche e nuove decisioni;
6. aggiornare il consolidato;
7. aggiornare la storia decisionale;
8. aggiornare il BRIEF;
9. eseguire l'audit;
10. salvare le modifiche.

---

# 7. SE UNA NUOVA CHAT CAMBIA UNA DECISIONE PRECEDENTE

Non modificare manualmente il vecchio BRIEF.

Metti semplicemente la nuova conversazione in `source/`.

Per esempio:

```text
chat-001.md
    ↓
decisione A

chat-002.md
    ↓
decisione A modificata

chat-003.md
    ↓
decisione A riaperta
```

Il workflow deve ricostruire la storia e determinare lo stato attuale.

La decisione precedente non viene cancellata dalla storia: viene classificata come modificata, riaperta o revocata, a seconda di quanto risulta dalle fonti.

---

# 8. QUANDO FARE UNA RICOSTRUZIONE COMPLETA

Chiedi esplicitamente una ricostruzione completa quando:

- hai modificato manualmente una conversazione sorgente;
- hai riorganizzato molti file;
- hai importato molte conversazioni contemporaneamente;
- sospetti che il BRIEF precedente sia incompleto;
- hai trovato un errore nel BRIEF;
- le decisioni del progetto sono diventate molto complesse;
- vuoi effettuare un controllo periodico di integrità.

Prompt:

> Ricostruisci integralmente il BRIEF MASTER del progetto `mio-progetto` utilizzando tutte le fonti presenti in `source/`. Non fidarti del BRIEF precedente come fonte primaria. Esegui l'intero workflow, inclusi consolidamento, storia decisionale, matrice di completezza e audit finale.

---

# 9. CONTROLLO RAPIDO DOPO L'ELABORAZIONE

Non è necessario rileggere tutte le conversazioni.

Controlla principalmente:

```text
brief/BRIEF-MASTER.md
audit/COMPLETENESS-AUDIT.md
audit/COMPLETENESS-MATRIX.md
```

La domanda fondamentale è:

> Tutte le conversazioni e tutti i segmenti risultano analizzati e verificati?

Se sì, il workflow ha superato il controllo di copertura.

---

# 10. UTILIZZARE IL BRIEF IN UNA NUOVA CHAT

Quando vuoi continuare il progetto in una nuova chat, puoi usare il BRIEF come contesto.

La nuova chat non deve necessariamente ricevere tutte le conversazioni originali.

Puoi fornire:

```text
brief/BRIEF-MASTER.md
```

oppure, se la chat ha accesso al repository:

> Leggi `projects/mio-progetto/brief/BRIEF-MASTER.md` e utilizza il BRIEF come contesto operativo corrente del progetto.

Il BRIEF deve essere sufficiente per riprendere il lavoro.

---

# 11. CICLO OPERATIVO CONSIGLIATO

Il ciclo normale diventa:

```text
CONVERSAZIONE
      ↓
esporta Markdown
      ↓
GitHub /source/
      ↓
CHATGPT
      ↓
aggiornamento BRIEF
      ↓
AUDIT
      ↓
GitHub /brief/
      ↓
nuova conversazione
      ↓
nuova esportazione
      ↓
GitHub /source/
      ↓
...
```

In questo modo il repository diventa la **memoria persistente del progetto**.

---

# 12. REGOLE DA RICORDARE

### NON fare

- non allegare necessariamente le conversazioni alla chat;
- non modificare manualmente il BRIEF per incorporare nuove informazioni;
- non cancellare le vecchie conversazioni;
- non usare il BRIEF precedente come unica fonte;
- non chiedere semplicemente "riassumi queste chat";
- non eliminare gli audit e gli artefatti intermedi.

### FARE

- conserva le conversazioni originali in `source/`;
- aggiungi le nuove conversazioni come nuovi file;
- usa sempre il prompt master;
- lascia che ChatGPT aggiorni il consolidato;
- lascia che ricostruisca la storia decisionale;
- esegui sempre l'audit;
- conserva il versionamento Git;
- usa `BRIEF-MASTER.md` come contesto operativo finale.

---

# 13. I TRE COMANDI PRINCIPALI

### Prima elaborazione

> Esegui il workflow BRIEF MASTER sul progetto `mio-progetto` utilizzando tutte le fonti presenti in `source/`. È una prima elaborazione completa.

### Aggiornamento

> Aggiorna il BRIEF MASTER di `mio-progetto` incorporando le nuove fonti presenti in `source/`. Determina cosa è cambiato rispetto allo stato precedente ed esegui l'audit completo.

### Ricostruzione

> Ricostruisci integralmente il BRIEF MASTER di `mio-progetto` da tutte le fonti presenti in `source/`, senza utilizzare il BRIEF precedente come fonte primaria. Esegui l'intero workflow e l'audit finale.

---

# 14. REGOLA D'ORO

Non pensare al sistema come:

**"ChatGPT fa un riassunto delle mie conversazioni."**

Pensalo come:

**"GitHub conserva la storia del progetto; ChatGPT ricostruisce periodicamente lo stato del progetto e verifica che la sintesi non abbia perso informazioni operative."**

Questa distinzione è fondamentale per mantenere affidabilità nel tempo.