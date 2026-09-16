# RUNBOOK
## Preparazione Repository GitHub per BRIEF MASTER

### Scopo

Questo Runbook descrive come creare e inizializzare da CLI un repository GitHub destinato al workflow:

**Conversazioni → Estrazione → Consolidamento → BRIEF → Audit → Versionamento**

Il repository deve fungere da memoria persistente del progetto.

Il workflow non richiede Work né Codex.

---

# 1. PREREQUISITI

Sono necessari:

- account GitHub;
- Git installato;
- GitHub CLI (`gh`) installata;
- autenticazione GitHub CLI;
- accesso al repository che verrà collegato a ChatGPT.

Verifica:

```bash
git --version
gh --version
```

Esempio:

```text
git version 2.x.x
gh version 2.x.x
```

---

# 2. AUTENTICAZIONE GITHUB CLI

Verifica lo stato:

```bash
gh auth status
```

Se non sei autenticato:

```bash
gh auth login
```

Segui la procedura guidata.

Per HTTPS, una configurazione tipica è:

```text
GitHub.com
HTTPS
Login with a web browser
```

Al termine verifica nuovamente:

```bash
gh auth status
```

Deve risultare autenticato l'account GitHub corretto.

---

# 3. CONFIGURAZIONE GIT

Configura nome e indirizzo email, se non sono già impostati:

```bash
git config --global user.name "NOME COGNOME"
git config --global user.email "email@example.com"
```

Verifica:

```bash
git config --global --list
```

---

# 4. CREAZIONE DEL REPOSITORY

## Opzione consigliata

Crea un repository privato:

```bash
gh repo create chat-continuity \
  --private \
  --description "Persistent source, audit and continuity briefs for ChatGPT projects"
```

Per controllare che sia stato creato:

```bash
gh repo view chat-continuity
```

Per aprirlo nel browser:

```bash
gh repo view chat-continuity --web
```

---

# 5. CLONAZIONE LOCALE

Clona il repository:

```bash
gh repo clone chat-continuity
```

Entra nella directory:

```bash
cd chat-continuity
```

Verifica:

```bash
git status
```

Dovresti vedere qualcosa di simile a:

```text
On branch main
Your branch is up to date with 'origin/main'.

nothing to commit, working tree clean
```

---

# 6. CREAZIONE DELLA STRUTTURA

Crea la struttura principale:

```bash
mkdir -p projects
```

Per un nuovo progetto:

```bash
PROJECT_ID="mio-progetto"

mkdir -p \
  "projects/$PROJECT_ID/source" \
  "projects/$PROJECT_ID/work/chunks" \
  "projects/$PROJECT_ID/work/extraction" \
  "projects/$PROJECT_ID/audit" \
  "projects/$PROJECT_ID/brief"
```

Controlla:

```bash
find "projects/$PROJECT_ID" -type d | sort
```

Risultato atteso:

```text
projects/mio-progetto
projects/mio-progetto/audit
projects/mio-progetto/brief
projects/mio-progetto/source
projects/mio-progetto/work
projects/mio-progetto/work/chunks
projects/mio-progetto/work/extraction
```

---

# 7. README DEL REPOSITORY

Crea il README principale:

```bash
cat > README.md <<'EOF'
# Chat Continuity

Repository persistente per la gestione di archivi di conversazioni,
stato operativo, BRIEF MASTER e audit di completezza.

## Struttura

- `projects/` — progetti
- `source/` — conversazioni originali
- `work/` — artefatti intermedi
- `audit/` — verifiche e audit
- `brief/` — BRIEF MASTER

## Principio

Le conversazioni presenti in `source/` costituiscono le fonti primarie.

Gli altri file sono artefatti derivati dal processo di analisi.

Il BRIEF non sostituisce le fonti primarie.
EOF
```

---

# 8. README DEL PROGETTO

Crea un README specifico:

```bash
cat > "projects/$PROJECT_ID/README.md" <<EOF
# $PROJECT_ID

## Struttura

- \`source/\` — conversazioni sorgente
- \`work/\` — estrazioni e stato consolidato
- \`audit/\` — verifiche di completezza
- \`brief/\` — BRIEF MASTER

## Fonte primaria

I file presenti in \`source/\` costituiscono le fonti primarie del progetto.

## BRIEF corrente

\`brief/BRIEF-MASTER.md\`
EOF
```

---

# 9. .GITKEEP

Git non versiona directory vuote.

Se vuoi mantenere la struttura anche prima della prima elaborazione:

```bash
touch \
  "projects/$PROJECT_ID/source/.gitkeep" \
  "projects/$PROJECT_ID/work/chunks/.gitkeep" \
  "projects/$PROJECT_ID/work/extraction/.gitkeep" \
  "projects/$PROJECT_ID/audit/.gitkeep" \
  "projects/$PROJECT_ID/brief/.gitkeep"
```

