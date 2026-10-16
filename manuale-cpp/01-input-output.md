# 🎤 Parla con il computer: Input / Output in C++

> *"Un programma che non comunica è come un amico che non risponde ai messaggi: tecnicamente esiste, ma a che serve?"*

---

## 💡 La metafora del mondo reale

Immagina di entrare in un fast food con il chiosco touchscreen:

- Lo **schermo** che ti mostra il menù e il conto è l'**Output** (`cout`): il programma ti *parla*.
- Il **dito** con cui tocchi "Doppio cheeseburger" è l'**Input** (`cin`): tu *parli* al programma.

In C++ il chiosco funziona con dei **tubi** (in inglese *stream*, "flussi"): i dati scorrono dentro e fuori dal programma. E le frecce `<<` e `>>` ti dicono **in che direzione scorre il tubo**.

---

## ⚙️ Sintassi & Codice C++

### Lo scheletro di ogni programma

```cpp
#include <iostream>          // 1) porta in classe la libreria per input/output
using namespace std;         // 2) così possiamo scrivere cout invece di std::cout

int main() {                 // 3) il programma parte SEMPRE da main
    cout << "Ciao, mondo!" << endl;   // 4) stampa e va a capo
    return 0;                // 5) "tutto è andato bene" (0 = successo)
}
```

> 💡 `using namespace std;` è una comodità per la scuola. Senza di essa dovresti scrivere `std::cout`, `std::cin`, `std::endl` ovunque: valido, ma più faticoso.

### Output: `cout` e l'operatore `<<`

La freccia `<<` si legge *"manda nel tubo"*. Puoi concatenare più pezzi:

```cpp
cout << "Punti: " << 150 << endl;           // testo + numero, tutto insieme
cout << "Riga uno\n";                        // \n = a capo
cout << "Una" << " " << "frase" << endl;     // più pezzi concatenati
```

| Elemento | Cosa fa |
|---|---|
| `cout << x` | Stampa `x` sullo schermo |
| `endl` | A capo (e svuota il buffer) |
| `'\n'` oppure `"\n"` | A capo (più veloce di `endl`) |
| `\t` | Tabulazione (una "spaziatura larga") |

### Input: `cin` e l'operatore `>>`

La freccia `>>` si legge *"prendi dal tubo e metti dentro"*:

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string nome;                          // 1) PRIMA preparo le scatole (variabili)
    int eta;

    cout << "Come ti chiami? ";
    getline(cin, nome);                   // 2) legge una RIGA intera (spazi inclusi)

    cout << "Quanti anni hai? ";
    cin >> eta;                           // 3) legge un numero e lo mette in eta

    cout << "Ciao " << nome << ", tra un anno ne avrai " << eta + 1 << "!" << endl;
    return 0;
}
```

**Cosa usare per leggere cosa:**

| Strumento | Legge | Si ferma a… |
|---|---|---|
| `cin >> x` | un valore (numero, parola, carattere) | primo spazio / a capo |
| `getline(cin, s)` | una riga intera in una `string` | a capo |

Puoi leggere più valori in una sola riga: `cin >> a >> b >> c;` (l'utente li separa con spazio o INVIO).

### ⚠️ La trappola del `getline` dopo `cin >>`

Il bug più famoso. Guarda:

```cpp
int livello;
string nome;

cout << "Livello: ";
cin >> livello;                   // l'utente scrive 5 e preme INVIO
cout << "Nome: ";
getline(cin, nome);               // 😱 legge subito "" (stringa vuota)!
```

**Perché?** `cin >> livello` prende il `5` ma lascia nel buffer l'**INVIO**. Il `getline` trova subito quell'INVIO, pensa "riga finita!" e restituisce una stringa vuota.

**Soluzione:** butta via l'INVIO avanzato con `cin.ignore()`.

```cpp
cin >> livello;
cin.ignore();                     // 🍽️ spazzola l'INVIO rimasto
getline(cin, nome);               // ora funziona!
```

### Formattare l'output con `<iomanip>`

```cpp
#include <iomanip>                              // libreria per la formattazione

