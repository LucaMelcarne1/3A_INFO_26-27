# 📦 Le scatole della memoria: Tipi di Dato, Variabili e Costanti

> *"Una variabile è una scatola con un'etichetta. Il tipo di dato decide cosa ci puoi mettere dentro: non puoi infilare una pizza in una busta da lettere."*

---

## 💡 La metafora del mondo reale

Pensa all'**inventario di Minecraft**: ogni slot ha un nome (l'oggetto) e un contenuto (la quantità). Ma immagina che esistano scatole di forme diverse:

- La scatola per le **uova** contiene solo numeri interi (3 uova, mai 3,5 uova!).
- La scatola per la **benzina** accetta anche i decimali (42,37 litri).
- Il **cartello** all'ingresso contiene testo.
- L'**interruttore della luce** ha solo due stati: acceso o spento.
- E poi c'è la scatola **sigillata con il lucchetto**: una volta chiusa non si apre più. Quella è la **costante**!

In C++ devi dichiarare *prima* che cosa metterai nella scatola, e poi non puoi cambiare idea sul tipo.

---

## ⚙️ Sintassi & Codice C++

### Dichiarare una variabile

```
tipo nomeVariabile = valore;
```

```cpp
int vite = 3;              // "dichiaro una scatola di tipo int chiamata vite e ci metto 3"
vite = 2;                  // ho perso una vita: sovrascrivo il contenuto (NON riscrivo "int")
```

### I tipi fondamentali

| Tipo | Cosa contiene | Esempio | Note |
|---|---|---|---|
| `int` | **interi** (circa ±2 miliardi) | `int punti = 1500;` | ⭐ Il tipo più usato |
| `float` | decimali (precisione bassa) | `float t = 36.6f;` | Serve la `f` finale |
| `double` | **decimali** (precisione alta) | `double pi = 3.14159;` | ⭐ Il default per i decimali |
| `char` | **un solo** carattere | `char voto = 'A';` | Apici **singoli** |
| `bool` | `true` o `false` | `bool vivo = true;` | Base di ogni decisione |
| `string` | **testo** | `string nome = "Gandalf";` | Apici **doppi**, serve `#include <string>` |

```cpp
#include <iostream>
#include <string>                       // necessario per usare string
using namespace std;

int main() {
    string nome = "Gandalf";
    cout << nome.length() << endl;       // 7 → numero di caratteri
    cout << nome[0] << endl;             // 'G' → il primo carattere (si parte da 0!)
    string saluto = "Ciao, " + nome;     // + concatena le stringhe: "Ciao, Gandalf"
    return 0;
}
```

> 💡 Un `bool` stampato con `cout` mostra `1` (vero) o `0` (falso). Per vedere `true` / `false` scrivi prima `cout << boolalpha;`.

### Costanti: la scatola col lucchetto 🔒

```cpp
const double PREZZO_POZIONE = 17.5;    // const = dopo l'assegnazione non cambia più
// PREZZO_POZIONE = 20;                // ❌ ERRORE di compilazione: è una costante!
```

Perché usarle?
- Evitano modifiche accidentali.
- Danno un **nome** ai "numeri magici": `* 0.22` non dice nulla, `* ALIQUOTA_IVA` sì.
- Se il valore cambia, lo modifichi **in un solo posto**.

> Convenzione: i nomi delle costanti si scrivono in **MAIUSCOLO_CON_UNDERSCORE**.

### Regole per i nomi delle variabili

- Iniziano con una lettera (o `_`), mai con un numero: `1vita` ❌ → `vita1` ✅
- Niente spazi né simboli strani: `punti totali` ❌ → `puntiTotali` ✅
- Convenzione **camelCase**: `puntiVitaMassimi`
- Nomi parlanti! `x` ❌ → `danno` ✅
- C++ distingue maiuscole e minuscole: `Punti` ≠ `punti`
- Non puoi usare parole riservate (`int`, `for`, `return`…)

### Il casting (conversione tra tipi)

**Conversione automatica** — da scatola piccola a grande: il compilatore lo fa da solo.

```cpp
int punti = 10;
double puntiDecimali = punti;           // ✅ 10 diventa 10.0
```

**Casting esplicito** — da grande a piccola: lo dichiari tu con `static_cast<tipo>(valore)` e **perdi informazione**.

```cpp
double prezzo = 9.99;
int prezzoIntero = static_cast<int>(prezzo);   // ⚠️ diventa 9: i decimali vengono TAGLIATI, non arrotondati!
```

### 🪤 Trappola classica: la divisione tra interi

```cpp
int a = 7, b = 2;
cout << a / b << endl;                          // 3    😱 (int / int = int, niente decimali!)
cout << static_cast<double>(a) / b << endl;     // 3.5  ✅ (basta rendere double UNO dei due)
cout << a % b << endl;                          // 1    → il resto della divisione (modulo)
```

### 🪤 Altra trappola: variabili non inizializzate

```cpp
int vite;                    // ⚠️ contiene un valore CASUALE (spazzatura in memoria)
cout << vite;                // stampa un numero imprevedibile!
```