---

# 10. .GITIGNORE

Crea un `.gitignore` minimale:

```bash
cat > .gitignore <<'EOF'
.DS_Store
Thumbs.db

*.tmp
*.temp
*.bak

# Editor / IDE
.vscode/
.idea/
EOF
```

### Importante

NON aggiungere:

```text
projects/*/source/
```

al `.gitignore`.

Le conversazioni sorgente devono essere versionate se GitHub è stato scelto come archivio persistente.

---

# 11. PROTEZIONE DEI DATI SENSIBILI

Prima di caricare conversazioni nel repository, verifica che non contengano informazioni che non vuoi conservare su GitHub.

In particolare controlla:

- password;
- API key;
- token;
- cookie;
- credenziali;
- dati finanziari;
- dati personali non necessari;
- segreti presenti accidentalmente nei log;
- chiavi private;
- `.env`.

Non inserire mai credenziali nel repository.

Per esempio, NON salvare:

```text
OPENAI_API_KEY=...
GITHUB_TOKEN=...
AWS_SECRET_ACCESS_KEY=...
```

Se una conversazione contiene accidentalmente un segreto, rimuovilo prima del commit e, se era già stato pubblicato, considera il segreto compromesso e procedi alla sua revoca/rotazione.

---

# 12. PRIMO COMMIT

Controlla cosa verrà committato:

```bash
git status
```

Controlla anche i file:

```bash
git diff --stat
```

Aggiungi:

```bash
git add .
```

Controlla nuovamente:

```bash
git status
```

Crea il commit:

```bash
git commit -m "Initialize Chat Continuity repository"
```

Invia a GitHub:

```bash
git push -u origin main
```

---

# 13. VERIFICA DEL REPOSITORY REMOTO

Controlla:

```bash
gh repo view chat-continuity
```

Controlla il remote:

```bash
git remote -v
```

Controlla lo stato:

```bash
git status
```

E, se vuoi verificare la cronologia:

```bash
git log --oneline --decorate --graph -10
```

---

# 14. INSERIMENTO DELLA PRIMA CONVERSAZIONE

Salva la conversazione Markdown nella directory:

```text
projects/mio-progetto/source/
```

Per esempio:

```text
projects/mio-progetto/source/chat-001.md
```

Puoi copiarla da CLI:

```bash
cp /percorso/della/conversazione.md \
   "projects/$PROJECT_ID/source/chat-001.md"
```

Verifica:

```bash
ls -lh "projects/$PROJECT_ID/source/"
```

---

# 15. CONTROLLO PRIMA DEL COMMIT

Controlla quali file Git vede come nuovi:

```bash
git status --short
```

Controlla la dimensione:

```bash
du -h "projects/$PROJECT_ID/source/chat-001.md"
```

Controlla eventualmente l'inizio del file:

```bash
head -n 20 "projects/$PROJECT_ID/source/chat-001.md"
```

e la fine:

```bash
tail -n 20 "projects/$PROJECT_ID/source/chat-001.md"
```

Questo controllo è utile per assicurarsi che l'esportazione sia effettivamente completa.

---

# 16. COMMIT DELLA CONVERSAZIONE

Aggiungi soltanto il sorgente:

```bash
git add "projects/$PROJECT_ID/source/chat-001.md"
```

Commit:

```bash
git commit -m "Add source conversation chat-001"
```

Push:

```bash
git push
```

---

# 17. PRIMA ELABORAZIONE CON CHATGPT

A questo punto il repository contiene la fonte primaria.

In ChatGPT, con GitHub collegato, utilizza il prompt:

**BRIEF MASTER — GitHub-First Continuous Workflow**

Poi impartisci:

> Esegui il workflow BRIEF MASTER sul progetto `mio-progetto`. Utilizza tutte le fonti presenti in `projects/mio-progetto/source/`. Questa è la prima elaborazione completa. Procedi fino all'audit finale e salva tutti gli artefatti nel repository.

ChatGPT dovrebbe produrre:

```text
projects/mio-progetto/work/SOURCE-INVENTORY.md
projects/mio-progetto/work/chunks/...
projects/mio-progetto/work/extraction/...
projects/mio-progetto/work/CONSOLIDATED-STATE.md
projects/mio-progetto/work/DECISION-HISTORY.md
projects/mio-progetto/audit/COMPLETENESS-MATRIX.md
projects/mio-progetto/audit/COMPLETENESS-AUDIT.md
projects/mio-progetto/brief/BRIEF-MASTER.md
```

---

# 18. VERIFICA DOPO CHATGPT

Dalla macchina locale:

```bash
git pull
```

Controlla:

```bash
git status
```

Visualizza i file modificati:

