# ⚡ 3A INF — Il Manuale di C++ della Classe

Benvenuti nel manuale **ufficiale** di C++ della 3ª A Informatica!

Qui trovi tutto quello che affrontiamo in classe, spiegato con metafore, schemi e codice da provare. Niente muri di testo: ogni capitolo ha la stessa struttura, così sai sempre dove cercare.

## 🧩 Come è fatto ogni capitolo

| Sezione | Cosa trovi |
|---|---|
| 💡 **La metafora** | Il concetto spiegato con un'analogia della vita reale (o di un videogioco) |
| ⚙️ **Sintassi & Codice C++** | Esempi commentati riga per riga |
| 🎨 **Schema Visivo** | Memoria e flusso di esecuzione disegnati in ASCII art |
| 🎯 **Esercizio guidato** | Un problema risolto passo dopo passo, col *perché* di ogni scelta |

## 📚 Indice dei capitoli

| # | Capitolo | Di cosa parla |
|---|---|---|
| 1 | [🎤 Parla con il computer: Input / Output](manuale-cpp/01-input-output.md) | `<iostream>`, `cout`, `cin`, `getline` |
| 2 | [📦 Le scatole della memoria: Tipi, Variabili e Costanti](manuale-cpp/02-tipi-di-dato-variabili-e-costanti.md) | `int`, `double`, `char`, `bool`, `string`, `const` |
| 3 | [🚦 Il bivio del destino: Condizioni e Switch](manuale-cpp/03-strutture-condizionali.md) | `if`, `else if`, `else`, `&&`, `\|\|`, `switch` |
| 4 | [🔁 Ripeti con stile: For, While e Do-While](manuale-cpp/04-cicli-di-iterazione.md) | Cicli di iterazione |
| 5 | [🚂 I vagoni del treno: Array](manuale-cpp/05-array-e-vettori.md) | Dichiarazione, caricamento, stampa, ricerca |
| 6 | [🧰 Le magie su richiesta: Funzioni](manuale-cpp/06-le-funzioni.md) | Firma, `return`, passaggio per valore |

## 🧭 Quale ciclo uso?

```
        Devo ripetere qualcosa?
                 │
     So QUANTE volte? ──── SÌ ──▶ for
                 │
                NO
                 │
   Il corpo deve girare ──── SÌ ──▶ do-while   (menu, validazione input)
   ALMENO una volta?
                 │
                NO
                 │
                 ▼
               while   (ripeti finché la condizione è vera)
```

## 🛠️ Come usare il manuale

1. **Leggi** la metafora: se capisci quella, il resto viene da sé.
2. **Copia** il codice in un tuo file `.cpp`, **compilalo** ed **eseguilo**.
3. **Rompilo** apposta: cambia un valore, togli un `;`, leggi l'errore. Si impara tantissimo così!
4. Prova le **sfide bonus** in fondo a ogni esercizio.

## ✅ Prerequisiti

Un compilatore C++ (per esempio `g++`) e un editor o IDE a scelta (Code::Blocks, VS Code, CLion, Dev-C++…).

Per compilare da terminale:

```bash
g++ -o programma main.cpp
./programma
```

## 🔜 Prossimamente

Nuovi capitoli man mano che avanziamo con il programma. Tieni d'occhio la repository!

---

*Buon coding! Ricorda: ogni programmatore ha passato ore a cercare un `;` mancante. Tu ora hai solo un vantaggio: lo sai già.* 😄
