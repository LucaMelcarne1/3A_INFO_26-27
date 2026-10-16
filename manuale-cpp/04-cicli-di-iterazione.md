# 🔁 Ripeti con stile: For, While e Do-While

> *"Il bravo programmatore è pigro nel modo giusto: non scrive 100 volte la stessa riga, scrive un ciclo."*

---

## 💡 La metafora del mondo reale

Il tuo prof ti punisce: *"Scrivi 10 volte: non userò più il telefono in classe."*

Puoi scrivere la frase a mano 10 volte (da principiante)... oppure dire a un robot di farlo per te. Ma *come* gli dai l'ordine cambia a seconda della situazione:

| Ciclo | Frase al robot | Esempio nerd |
|---|---|---|
| 🏋️ **for** | *"Fai 10 giri esatti."* | "3 serie da 10 push-up" |
| 🐉 **while** | *"FINCHÉ il boss è vivo, attacca."* | Boss fight: non sai quanti colpi servono |
| 🧪 **do-while** | *"Fallo una volta, POI chiedimi se ripeterlo."* | Il menu di gioco: lo vedi sempre almeno una volta |

---

## ⚙️ Sintassi & Codice C++

### 🏋️ Il ciclo `for`: so già quante volte

```
for (inizializzazione; condizione; aggiornamento) {
    // corpo: le istruzioni che si ripetono
}
```

| Parte | Quando viene eseguita | Cosa fa |
|---|---|---|
| **Inizializzazione** | **una sola volta**, all'inizio | Crea il contatore: `int i = 0` |
| **Condizione** | **prima di ogni giro** | Se `true` → entra, se `false` → il ciclo finisce |
| **Aggiornamento** | **alla fine di ogni giro** | Cambia il contatore: `i++` |

```cpp
for (int i = 1; i <= 5; i++) {                      // parte da 1, finché i <= 5, i aumenta di 1
    cout << "Giro numero " << i << endl;            // eseguito 5 volte
}
// Dopo il for, la variabile i NON ESISTE più (vive solo dentro il ciclo)
```

**Varianti utili:**

```cpp
for (int i = 0; i < 5; i++)         { ... }   // da 0 a 4: 5 giri (lo standard dei programmatori!)
for (int i = 10; i > 0; i--)        { ... }   // conto alla rovescia: 10, 9, 8 ... 1
for (int i = 0; i <= 100; i += 10)  { ... }   // salti di 10: 0, 10, 20 ... 100
```

> 💡 `i++` = `i = i + 1`; `i--` = `i = i - 1`; `i += 10` = `i = i + 10`.

**Accumulatore (somma):**

```cpp
int somma = 0;                       // l'ACCUMULATORE: parte da zero (elemento neutro della somma)
for (int i = 1; i <= 10; i++) {
    somma += i;                      // a ogni giro aggiungo il valore corrente di i
}
cout << "1+2+...+10 = " << somma << endl;    // 55
```

**Cicli annidati** (un ciclo dentro un altro):

```cpp
for (int riga = 1; riga <= 4; riga++) {          // ESTERNO: quante righe
    for (int col = 1; col <= riga; col++) {      // INTERNO: quante stelle in questa riga
        cout << "*";
    }
    cout << endl;                                 // a capo a fine riga
}
```

```
*
**
***
****
```

### 🐉 Il ciclo `while`: ripeti finché la condizione è vera

```
while (condizione) {
    // corpo: si ripete FINCHÉ la condizione è true
}
```

La condizione si controlla **PRIMA** di ogni giro: se è `false` già all'inizio, il corpo **non gira nemmeno una volta**.

```cpp
int energia = 3;                                    // 1) preparo la variabile da controllare

while (energia > 0) {                               // 2) controllo: finché ho energia...
    cout << "Corro! Energia: " << energia << endl;
    energia--;                                      // 3) ⚠️ AGGIORNO, altrimenti il ciclo non finisce mai
}
cout << "Sono esausto" << endl;
```

**Le tre regole per un `while` sano:**
1. **Inizializza** la variabile usata nella condizione *prima* del ciclo.
2. Scrivi una **condizione** chiara.
3. **Modifica** nel corpo qualcosa che prima o poi la renda falsa. Se dimentichi: **ciclo infinito** 💥 (nel terminale: `Ctrl + C`).

**Pattern del valore sentinella** ("0 per terminare"):

```cpp
int somma = 0, numero;

cout << "Inserisci un numero (0 per terminare): ";
cin >> numero;                                  // 1) PRIMA lettura, fuori dal ciclo

while (numero != 0) {                           // 2) finché non arriva la sentinella
    somma += numero;                            // 3) elaboro il numero
    cout << "Prossimo numero (0 per terminare): ";
    cin >> numero;                              // 4) NUOVA lettura: aggiorna la condizione
}
cout << "Somma totale: " << somma << endl;
```