```bash
git log --oneline --decorate -10
```

Per vedere quali file sono stati modificati nell'ultimo commit:

```bash
git show --stat --oneline HEAD
```

---

# 19. AGGIUNTA DI UNA NUOVA CONVERSAZIONE

Quando una nuova conversazione termina, salvala come nuovo Markdown.

Esempio:

```bash
cp /percorso/chat-004.md \
   "projects/$PROJECT_ID/source/chat-004.md"
```

Controlla:

```bash
git status --short
```

Commit:

```bash
git add "projects/$PROJECT_ID/source/chat-004.md"
git commit -m "Add source conversation chat-004"
git push
```

Poi chiedi a ChatGPT:

> Aggiorna il BRIEF MASTER di `mio-progetto` incorporando le nuove fonti presenti in `source/`. Esegui il workflow incrementale completo e l'audit finale.

---

# 20. AGGIORNAMENTO DI UN SORGENTE ESISTENTE

Se devi correggere o aggiornare una conversazione:

```bash
git status
```

Modifica il file.

Poi:

```bash
git diff -- "projects/$PROJECT_ID/source/chat-002.md"
```

Controlla attentamente la modifica.

Commit:

```bash
git add "projects/$PROJECT_ID/source/chat-002.md"
git commit -m "Update source conversation chat-002"
git push
```

Successivamente chiedi a ChatGPT di aggiornare il BRIEF.

Se non è possibile determinare con affidabilità l'impatto della modifica, il workflow deve rianalizzare l'intera fonte.

---

# 21. VISUALIZZARE LA STORIA DEL BRIEF

Per vedere quando il BRIEF è cambiato:

```bash
git log --oneline --follow -- \
  "projects/$PROJECT_ID/brief/BRIEF-MASTER.md"
```

Per vedere una specifica modifica:

```bash
git show <COMMIT> -- \
  "projects/$PROJECT_ID/brief/BRIEF-MASTER.md"
```

Per confrontare due versioni:

```bash
git diff <COMMIT1> <COMMIT2> -- \
  "projects/$PROJECT_ID/brief/BRIEF-MASTER.md"
```

---

# 22. VERIFICA RAPIDA DI INTEGRITÀ

Per controllare lo stato del repository:

```bash
git status
```

Per vedere i file sorgente:

```bash
find "projects/$PROJECT_ID/source" -type f -name "*.md" | sort
```

Per vedere gli artefatti:

```bash
find "projects/$PROJECT_ID/work" \
     "projects/$PROJECT_ID/audit" \
     "projects/$PROJECT_ID/brief" \
     -type f | sort
```

Per contare le conversazioni:

```bash
find "projects/$PROJECT_ID/source" \
     -type f -name "*.md" | wc -l
```

---

# 23. VERIFICA CHE TUTTO SIA STATO PUSHATO

Usa:

```bash
git status
```

Il risultato desiderato è:

```text
nothing to commit, working tree clean
```

Puoi anche verificare:

```bash
git fetch
git status
```

---

# 24. BACKUP LOCALE

Il repository GitHub è il repository remoto, ma il clone locale costituisce una seconda copia.

Per aggiornarlo:

```bash
git pull
```

Per scaricare un repository nuovamente:

```bash
gh repo clone chat-continuity
```

---

# 25. COMANDI OPERATIVI QUOTIDIANI

## Aggiungere una nuova chat

```bash
cp /percorso/chat.md \
   projects/mio-progetto/source/chat-XXX.md

git add projects/mio-progetto/source/chat-XXX.md
git commit -m "Add source conversation chat-XXX"
git push
```

## Aggiornare il repository locale

```bash
git pull
```

## Controllare modifiche

```bash
git status
git diff
```

## Vedere la cronologia

```bash
git log --oneline --decorate --graph -20
```

## Vedere il BRIEF

```bash
less projects/mio-progetto/brief/BRIEF-MASTER.md
```

Su macOS puoi anche:

```bash
open projects/mio-progetto/brief/BRIEF-MASTER.md
```

Su Linux:

```bash
xdg-open projects/mio-progetto/brief/BRIEF-MASTER.md
```

---

# 26. COMANDO DI EMERGENZA — RICOSTRUZIONE COMPLETA

Se sospetti che il BRIEF sia incompleto:

Prima aggiorna il repository:

```bash
git pull
```

Poi chiedi a ChatGPT:

> Ricostruisci integralmente il BRIEF MASTER di `mio-progetto` utilizzando tutte le fonti presenti in `source/`. Non utilizzare il BRIEF precedente come fonte primaria. Verifica ogni sorgente, ricostruisci lo stato e la storia decisionale, quindi esegui un audit completo SOURCE → BRIEF e la verifica finale di continuità.

---