cout << fixed << setprecision(2) << 3.14159;    // 3.14  → sempre 2 decimali
cout << setw(10) << "Luca";                     // "      Luca" → occupa 10 caratteri
cout << left << setw(10) << "Luca";             // "Luca      " → allineato a sinistra
```

| Manipolatore | Effetto |
|---|---|
| `fixed` + `setprecision(n)` | `n` cifre dopo la virgola |
| `setw(n)` | riserva `n` caratteri per il **prossimo** valore |
| `left` / `right` | allineamento (resta attivo finché non lo cambi) |

---

## 🎨 Schema Visivo

```
   TASTIERA                    PROGRAMMA                      SCHERMO
  ┌────────┐   "Luca⏎"      ┌──────────────┐               ┌──────────────┐
  │ ⌨️ ⌨️ ⌨️ │ ─── cin >> ──▶ │   nome =     │               │              │
  └────────┘                │   "Luca"     │               │              │
                            │      │       │               │              │
                            │      ▼       │               │              │
                            │ cout << nome │ ── cout << ──▶│  Ciao Luca!  │
                            └──────────────┘               └──────────────┘

   Le frecce indicano il verso del flusso:
        cin  >>  variabile      (dal mondo ───▶ dentro la variabile)
        cout <<  valore         (dalla variabile ───▶ fuori sullo schermo)

   💡 Trucco: le "punte" delle frecce guardano nella direzione dei dati!
```

---

## 🎯 Esercizio di Spiegazione Guidata: "La Scheda del Tuo Eroe RPG"

**Consegna:** il programma chiede livello, altezza e nome del personaggio e stampa una scheda ordinata.

### Passo 1 — Ragioniamo prima di scrivere

Il programmatore non parte mai dalla tastiera: parte dalla domanda *"quali dati mi servono e di che tipo sono?"*

- Livello → numero intero → `int`
- Altezza → numero con la virgola → `double`
- Nome → testo con possibili spazi ("Aria la Rossa") → `string`

### Passo 2 — Il codice

```cpp
#include <iostream>
#include <string>
#include <iomanip>
using namespace std;

int main() {
    int livello;
    double altezza;
    string nome;

    // Leggiamo prima i numeri...
    cout << "Livello del tuo eroe: ";
    cin >> livello;

    cout << "Altezza in metri (es. 1.85): ";
    cin >> altezza;

    cin.ignore();                         // 🍽️ ripulisco l'INVIO lasciato da cin >>

    // ...poi il testo (può contenere spazi!)
    cout << "Nome dell'eroe: ";
    getline(cin, nome);

    // Scheda formattata
    cout << "\n";
    cout << "╔══════════════════════════╗\n";
    cout << "║ Nome:    " << left << setw(15) << nome << " ║\n";
    cout << "║ Livello: " << left << setw(15) << livello << " ║\n";
    cout << "║ Altezza: " << left << setw(15) << fixed << setprecision(2) << altezza << " ║\n";
    cout << "╚══════════════════════════╝" << endl;

    return 0;
}
```

### Passo 3 — Perché l'abbiamo scritto così?

1. **Perché dichiariamo le variabili all'inizio?** In C++ una variabile deve esistere *prima* di poterla riempire con `cin >>`: prima prepariamo le scatole, poi le riempiamo.
2. **Perché `getline` per il nome e non `cin >>`?** `cin >>` si ferma al primo spazio: con "Aria la Rossa" leggerebbe solo "Aria". `getline` prende tutta la riga.
3. **Perché `cin.ignore()`?** Per eliminare l'INVIO rimasto nel buffer dopo `cin >> altezza`. Senza di lui, `getline` leggerebbe un nome vuoto.
4. **Perché `setw(15)` e `left`?** Riserva 15 caratteri e allinea a sinistra: la scheda resta dritta anche con nomi di lunghezze diverse.
5. **Perché `fixed << setprecision(2)`?** Per mostrare sempre due decimali (`1.80` invece di `1.8`).

> 🧠 **Sfida bonus:** aggiungi una riga "Classe" (Guerriero, Mago, Ladro…). Dovrai leggere un'altra stringa: dove metti il `cin.ignore()`?

---

## ✅ Ricorda

- `#include <iostream>` serve per `cin` e `cout`.
- `cout <<` → il programma parla; `cin >>` → il programma ascolta.
- Per leggere una riga con spazi: `getline(cin, variabile)`.
- `getline` dopo `cin >>`? **Pulisci il buffer con `cin.ignore()`!**