**Buona abitudine:** inizializza sempre le variabili: `int vite = 0;`.

---

## 🎨 Schema Visivo

```
 MEMORIA DEL COMPUTER (RAM)
 ┌───────────────┬───────────────┬───────────────┬───────────────┬───────────────┐
 │  int          │  double       │  bool         │  string       │  const double │
 │  ┌─────────┐  │  ┌─────────┐  │  ┌─────────┐  │  ┌─────────┐  │  ┌─────────┐  │
 │  │   150   │  │  │  36.6   │  │  │  true   │  │  │"Gandalf"│  │  │  17.5   │  │
 │  └─────────┘  │  └─────────┘  │  └─────────┘  │  └─────────┘  │  └─────────┘  │
 │  etichetta:   │  etichetta:   │  etichetta:   │  etichetta:   │  etichetta:   │
 │   "monete"    │ "temperatura" │   "vivo"      │   "nome"      │ PREZZO_POZIONE│
 │               │               │               │               │   🔒 bloccata │
 └───────────────┴───────────────┴───────────────┴───────────────┴───────────────┘
        ▲
        └── il TIPO decide la forma della scatola,
            il NOME è l'etichetta, il VALORE è il contenuto
```

---

## 🎯 Esercizio di Spiegazione Guidata: "L'Inventario dell'RPG"

**Consegna:** il tuo eroe ha 120 monete d'oro. Ogni pozione costa 17,50 monete. Calcola quante pozioni può comprare e quanto oro gli resta.

### Passo 1 — Ragioniamo sui dati

| Dato | Valore | Che tipo di scatola? | Perché? |
|---|---|---|---|
| Nome eroe | "Aria" | `string` | È testo |
| Monete | 120 | `int` | Le monete si contano intere |
| Prezzo pozione | 17.5 | `const double` | Ha la virgola e non deve cambiare |
| Pozioni comprabili | ? | `int` | Non puoi comprare mezza pozione! |
| Oro rimasto | ? | `double` | Il prezzo è decimale, quindi anche il resto |

### Passo 2 — Il codice

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    string nome = "Aria";                   // testo → string
    int monete = 120;                       // intero → int
    const double PREZZO_POZIONE = 17.5;     // prezzo fisso → costante

    // Quante pozioni? 120 / 17.5 = 6.857...
    // Mi serve un intero: static_cast<int> TAGLIA i decimali → 6
    int pozioni = static_cast<int>(monete / PREZZO_POZIONE);

    // Quanto spendo e quanto resta?
    double speso = pozioni * PREZZO_POZIONE;   // int * double → double (conversione automatica)
    double resto = monete - speso;

    bool puoComprareAltro = resto >= PREZZO_POZIONE;   // bool: una domanda con risposta sì/no

    cout << boolalpha;                      // così i bool si stampano come true/false
    cout << nome << " compra " << pozioni << " pozioni" << endl;
    cout << "Spesa totale: " << speso << " monete" << endl;
    cout << "Oro rimasto:  " << resto << " monete" << endl;
    cout << "Può comprarne un'altra? " << puoComprareAltro << endl;

    return 0;
}
```

**Output:**

```
Aria compra 6 pozioni
Spesa totale: 105 monete
Oro rimasto:  15 monete
Può comprarne un'altra? false
```

### Passo 3 — Il ragionamento dietro ogni scelta

1. **Perché `PREZZO_POZIONE` è `const`?** Il prezzo non deve cambiare per sbaglio durante il programma: se qualcuno ci prova, il compilatore lo blocca.
2. **Perché il cast?** `monete / PREZZO_POZIONE` dà `6.857`. Noi vogliamo le pozioni *intere*: `static_cast<int>` taglia la parte decimale. Qui il "difetto" del cast è proprio ciò che ci serve!
3. **Perché `pozioni * PREZZO_POZIONE` funziona?** `int` per `double`: il compilatore allarga da solo l'`int` a `double` e il risultato è `double`.
4. **Perché un `bool` alla fine?** Perché la risposta a "ne posso comprare un'altra?" è sì/no. Tra poco useremo proprio questi `bool` per far prendere decisioni al programma.
5. **Perché `cout` mostra `105` e non `105.0`?** `cout` non scrive i decimali inutili. Per forzarli: `fixed << setprecision(1)` (ricordi `<iomanip>`?).

> 🧠 **Sfida bonus:** cosa succede se scrivi `int pozioni = monete / PREZZO_POZIONE;` senza il cast? Il compilatore potrebbe solo avvisarti (warning) e tagliare comunque: capire *perché* è un ottimo allenamento.

---

## ✅ Ricorda

- Il **tipo** decide cosa puoi mettere nella scatola, il **nome** è l'etichetta.
- `int` interi, `double` decimali, `bool` vero/falso, `char` un carattere, `string` testo.
- `const` = scatola col lucchetto: il valore non cambia più.
- Da piccolo a grande: conversione automatica. Da grande a piccolo: `static_cast` e perdi dati.
- `int / int` = `int`. Occhio alle divisioni!
- **Inizializza sempre** le variabili.