# 27. NON MODIFICARE MANUALMENTE GLI ARTEFATTI DERIVATI SENZA MOTIVO

In condizioni normali:

**source/**  
→ gestito dall'utente.

**work/**  
→ prodotto e aggiornato dal workflow.

**audit/**  
→ prodotto e aggiornato dal workflow.

**brief/**  
→ prodotto e aggiornato dal workflow.

Se correggi manualmente un artefatto derivato, considera la modifica durante la successiva elaborazione.

Le fonti primarie restano comunque `source/`.

---

# 28. CONVENZIONE DEI NOMI

Usa nomi semplici e stabili.

Per le conversazioni:

```text
chat-001.md
chat-002.md
chat-003.md
```

oppure, se utile:

```text
2026-09-01-chat.md
2026-09-08-chat.md
2026-09-15-chat.md
```

Evita nomi ambigui come:

```text
final.md
final2.md
nuovo.md
ultima-versione.md
```

Perché il nome del file non deve essere utilizzato dal workflow come indicatore automatico della validità.

---

# 29. REGOLA SULL'ORDINE TEMPORALE

Il numero progressivo del file NON costituisce necessariamente prova della sequenza temporale.

Quando possibile, l'ordine deve essere determinato da:

1. timestamp contenuti nella conversazione;
2. metadati affidabili;
3. informazioni presenti nel sorgente;
4. convenzioni del progetto.

Se l'ordine non è determinabile, non inventarlo.

---

# 30. PROCEDURA COMPLETA IN 10 COMANDI

Una configurazione iniziale minimale può essere eseguita così:

```bash
gh auth login

gh repo create chat-continuity \
  --private \
  --description "Persistent source, audit and continuity briefs"

gh repo clone chat-continuity

cd chat-continuity

mkdir -p projects/mio-progetto/{source,work/chunks,work/extraction,audit,brief}

touch projects/mio-progetto/source/.gitkeep
touch projects/mio-progetto/work/chunks/.gitkeep
touch projects/mio-progetto/work/extraction/.gitkeep
touch projects/mio-progetto/audit/.gitkeep
touch projects/mio-progetto/brief/.gitkeep

git add .
git commit -m "Initialize project structure"

git push
```

Dopodiché inserisci le conversazioni in:

```text
projects/mio-progetto/source/
```

e fai il relativo commit.

---

# 31. WORKFLOW COMPLESSIVO

La sequenza corretta è:

```text
                 ┌──────────────────┐
                 │  CONVERSAZIONE   │
                 └────────┬─────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │     source/      │
                 └────────┬─────────┘
                          │
                     Git commit
                          │
                          ▼
                 ┌──────────────────┐
                 │   ChatGPT Go     │
                 └────────┬─────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │     work/        │
                 │ extraction      │
                 │ consolidated    │
                 │ decision history│
                 └────────┬─────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │      brief/      │
                 │ BRIEF-MASTER.md  │
                 └────────┬─────────┘
                          │
                          ▼
                 ┌──────────────────┐
                 │      audit/      │
                 │ completeness     │
                 └────────┬─────────┘
                          │
                     eventuale
                      correzione
                          │
                          ▼
                 ┌──────────────────┐
                 │      Git         │
                 │    versioning    │
                 └──────────────────┘
```

---

# 32. PRINCIPIO OPERATIVO FINALE

Il repository deve essere considerato come:

**la memoria persistente del progetto.**

Le conversazioni sono la fonte.

Gli artefatti `work/` rappresentano l'elaborazione.

Gli artefatti `audit/` rappresentano la verifica.

`BRIEF-MASTER.md` rappresenta lo stato operativo corrente.

Git rappresenta la storia delle modifiche.

La nuova chat utilizza il BRIEF per riprendere il lavoro senza dover ricostruire ogni volta l'intera conversazione.

---

# CHECKLIST DI CONFIGURAZIONE

Prima di iniziare il primo workflow verifica:

- [ ] repository GitHub creato;
- [ ] repository impostato come Private, se necessario;
- [ ] GitHub CLI autenticata;
- [ ] Git configurato;
- [ ] repository clonato;
- [ ] directory `projects/` creata;
- [ ] directory del progetto creata;
- [ ] `source/` presente;
- [ ] `work/` presente;
- [ ] `audit/` presente;
- [ ] `brief/` presente;
- [ ] `.gitignore` configurato;
- [ ] nessun segreto presente nei sorgenti;
- [ ] prima conversazione salvata in `source/`;
- [ ] sorgente committato;
- [ ] sorgente pushato su GitHub;
- [ ] repository collegato a ChatGPT;
- [ ] ChatGPT autorizzato a leggere e modificare il repository;
- [ ] prompt BRIEF MASTER disponibile;
- [ ] pronto per la prima elaborazione.
