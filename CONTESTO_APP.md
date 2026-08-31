# Contesto dell'applicazione — Mollick Prompt Builder

## Cos'è

Un'applicazione web React (Vite, single file `src/App.jsx`) che guida i docenti nella costruzione di prompt pedagogici basati sul framework di Mollick & Mollick (2023), *Assigning AI: Seven Approaches for Students with Prompts*, Wharton School.

L'app **non genera prompt automaticamente**: è uno spazio di co-costruzione guidata. Il docente risponde a domande, il ruolo AI più adatto emerge dalle risposte, poi il docente compone il prompt componente per componente. Il risultato è un'istruzione pronta per addestrare un chatbot personalizzato (Claude Project, ChatGPT GPT, Gemini Gem, NotebookLM).

---

## Struttura tecnica

- **Stack**: React 18 + Vite 5, zero dipendenze esterne oltre a `react` e `react-dom`
- **File principale**: `src/App.jsx` (file unico, ~1550 righe)
- **Stile**: CSS-in-JS inline, palette centralizzata nell'oggetto `C`
- **I18N**: oggetto `I18N` con chiavi `it` e `en`, tutto il testo dell'app è lì
- **Stato**: gestito con `useState` nel componente principale `App`

---

## Palette colori (`C`)

```js
ink:     "#1a1814"   // sfondo header, testo principale scuro
paper:   "#faf7f2"   // sfondo pagina
cream:   "#f0ebe0"   // sfondo sezioni secondarie
rule:    "#d8d0c4"   // bordi
gold:    "#b5883a"   // accento primario (sovrattitoli, bordi enfatici)
goldlt:  "#e8c97a"   // gold chiaro
muted:   "#7c7168"   // testo secondario/grigio
accent:  "#2c4a3e"   // verde scuro (titoli sezioni nella modal guida)
```

---

## Schermate (variabile `phase`)

| Phase | Schermata |
|---|---|
| *(nessuna lingua)* | Scelta lingua (`lang === null`) |
| `"intro"` | Landing page con i 5 passi e il banner guida |
| `"table"` | Tabella riassuntiva dei 7 ruoli |
| `"questions"` | 3 domande sul contesto didattico |
| `"suggest"` | Suggerimento del ruolo con spiegazione |
| `"params"` | Configurazione parametri del ruolo scelto |
| `"compose"` | Scrittura guidata dei 5 componenti Mollick |
| `"result"` | Prompt finale, pronto per la copia |

---

## I 7 ruoli (`I18N.it.roles` / `I18N.en.roles`)

Ciascun ruolo ha:
- `id`, `icon`, `label`, `subtitle`
- `color`, `accent` (colori del ruolo)
- `when` — quando usarlo
- `theory` — base pedagogica
- `risk` — rischio principale
- `keywords` — array di parole chiave usate per il suggerimento automatico
- `params` — array di parametri da configurare (5 campi con `id`, `label`, `why`, `placeholder`)
- `buildPrompt(values)` — funzione che genera il prompt finale dai valori inseriti

| ID | Icona | Ruolo IT | Ruolo EN |
|---|---|---|---|
| `tutor` | 📚 | Tutor | Tutor |
| `mentor` | 🎯 | Mentor | Mentor |
| `coach` | 🧠 | Coach | Coach |
| `teammate` | 🤝 | Compagno di squadra | Teammate |
| `student` | ✏️ | Studente | Student |
| `simulator` | 🎭 | Simulatore | Simulator |
| `tool` | 🔧 | Strumento | Tool |

---

## I 5 componenti Mollick (`mollickComponents`)

Usati nella fase `compose` per costruire il prompt passo per passo:

| ID | Label | Titolo IT |
|---|---|---|
| `role_goal` | A — Role & Goal | Ruolo e obiettivo |
| `steps` | B — Step-by-step | Istruzioni passo per passo |
| `pedagogy` | C — Pedagogy | Principi pedagogici |
| `constraints` | D — Constraints | Vincoli espliciti |
| `personalization` | E — Personalization | Personalizzazione |

---

## Componenti React principali

| Componente | Descrizione |
|---|---|
| `App` | Componente principale, gestisce tutto lo stato |
| `AboutModal` | Modal centrata con la guida completa (IT/EN), aperta da header e landing |
| `RoleDrawer` | Drawer laterale con i 7 ruoli (slide da destra) |
| `RoleTable` | Tabella espandibile dei 7 ruoli |
| `ProgressBar` | Barra di avanzamento nelle fasi questions/suggest/params/compose |

---

## Logica di suggerimento del ruolo

La funzione `suggestRole(answers, roles)` analizza le tre risposte dell'utente e le confronta con i `keywords` di ogni ruolo usando un sistema di scoring. Restituisce `{ role, confidence, secondary }` dove:
- `confidence` è una stringa ("alta", "media", "bassa" / "high", "medium", "low")
- `secondary` è il secondo ruolo più vicino

---

## Generazione del prompt

Due modalità nel risultato finale:
1. **Auto** (`buildPrompt(paramValues)`): genera il prompt dai parametri configurati
2. **Composed** (`buildComposedPrompt(composeValues)`): assembla i 5 componenti scritti dal docente

---

## I18N — struttura chiave per lingua

```
I18N.it / I18N.en
  ├── lang, langLabel, otherLang, otherLangLabel
  ├── headerSub, headerTitle, drawerBtn
  ├── langScreen  { title, sub, btnIt, btnEn }
  ├── intro       { emoji, title, sub, steps[], startBtn, ... }
  ├── table       { backBtn, title, sub, cols[], ... }
  ├── drawer      { tag, title, cite, when, theory, risk }
  ├── questions   { label, hint, backBtn, nextBtn, finalBtn, answersTitle }
  ├── suggest     { analysisLabel, chooseLabel, confirmBtn, ... }
  ├── params      { configuring, paramLabel, whyLabel, ... }
  ├── compose     { writing, componentLabel, whyLabel, ... }
  ├── result      { title, promptLabel, selectBtn, ... }
  ├── about       { bannerTitle, bannerBtn, headerBtn, close, modalTitle, sections[] }
  ├── roles       { tutor, mentor, coach, teammate, student, simulator, tool }
  └── mollickComponents[]
```

---

## File e branch

- **Branch di sviluppo**: `claude/start-project-FFyb3`
- **Branch principale**: `main`
- **Repository**: `loujazz/mollick-prompt-builder-1`
- **File da allegare a Claude**: `src/App.jsx` (contiene tutto il codice)
