# 🧰 Le magie su richiesta: le Funzioni in C++

> *"Se scrivi lo stesso codice due volte, stai sbagliando qualcosa. Se lo scrivi tre volte, stai SICURAMENTE sbagliando qualcosa."*

---

## 💡 La metafora del mondo reale

Una **funzione** è una **magia nel grimorio del mago**:

- Ha un **nome** (`Palla di Fuoco`).
- Vuole degli **ingredienti** da te (i *parametri*: mana, bersaglio).
- Fa qualcosa di preciso e ti **restituisce un risultato** (il danno inflitto).
- Una volta scritta nel libro, la puoi **lanciare mille volte** senza riscriverla.

Oppure pensa a un **distributore di caffè**: infili i soldi (input), premi il bottone, e riceve il caffè (output). Non devi sapere *come* funziona dentro: devi sapere cosa gli dai e cosa ti restituisce.

In C++ anche `main()` è una funzione: quella da cui parte tutto!

---

## ⚙️ Sintassi & Codice C++

### Anatomia di una funzione

```
tipoRestituito nomeFunzione(tipo param1, tipo param2) {
    // corpo
    return valore;
}
```

```cpp
int somma(int a, int b) {        // FIRMA: restituisce un int, si chiama somma, vuole due int
    int risultato = a + b;       // corpo: il lavoro vero e proprio
    return risultato;            // return: restituisce il valore e TERMINA la funzione
}
```

| Parte | Cosa significa |
|---|---|
| **Tipo restituito** (`int`) | Che tipo di valore esce dalla funzione |
| **Nome** (`somma`) | Come la richiamo (camelCase, verbo parlante!) |
| **Parametri** (`int a, int b`) | Gli "ingredienti" richiesti: sono variabili locali alla funzione |
| **`return`** | Consegna il risultato al chiamante e chiude la funzione |

> La riga `int somma(int a, int b)` si chiama **firma** (o *intestazione*) della funzione.

### Chiamare (invocare) una funzione

```cpp
int main() {
    int x = somma(3, 4);              // chiamo la funzione: 3 e 4 sono gli ARGOMENTI
    cout << x << endl;                // 7
    cout << somma(10, 20) << endl;    // posso usare il risultato direttamente: 30
    return 0;
}
```

> Distinzione: **parametri** = le variabili nella firma (`a`, `b`); **argomenti** = i valori che passi alla chiamata (`3`, `4`).

### Funzioni `void`: fanno qualcosa ma non restituiscono nulla

```cpp
void stampaBanner() {                  // void = "vuoto": nessun valore restituito
    cout << "=== BENVENUTO NEL DUNGEON ===" << endl;
}                                      // niente return (o solo "return;")

// uso:
stampaBanner();
```

### Il prototipo: avvisare il compilatore

Il compilatore legge dall'alto verso il basso: se chiami una funzione **prima** di averla definita, si lamenta. Due soluzioni:

```cpp
#include <iostream>
using namespace std;

int calcolaDanno(int attacco, int difesa);     // PROTOTIPO: "fidati, esiste, la scrivo dopo"

int main() {
    cout << calcolaDanno(30, 12) << endl;      // ora il compilatore sa che esiste
    return 0;
}

int calcolaDanno(int attacco, int difesa) {    // DEFINIZIONE completa, sotto il main
    return attacco - difesa;
}
```

**Struttura consigliata del file:** `#include` → prototipi → `main` → definizioni.

### Ambito (scope): variabili locali

```cpp
void incantesimo() {
    int mana = 50;           // variabile LOCALE: esiste solo dentro incantesimo()
}

int main() {
    // cout << mana;         // ❌ ERRORE: main non conosce "mana"
    return 0;
}
```

Ogni funzione ha le sue variabili private: due funzioni possono avere variabili con lo stesso nome senza interferire.

### Passaggio per valore: la funzione lavora su una COPIA

Quando chiami una funzione, il valore dell'argomento viene **copiato** nel parametro. La funzione modifica la copia, **non l'originale**.

```cpp
void potenzia(int forza) {
    forza += 10;                                        // modifico la COPIA
    cout << "Dentro la funzione: " << forza << endl;    // 15
}

int main() {
    int forza = 5;
    potenzia(forza);
    cout << "Nel main: " << forza << endl;              // 5 😮 l'originale non è cambiato!
    return 0;
}
```

**Come "recuperare" il risultato?** Facendolo **restituire**:

```cpp
int potenzia(int forza) {
    return forza + 10;
}

int main() {
    int forza = 5;
    forza = potenzia(forza);        // riassegno il risultato: ora forza vale 15
    return 0;
}
```

> 💡 Esiste anche il *passaggio per riferimento* (`int &forza`), che permette di modificare l'originale. Lo scopriremo più avanti.

### Funzioni che restituiscono `bool`

Perfette per fare "domande" al programma:

```cpp
bool isPari(int n) {
    return n % 2 == 0;              // il confronto produce già un bool
}

if (isPari(8)) { cout << "pari" << endl; }
```

### Perché usare le funzioni?

| Vantaggio | Spiegazione |
|---|---|
| **Riuso** | Scrivi una volta, usi mille volte |
| **Leggibilità** | `if (isCritico(tiro))` si legge come una frase |
| **Manutenzione** | Un bug o una modifica? Un solo punto da correggere |
| **Divide et impera** | Un problema grande diventa tanti piccoli problemi |

---

## 🎨 Schema Visivo