### 🧪 Il ciclo `do-while`: prima agisci, poi controlli

```
do {
    // corpo: eseguito ALMENO UNA VOLTA
} while (condizione);        // ⚠️ qui serve il PUNTO E VIRGOLA!
```

La condizione si controlla **DOPO** il corpo, quindi il corpo gira **sempre almeno una volta**.

```cpp
int x = 100;

while (x < 10) {                    // controlla PRIMA: false → il corpo NON gira mai
    cout << "while" << endl;
}

do {
    cout << "do-while" << endl;     // gira UNA volta, poi controlla: false → esce
} while (x < 10);
```

Stampa **solo** `do-while`.

**Pattern classico: validazione dell'input**

```cpp
int voto;                           // dichiarata FUORI: deve essere visibile nella condizione

do {
    cout << "Inserisci un voto da 1 a 10: ";
    cin >> voto;
    if (voto < 1 || voto > 10) {
        cout << "Valore non valido, riprova!" << endl;
    }
} while (voto < 1 || voto > 10);    // ripeti SE il voto è fuori range

cout << "Voto accettato: " << voto << endl;
```

### 🧭 Quale ciclo scelgo?

| Situazione | Scelta |
|---|---|
| Conosco in anticipo il numero di ripetizioni | `for` |
| Ripeto finché una condizione imprevedibile resta vera | `while` |
| Il corpo deve girare **almeno una volta** (menu, validazione) | `do-while` |

### Controlli di flusso extra

```cpp
break;       // esce subito dal ciclo
continue;    // salta il resto del giro e passa al prossimo controllo
```

### Gli errori classici

| Errore | Effetto |
|---|---|
| `i <= 5` invece di `i < 5` partendo da 0 | Un giro **in più** (*off-by-one*) |
| Dimenticare l'aggiornamento (`i++`) | **Ciclo infinito** 🔥 |
| `for (...);` con punto e virgola subito dopo | Il corpo non fa parte del ciclo! |
| Dimenticare il `;` dopo `while (...)` nel do-while | Errore di compilazione |

---

## 🎨 Schema Visivo

### Il `for`

```
for (int i = 1; i <= 3; i++) { cout << i; }

          ┌─────────────────┐
          │ 1. int i = 1    │  ← INIZIALIZZAZIONE (una volta sola)
          └────────┬────────┘
                   ▼
          ┌─────────────────┐   NO
    ┌────▶│ 2. i <= 3 ?     │────────────▶ ESCI dal ciclo 🚪
    │     └────────┬────────┘
    │              │ SÌ
    │              ▼
    │     ┌─────────────────┐
    │     │ 3. CORPO:       │
    │     │    cout << i    │
    │     └────────┬────────┘
    │              ▼
    │     ┌─────────────────┐
    └─────│ 4. i++          │  ← AGGIORNAMENTO
          └─────────────────┘

  ┌───────┬───────────┬──────────┬────────────┐
  │ Giro  │ i prima   │ i <= 3 ? │ Stampa     │
  ├───────┼───────────┼──────────┼────────────┤
  │   1   │    1      │   SÌ     │    1       │
  │   2   │    2      │   SÌ     │    2       │
  │   3   │    3      │   SÌ     │    3       │
  │  (4)  │    4      │   NO ❌  │  (esce)    │
  └───────┴───────────┴──────────┴────────────┘
```

### `while` contro `do-while`

```
      while:                          do-while:

   ┌──────────────┐                ┌──────────────┐ ◀──────┐
┌─▶│  CONDIZIONE? │── NO ──▶ fine  │    CORPO     │        │
│  └──────┬───────┘                └──────┬───────┘        │
│        SÌ                                ▼               │
│         ▼                         ┌──────────────┐       │
│  ┌──────────────┐                 │ CONDIZIONE?  │─ SÌ ──┘
└──│    CORPO     │                 └──────┬───────┘
   └──────────────┘                       NO
                                           ▼
                                         fine

 Il corpo può girare 0 volte      Il corpo gira ALMENO 1 volta
```

---

## 🎯 Esercizio di Spiegazione Guidata: "Il Drago Lagghoso"

**Consegna:** il tuo eroe si allena per 5 giorni (il giorno *n* guadagna *n* punti forza). Poi sceglie la difficoltà da 1 a 3 (da riproporre finché non è valida). Il Drago ha `100 × difficoltà` punti vita. A ogni turno il danno è un numero casuale tra 5 e 25, più la forza accumulata. Simula il combattimento.

### Passo 1 — Quale ciclo per quale pezzo?

| Parte del problema | Ci chiediamo... | Ciclo |
|---|---|---|
| Allenamento di 5 giorni | So quante volte? **Sì, 5** | `for` |
| Scelta della difficoltà valida | Va chiesta **almeno una volta**, poi ripetuta se sbagliata | `do-while` |
| Combattimento col drago | Quanti turni? **Non lo so**: finché il drago è vivo | `while` |

