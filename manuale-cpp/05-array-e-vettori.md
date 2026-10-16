# 🚂 I vagoni del treno: alla scoperta degli Array

> *"Hai 100 punteggi da salvare. Crei 100 variabili con 100 nomi diversi? No. Crei un treno con 100 vagoni."*

---

## 💡 La metafora del mondo reale

Un **array** (o vettore monodimensionale) è un **treno**:

- Ogni **vagone** contiene *un* valore.
- Tutti i vagoni sono **dello stesso tipo** (un treno merci non trasporta metà carbone e metà passeggeri).
- Hanno tutti **lo stesso nome** (il treno) ma un **numero** diverso (il vagone).
- Il treno ha una **lunghezza fissa**: decidi da subito quanti vagoni ha.
- ⚠️ I vagoni si numerano **a partire da 0**: il primo è il vagone 0, l'ultimo di un treno da 5 è il vagone **4**.

È anche come i **cassetti numerati** di uno schedario, o gli **slot dell'inventario** di un videogioco.

---

## ⚙️ Sintassi & Codice C++

### Dichiarare un array

```
tipo nomeArray[dimensione];
```

```cpp
const int N = 5;              // la dimensione la mettiamo in una COSTANTE (buona pratica!)
int punteggi[N];              // un treno di 5 vagoni di tipo int, ma ancora NON inizializzati
```

> ⚠️ Gli elementi di un array dichiarato così contengono **valori casuali** (spazzatura) finché non li riempi.

### Inizializzazione al momento della dichiarazione

```cpp
int voti[5] = {7, 8, 6, 9, 10};      // 5 vagoni già riempiti
int zeri[5] = {0};                   // tutti a 0 (il primo a 0, gli altri azzerati di conseguenza)
int giorni[] = {31, 28, 31};         // dimensione dedotta dal compilatore: 3
```

### Accedere a un elemento: l'**indice**

```cpp
int voti[5] = {7, 8, 6, 9, 10};

cout << voti[0] << endl;         // 7  → il PRIMO elemento (indice 0)
cout << voti[4] << endl;         // 10 → l'ULTIMO (indice N-1 = 4)
voti[2] = 8;                     // modifico il terzo vagone: ora {7, 8, 8, 9, 10}
```

### ⚠️ Attenzione agli indici fuori limite

```cpp
int voti[5];
voti[5] = 10;    // 😱 ERRORE: gli indici validi sono 0..4!
```

C++ **non ti avvisa**: scrive in una zona di memoria che non è tua e il programma può comportarsi in modo imprevedibile o andare in crash. L'indice valido va **sempre** da `0` a `N-1`.

### I tre cicli "da array" (con `for`)

#### 1) Caricamento (input dall'utente)

```cpp
const int N = 5;
int punteggi[N];

for (int i = 0; i < N; i++) {                         // i va da 0 a N-1
    cout << "Punteggio " << i + 1 << ": ";            // i+1 per mostrare "1, 2, 3..." all'utente
    cin >> punteggi[i];                               // riempio il vagone i
}
```

#### 2) Stampa

```cpp
for (int i = 0; i < N; i++) {
    cout << "Vagone " << i << ": " << punteggi[i] << endl;
}
```

#### 3) Elaborazione (somma, media, massimo...)

```cpp
int somma = 0;
for (int i = 0; i < N; i++) {
    somma += punteggi[i];                // accumulatore
}
double media = static_cast<double>(somma) / N;     // cast per non perdere i decimali!
```

### Ricerca del massimo

```cpp
int massimo = punteggi[0];              // parto assumendo che il primo sia il più grande
int posizione = 0;                      // e ricordo DOVE si trova

for (int i = 1; i < N; i++) {           // parto da 1: lo 0 l'ho già preso
    if (punteggi[i] > massimo) {        // trovato uno più grande?
        massimo = punteggi[i];          // aggiorno il record
        posizione = i;
    }
}
```

### Ricerca lineare (cerco un valore)

```cpp
int cercato;
cout << "Che punteggio cerchi? ";
cin >> cercato;

bool trovato = false;
int pos = -1;                           // -1 = "non ancora trovato" (indice impossibile)

for (int i = 0; i < N; i++) {
    if (punteggi[i] == cercato) {
        trovato = true;
        pos = i;
        break;                          // trovato! Inutile continuare
    }
}

if (trovato) {
    cout << "Trovato al vagone " << pos << endl;
} else {
    cout << "Non presente" << endl;
}
```

### 💡 Un assaggio di `vector`

Gli array hanno dimensione **fissa**. In C++ esiste anche `std::vector` (`#include <vector>`), un "treno elastico" che può allungarsi e accorciarsi. Lo vedremo più avanti: per ora padroneggia gli array classici!

---

## 🎨 Schema Visivo

```
  int punteggi[5] = {120, 85, 200, 60, 150};

   indice →   [0]    [1]    [2]    [3]    [4]
            ┌──────┬──────┬──────┬──────┬──────┐
 punteggi → │ 120  │  85  │ 200  │  60  │ 150  │
            └──────┴──────┴──────┴──────┴──────┘
   memoria:  tutti "attaccati" uno dopo l'altro, stessa dimensione

   punteggi[0] → 120     (primo vagone)
   punteggi[2] → 200
   punteggi[4] → 150     (ultimo: N-1)
   punteggi[5] → 💥      (fuori dal treno!)

  ────────────────────────────────────────────────────────────
  Ricerca del MASSIMO, passo dopo passo:

   massimo = 120
   i=1: 85  > 120 ? NO   → massimo resta 120
   i=2: 200 > 120 ? SÌ   → massimo = 200, posizione = 2
   i=3: 60  > 200 ? NO   → massimo resta 200
   i=4: 150 > 200 ? NO   → massimo resta 200
   ➜ risultato: massimo = 200 in posizione 2
```