```
  int main() {                                int calcolaDanno(int att, int dif) {
      ...                                         int danno = att - dif;
      int d = calcolaDanno(30, 12);  ────┐        return danno;      ────┐
      cout << d;                         │    }                         │
      ...                                │                              │
  }                                      │                              │
                                         │   1) CHIAMATA: gli argomenti │
                                         └──▶    30 e 12 vengono        │
                                               COPIATI in att e dif     │
                                                                        │
                                             2) la funzione calcola     │
                                                                        │
              ◀─────────────────────────────  3) RETURN: il valore 18   │
              d = 18                             torna al chiamante  ◀──┘
                                                 e la funzione finisce

  ────────────────────────────────────────────────────────────
  PASSAGGIO PER VALORE: due scatole separate

   main():                    potenzia():
   ┌────────────┐   copia    ┌────────────┐
   │ forza = 5  │ ─────────▶ │ forza = 5  │  → diventa 15
   └────────────┘            └────────────┘
   resta 5!                  (si butta via alla fine)
```

---

## 🎯 Esercizio di Spiegazione Guidata: "Il Calcolatore di Danno"

**Consegna:** il tuo eroe attacca un goblin per 3 round. Per ogni round si tira un dado a 20 facce. Il danno base è `attacco - difesa` (minimo 1). Con un tiro da 18 in su è un **colpo critico**: danno doppio. Organizza il codice con **funzioni**.

### Passo 1 — Spacchettiamo il problema ("divide et impera")

| Compito | Funzione | Parametri | Restituisce |
|---|---|---|---|
| Tirare il dado a 20 facce | `tiraDado()` | nessuno | `int` (1..20) |
| Calcolare il danno base | `calcolaDanno(att, dif)` | due `int` | `int` |
| Decidere se è critico | `isCritico(tiro)` | un `int` | `bool` |
| Stampare il titolo | `stampaBanner()` | nessuno | niente (`void`) |

**Regola d'oro:** ogni funzione fa **una cosa sola** e ha un nome che dice cosa fa.

### Passo 2 — Il codice

```cpp
#include <iostream>
#include <cstdlib>
#include <ctime>
using namespace std;

// ---------- PROTOTIPI ----------
void stampaBanner();
int  tiraDado();
int  calcolaDanno(int attacco, int difesa);
bool isCritico(int tiro);

// ---------- MAIN: legge come una storia ----------
int main() {
    srand(time(0));                          // seme per i numeri casuali

    const int ATTACCO = 30;
    const int DIFESA_GOBLIN = 12;

    stampaBanner();

    for (int round = 1; round <= 3; round++) {
        int tiro = tiraDado();
        int danno = calcolaDanno(ATTACCO, DIFESA_GOBLIN);

        cout << "Round " << round << " | dado: " << tiro;

        if (isCritico(tiro)) {
            danno = danno * 2;
            cout << " | COLPO CRITICO!";
        }
        cout << " | danno: " << danno << endl;
    }

    return 0;
}

// ---------- DEFINIZIONI ----------
void stampaBanner() {
    cout << "=== ARENA DEI GOBLIN ===" << endl;
}

int tiraDado() {
    return rand() % 20 + 1;                  // rand()%20 → 0..19, +1 → 1..20
}

int calcolaDanno(int attacco, int difesa) {
    int danno = attacco - difesa;
    if (danno < 1) {                         // il danno minimo è sempre 1
        danno = 1;
    }
    return danno;
}

bool isCritico(int tiro) {
    return tiro >= 18;                       // il confronto è già un bool
}
```

**Esempio di output:**

```
=== ARENA DEI GOBLIN ===
Round 1 | dado: 7 | danno: 18
Round 2 | dado: 19 | COLPO CRITICO! | danno: 36
Round 3 | dado: 12 | danno: 18
```

### Passo 3 — Il ragionamento dietro ogni scelta

1. **Perché il `main` è così corto?** Perché ogni dettaglio è nascosto dietro un nome parlante. Leggendo `isCritico(tiro)` capisci subito *cosa* succede senza sapere *come*.
2. **Perché `tiraDado()` non ha parametri?** Non le serve nulla in ingresso: tira sempre lo stesso dado. Ma le parentesi `()` restano, sia nella firma che nella chiamata.
3. **Perché `isCritico` restituisce un `bool`?** La domanda "è critico?" ha risposta sì/no: scrivere `if (isCritico(tiro))` si legge come una frase in italiano.
4. **Perché il `return` in `calcolaDanno` è alla fine?** Prima ci assicuriamo che il danno sia almeno 1, poi lo restituiamo. `return` chiude la funzione: tutto ciò che sta dopo non verrebbe eseguito.
5. **Perché `danno` nel `main` e `danno` in `calcolaDanno` non si "scontrano"?** Sono due variabili locali separate, in due scatole diverse. Il valore viaggia dalla funzione al main grazie al `return`.
6. **Perché i prototipi in alto?** Così `main` può stare per primo (e leggersi come indice del programma) e le definizioni di dettaglio stanno sotto.
7. **Perché `ATTACCO` e `DIFESA_GOBLIN` sono `const`?** Sono valori di gioco che non devono cambiare per errore; averli con un nome rende subito chiaro cosa rappresentano (e li modifichi in un posto solo).

> 🧠 **Sfida bonus:** crea una funzione `stampaBarraVita(int vita, int vitaMax)` che disegna `[#####-----]`. Poi aggiungi una funzione `int tiraDado(int facce)` con **un parametro**: riesci a usarla sia per un d20 sia per un d6?

---

## ✅ Ricorda

- Una funzione ha: **tipo restituito, nome, parametri, corpo, `return`**.
- `void` = non restituisce nulla.
- **Prototipo** sopra il `main`, **definizione** sotto (oppure definisci tutto prima del main).
- Le variabili dentro una funzione sono **locali**.
- **Passaggio per valore** = la funzione lavora su una **copia**: per riavere un risultato, usa `return`.
- Una funzione = **una cosa sola**, con un nome che la racconta.
