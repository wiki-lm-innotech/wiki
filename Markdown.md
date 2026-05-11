# Caratteri di Controllo Markdown

> [!NOTE]
> - Per aprire l'anteprima: `Ctrl + Shift + V`
> - Per l'apice inverso (backtick) `` ` ``: `Alt + 96` (tastierino numerico)

Ecco un elenco succinto dei principali caratteri di formattazione:

- **Titoli**: `#` (H1), `##` (H2), `###` (H3), ecc.
- **Grassetto**: `**testo**` o `__testo__`
- **Corsivo**: `*testo*` o `_testo_`
- **Sbarrato**: `~~testo~~`
- **Liste puntate**: `-`, `+`, o `*`
- **Liste numerate**: `1.`, `2.`, ecc.
- **Link**: `[testo](url)`
- **Immagini**: `![alt](url)`
- **Codice inline**: `` `codice` ``
- **Blocchi di codice**: ```` ``` ```` (all'inizio e alla fine)
- **Citazioni**: `>`
- **Linea orizzontale**: `---` o `***`
- **Task list**: `- [ ]` (da fare) o `- [x]` (completato)
- **Tabelle**: `|` (colonne) e `-` (separatore intestazione)
- **Escape**: `\` (per scrivere caratteri speciali senza formattarli, es. `\*`)
- **Link automatici**: `<url>` (es. `<https://google.it>`)
- **Note a piè di pagina**: `[^1]` e `[^1]: spiegazione`
- **Emoji**: `:nome:` (es. `:smile:`)
- **Pedice/Apice**: `<sub>` e `<sup>` (tag HTML)

---

## Esempi di Utilizzo

### Titoli
# Titolo H1
## Titolo H2
### Titolo H3

### Enfasi
Questo è un **testo in grassetto**.
Questo è un *testo in corsivo*.
Questo è un ~~testo sbarrato~~.

### Liste
- Elemento puntato 1
- Elemento puntato 2
  - Sotto-elemento

1. Primo elemento numerato
2. Secondo elemento numerato

### Link e Immagini
[Visita il sito di Google](https://www.google.it)

![Logo Markdown](https://upload.wikimedia.org/wikipedia/commons/4/48/Markdown-mark.svg)

### Codice
Per evidenziare una `parola` nel testo.

```javascript
// Esempio di blocco di codice
function saluto() {
  console.log("Ciao Mondo!");
}
```

### Altri elementi
> Questa è una citazione (blockquote).

---
(Linea orizzontale sopra)

- [x] Task completata
- [ ] Task da fare

| Nome | Ruolo |
| :--- | :--- |
| Lorenzo | Utente |
| Antigravity | AI |

Questo è un esempio di escape: \*asterischi visibili\* (non corsivo).

Link automatico: <https://www.wikipedia.org>

Esempio di nota[^1].

Emoji: :rocket: :pizza:

Esempio pedice e apice: H<sub>2</sub>O, E=mc<sup>2</sup>

[^1]: Questa è la spiegazione della nota a piè di pagina.