---

## 🎯 Esercizio di Spiegazione Guidata: "La Classifica dell'Arena"

**Consegna:** 5 giocatori hanno finito una partita. Il programma legge i 5 punteggi, li stampa in una tabella, calcola la media, individua il **punteggio massimo** (e a quale giocatore appartiene) e poi permette di cercare se esiste un certo punteggio.

### Passo 1 — Pianifichiamo

| Cosa devo fare | Strumento |
|---|---|
| Salvare 5 punteggi dello stesso tipo | un **array** `int punteggi[N]` |
| Dimensione fissa e riutilizzabile | una **costante** `N` |
| Riempirlo, stamparlo, elaborarlo | **cicli `for`** (so già che sono N giri) |
| Trovare il massimo | variabile `massimo` + confronto a ogni giro |
| Cercare un valore | **ricerca lineare** con `bool trovato` |

### Passo 2 — Il codice

```cpp
#include <iostream>
#include <iomanip>
using namespace std;

int main() {
    const int N = 5;                         // numero di giocatori: UN solo posto da cambiare
    int punteggi[N];

    // ---------- 1) CARICAMENTO ----------
    for (int i = 0; i < N; i++) {
        cout << "Punteggio del giocatore " << i + 1 << ": ";
        cin >> punteggi[i];
    }

    // ---------- 2) STAMPA + SOMMA ----------
    cout << "\n=== CLASSIFICA ===" << endl;
    int somma = 0;
    for (int i = 0; i < N; i++) {
        cout << "Giocatore " << i + 1 << ": " << punteggi[i] << endl;
        somma += punteggi[i];                // approfitto del giro per sommare
    }

    double media = static_cast<double>(somma) / N;
    cout << fixed << setprecision(1);
    cout << "Media: " << media << endl;

    // ---------- 3) MASSIMO ----------
    int massimo = punteggi[0];
    int posMax = 0;
    for (int i = 1; i < N; i++) {
        if (punteggi[i] > massimo) {
            massimo = punteggi[i];
            posMax = i;
        }
    }
    cout << "Il migliore è il giocatore " << posMax + 1
         << " con " << massimo << " punti!" << endl;

    // ---------- 4) RICERCA ----------
    int cercato;
    cout << "\nCerchi un punteggio? Scrivilo: ";
    cin >> cercato;

    bool trovato = false;
    for (int i = 0; i < N; i++) {
        if (punteggi[i] == cercato) {
            cout << "Trovato! Appartiene al giocatore " << i + 1 << endl;
            trovato = true;
            break;
        }
    }
    if (!trovato) {
        cout << "Nessun giocatore ha " << cercato << " punti." << endl;
    }

    return 0;
}
```

**Esempio di esecuzione:**

```
Punteggio del giocatore 1: 120
Punteggio del giocatore 2: 85
Punteggio del giocatore 3: 200
Punteggio del giocatore 4: 60
Punteggio del giocatore 5: 150

=== CLASSIFICA ===
Giocatore 1: 120
Giocatore 2: 85
Giocatore 3: 200
Giocatore 4: 60
Giocatore 5: 150
Media: 123.0
Il migliore è il giocatore 3 con 200 punti!

Cerchi un punteggio? Scrivilo: 60
Trovato! Appartiene al giocatore 4
```

### Passo 3 — Il ragionamento dietro ogni scelta

1. **Perché una costante `N` e non scrivere `5` ovunque?** Se domani i giocatori diventano 8, cambi **una riga** invece di cercare ogni `5` nel codice (e rischiare di dimenticarne uno).
2. **Perché `i < N` e non `i <= N`?** Gli indici validi sono `0..N-1`. Con `<=` l'ultimo giro userebbe `punteggi[5]`: fuori dal treno!
3. **Perché stampo `i + 1` all'utente?** L'indice parte da 0, ma "giocatore 0" suona strano. Il computer ragiona con `i`, gli umani con `i + 1`.
4. **Perché `massimo = punteggi[0]` e non `massimo = 0`?** Se tutti i punteggi fossero negativi, partire da 0 darebbe un massimo sbagliato. Partire dal primo elemento è sempre corretto.
5. **Perché il ciclo del massimo parte da `i = 1`?** L'elemento 0 è già dentro `massimo`: confrontarlo con se stesso sarebbe un giro sprecato.
6. **Perché `break` nella ricerca?** Appena trovato il valore, continuare sarebbe uno spreco.
7. **Perché serve `bool trovato`?** Dopo il ciclo devo sapere *se* l'ho trovato, per decidere se stampare "non presente". Il `bool` è la "memoria" di ciò che è successo durante il ciclo.
8. **Perché il cast `static_cast<double>(somma)`?** Altrimenti `somma / N` sarebbe una divisione tra interi e perderebbe i decimali (ricordi il Cap. 2?).

> 🧠 **Sfida bonus:** trova anche il punteggio **minimo**, e conta quanti giocatori hanno superato la media. Poi stampa la classifica **al contrario** (da `N-1` a `0`): come cambia il `for`?

---

## ✅ Ricorda

- Un array è un **treno di vagoni** dello stesso tipo: `tipo nome[N];`
- Gli indici vanno da **0 a N-1**. Fuori da lì: pericolo!
- Si scorre con un `for`: `for (int i = 0; i < N; i++)`
- Usa una **costante** per la dimensione.
- Pattern fondamentali: **caricamento, stampa, somma/media, massimo, ricerca lineare**.