Questo è il **ragionamento del programmatore**: prima analizzo, poi scelgo lo strumento.

### Passo 2 — Il codice

```cpp
#include <iostream>
#include <cstdlib>      // rand() e srand()
#include <ctime>        // time()
using namespace std;

int main() {
    srand(time(0));     // "mescola il mazzo": numeri casuali diversi a ogni esecuzione

    // ---------- 1) FOR: allenamento di 5 giorni ----------
    int forza = 0;                               // accumulatore FUORI dal ciclo
    for (int giorno = 1; giorno <= 5; giorno++) {
        forza += giorno;                         // giorno 1: +1, giorno 2: +2, ...
        cout << "Giorno " << giorno << ": forza = " << forza << endl;
    }

    // ---------- 2) DO-WHILE: scelta della difficoltà ----------
    int difficolta;                              // dichiarata fuori: la condizione deve vederla
    do {
        cout << "\nDifficoltà (1 = facile, 2 = media, 3 = epica): ";
        cin >> difficolta;
        if (difficolta < 1 || difficolta > 3) {
            cout << "Valore non valido, riprova!" << endl;
        }
    } while (difficolta < 1 || difficolta > 3);

    // ---------- 3) WHILE: boss fight ----------
    int vitaBoss = 100 * difficolta;             // INIZIALIZZO la variabile della condizione
    int turno = 0;
    cout << "\nIl Drago Lagghoso appare! (" << vitaBoss << " HP)" << endl;

    while (vitaBoss > 0) {                       // finché il boss è vivo
        turno++;
        int danno = rand() % 21 + 5 + forza;     // rand()%21 → 0..20, +5 → 5..25, + forza
        vitaBoss -= danno;                       // AGGIORNO: ci avviciniamo alla fine

        if (vitaBoss < 0) {                      // evitiamo "vita negativa"
            vitaBoss = 0;
        }
        cout << "Turno " << turno << ": colpisci per " << danno
             << " -> HP boss: " << vitaBoss << endl;
    }

    cout << "\nBoss sconfitto in " << turno << " turni!" << endl;
    return 0;
}
```

**Esempio di output (la parte casuale varia!):**

```
Giorno 1: forza = 1
Giorno 2: forza = 3
Giorno 3: forza = 6
Giorno 4: forza = 10
Giorno 5: forza = 15

Difficoltà (1 = facile, 2 = media, 3 = epica): 5
Valore non valido, riprova!

Difficoltà (1 = facile, 2 = media, 3 = epica): 1

Il Drago Lagghoso appare! (100 HP)
Turno 1: colpisci per 32 -> HP boss: 68
Turno 2: colpisci per 25 -> HP boss: 43
Turno 3: colpisci per 38 -> HP boss: 5
Turno 4: colpisci per 29 -> HP boss: 0

Boss sconfitto in 4 turni!
```

### Passo 3 — Il ragionamento dietro ogni scelta

1. **Perché `forza` è fuori dal `for`?** Se la dichiarassimo dentro, ripartirebbe da 0 a ogni giro e dopo il ciclo non esisterebbe più: ci serve per il combattimento!
2. **Perché `do-while` per la difficoltà?** Dobbiamo chiedere il valore **almeno una volta**: prima di sapere se è valido, dobbiamo averlo letto. Con un `while` dovremmo inventare un valore falso per farlo partire.
3. **Perché `difficolta` è dichiarata fuori dal `do`?** La condizione `while (...)` sta *fuori* dalle graffe e non vedrebbe una variabile dichiarata dentro.
4. **Perché `while` per il boss?** Non sappiamo in quanti turni cadrà: dipende dal caso. Il numero di turni è un *risultato* che scopriamo alla fine.
5. **Perché `vitaBoss -= danno` è la riga "salvavita"?** È ciò che rende prima o poi falsa la condizione. Senza, il drago sarebbe immortale e il programma girerebbe per sempre.
6. **Perché `srand(time(0))`?** Senza, `rand()` darebbe sempre la stessa sequenza a ogni esecuzione. L'orologio del PC è il "seme" sempre diverso.

> 🧠 **Sfida bonus:** se scrivi una lettera invece di un numero alla difficoltà, il programma va in loop infinito (`cin` si "blocca" in stato di errore). Come potresti difenderti? Cerca `cin.fail()` e `cin.clear()`!

---

## ✅ Ricorda

- **`for`**: so quante volte. Tre parti: *inizio; condizione; aggiornamento*.
- **`while`**: ripeti finché la condizione è vera. Può girare **0 volte**.
- **`do-while`**: prima il corpo, poi il controllo. Gira **almeno 1 volta**. Punto e virgola finale!
- Gli accumulatori (`somma`, `totale`) si dichiarano **fuori** dal ciclo.
- Attenzione a `<` vs `<=` e ai cicli infiniti.
