# 🚦 Il bivio del destino: Strutture Condizionali e Switch

> *"Un programma senza decisioni è una playlist: parte, va avanti, finisce. Con gli `if` diventa un videogioco: dipende da come giochi."*

---

## 💡 La metafora del mondo reale

Sei davanti a una **porta di un dungeon** con un buttafuori troll:

- *SE* hai la chiave → entri.
- *ALTRIMENTI SE* hai abbastanza monete → paghi e entri.
- *ALTRIMENTI* → il troll ti lancia lontano.

Il troll controlla le condizioni **dall'alto verso il basso** e si ferma alla **prima** che è vera. Questo è un `if / else if / else`.

E lo `switch`? È il **menu di selezione personaggio**: premi 1 per il Guerriero, 2 per il Mago, 3 per il Ladro. Ogni tasto porta a un "corridoio" diverso. Utile quando devi confrontare **una sola cosa** con tanti valori precisi.

---

## ⚙️ Sintassi & Codice C++

### La struttura base

```cpp
if (condizione) {
    // eseguito SOLO se la condizione è true
} else if (altraCondizione) {
    // eseguito se la prima era false e questa è true
} else {
    // eseguito se TUTTE le condizioni precedenti erano false
}
```

- La condizione è un'espressione che produce un **`bool`** (in C++ anche i numeri funzionano: `0` = falso, ogni altro numero = vero).
- `else if` ed `else` sono **opzionali**.
- `else` è uno solo ed è sempre l'ultimo.

```cpp
int vita = 25;

if (vita <= 0) {                               // 1) condizione più "grave" per prima
    cout << "Game Over 💀" << endl;
} else if (vita < 30) {                        // 2) solo se la 1 è falsa
    cout << "Attenzione, vita bassa!" << endl;
} else {                                       // 3) tutti gli altri casi
    cout << "Stai benone 💪" << endl;
}
```

### Operatori relazionali (confronti → restituiscono `bool`)

| Operatore | Significa | Esempio | Risultato |
|---|---|---|---|
| `==` | uguale a | `5 == 5` | `true` |
| `!=` | diverso da | `5 != 3` | `true` |
| `>` | maggiore | `3 > 5` | `false` |
| `<` | minore | `3 < 5` | `true` |
| `>=` | maggiore o uguale | `5 >= 5` | `true` |
| `<=` | minore o uguale | `6 <= 5` | `false` |

> ⚠️ **`=` assegna, `==` confronta!** `if (vita = 0)` **compila** ma è un bug subdolo: *assegna* 0 a `vita` e la condizione risulta falsa. Leggi `=` come "diventa" e `==` come "è uguale a?".

### Operatori logici (combinano più condizioni)

| Operatore | Nome | Vero quando… | Esempio |
|---|---|---|---|
| `&&` | AND | **entrambe** sono vere | `eta >= 18 && haBiglietto` |
| `\|\|` | OR | **almeno una** è vera | `haChiave \|\| haScasso` |
| `!` | NOT | inverte il valore | `!haPagato` |

**Tabelle di verità:**

```
  A      B    │  A && B  │  A || B       A   │  !A
 ─────────────┼──────────┼─────────     ─────┼──────
 true  true   │   true   │   true       true │ false
 true  false  │   false  │   true       false│ true
 false true   │   false  │   true
 false false  │   false  │   false
```

### ✅ Buona notizia: in C++ le stringhe si confrontano con `==`

```cpp
string password;
getline(cin, password);

if (password == "magia") {          // ✅ in C++ con string funziona benissimo!
    cout << "Accesso consentito" << endl;
}
```

(Attenzione: il confronto distingue maiuscole da minuscole: `"Magia"` ≠ `"magia"`.)

### Condizioni annidate

```cpp
if (haChiave) {
    if (livello >= 5) {
        cout << "Il portone si apre!" << endl;
    } else {
        cout << "Hai la chiave ma sei troppo debole." << endl;
    }
}
```

Quando possibile, **semplifica con `&&`**: `if (haChiave && livello >= 5)`.

### Lo `switch`: tanti valori, una sola variabile

```cpp
char scelta = 'M';

switch (scelta) {                              // 1) la variabile da controllare
    case 'G':                                  // 2) se scelta == 'G'
        cout << "Hai scelto il Guerriero ⚔️" << endl;
        break;                                 // 3) ESCI dallo switch!
    case 'M':
        cout << "Hai scelto il Mago 🧙" << endl;
        break;
    case 'L':
        cout << "Hai scelto il Ladro 🗡️" << endl;
        break;
    default:                                   // 4) nessun caso corrisponde (come l'else)
        cout << "Classe non valida" << endl;
}
```

**Regole dello `switch`:**

- Funziona con **interi** e **`char`** (non con `string` né `double`).
- Ogni `case` confronta per **uguaglianza esatta**: niente `>` o `<`.
- ⚠️ Senza `break`, l'esecuzione **"cade" nel case successivo** (*fall-through*): è la causa di bug classici!
- `default` è opzionale ma consigliato.

**Quando uso `if` e quando `switch`?**

| Situazione | Scelta |
|---|---|
| Intervalli o confronti (`vita < 30`, `eta >= 18`) | `if` |
| Condizioni composte (`&&`, `\|\|`) | `if` |
| Una variabile intera/char confrontata con valori precisi (menu, giorni, opzioni) | `switch` |

---

## 🎨 Schema Visivo

### `if / else if / else`

```
                    ┌──────────────────┐
                    │  INIZIO controllo│
                    └────────┬─────────┘
                             ▼
                   ◇ vita <= 0 ? ◇
                   │              │
                  SÌ             NO
                   │              ▼
                   │        ◇ vita < 30 ? ◇
                   │        │              │
                   │       SÌ             NO
                   ▼        ▼              ▼
              "Game Over" "Vita bassa!" "Stai benone"
                   │        │              │
                   └────────┴──────┬───────┘
                                   ▼
                       ┌────────────────────┐
                       │ il programma       │
                       │ continua da qui    │
                       └────────────────────┘

  ➜ Si esegue UN SOLO ramo: il primo la cui condizione è vera.
```

### `switch`

```
                  switch (scelta)
                        │
        ┌───────────┬───┴───────┬─────────────┐
        ▼           ▼           ▼             ▼
    case 'G':   case 'M':   case 'L':     default:
    Guerriero     Mago        Ladro      "non valida"
        │           │           │             │
      break       break       break           │
        │           │           │             │
        └───────────┴─────┬─────┴─────────────┘
                          ▼
                 il programma continua

   ⚠️ Senza break il flusso "scivola" nel case successivo!
```

---

## 🎯 Esercizio di Spiegazione Guidata: "Il Buttafuori del Dungeon"

**Consegna:** per entrare nel Dungeon dell'Oscurità servono:
- **livello almeno 10**, oppure
- la **password segreta** ("alohomora"), a prescindere dal livello.

Con livello tra 5 e 9 (e senza password) il troll dà un consiglio gentile; sotto il 5 ti caccia. Se entri, scegli la tua classe con uno `switch`.

### Passo 1 — Traduciamo le regole in condizioni

| Regola in italiano | Condizione C++ |
|---|---|
| "livello almeno 10" | `livello >= 10` |
| "oppure password giusta" | `\|\| password == "alohomora"` |
| "livello tra 5 e 9" | `livello >= 5` (ci arriviamo solo se la prima è falsa!) |
| "altrimenti" | `else` |
| "scelta della classe" | `switch` su un `char` (valori precisi!) |

💡 **Trucco da programmatore:** l'ordine conta! Siccome `else if` si valuta solo se il precedente è `false`, non serve scrivere `livello >= 5 && livello <= 9`: se siamo arrivati lì, è già sotto il 10.

### Passo 2 — Il codice

```cpp
#include <iostream>
#include <string>
using namespace std;

int main() {
    int livello;
    string password;
    char classe;

    cout << "Livello del tuo eroe: ";
    cin >> livello;
    cin.ignore();                                   // pulizia del buffer (ricordi il Cap. 1?)

    cout << "Password segreta (INVIO se non la sai): ";
    getline(cin, password);

    // 1) Il caso "vincente" va per primo
    if (livello >= 10 || password == "alohomora") {
        cout << "Il troll si scansa: «Prego, entra, grande eroe!»" << endl;

        cout << "Scegli la classe (G = Guerriero, M = Mago, L = Ladro): ";
        cin >> classe;

        switch (classe) {
            case 'G':
                cout << "Impugni la spada: +10 forza ⚔️" << endl;
                break;
            case 'M':
                cout << "Apri il grimorio: +10 magia 🧙" << endl;
                break;
            case 'L':
                cout << "Ti nascondi nell'ombra: +10 agilità 🗡️" << endl;
                break;
            default:
                cout << "Classe sconosciuta: entri a mani nude 🤷" << endl;
        }
    }
    // 2) Se siamo qui, livello < 10 E password sbagliata
    else if (livello >= 5) {
        cout << "Il troll sospira: «Torna al livello 10, ragazzo.»" << endl;
    }
    // 3) Tutti gli altri
    else {
        cout << "«TU?! Fuori di qui!» *vieni lanciato a 300 metri*" << endl;
    }

    return 0;
}
```

### Passo 3 — Il ragionamento dietro ogni scelta

1. **Perché `||` nel primo `if`?** Le regole dicono "livello OPPURE password": basta che *una* delle due sia vera.
2. **Perché `password == "alohomora"` funziona?** In C++ la classe `string` ridefinisce `==` per confrontare il *contenuto*. (Attenzione: con altri linguaggi non è così!)
3. **Perché il caso "entra" va per primo?** Mettere per prima la condizione più specifica evita controlli inutili e rende il codice leggibile come una storia.
4. **Perché nell'`else if` basta `livello >= 5`?** Ci arriviamo solo se `livello >= 10` era falso: la condizione "< 10" è già garantita.
5. **Perché `switch` per la classe e non tre `if`?** Stiamo confrontando **un solo `char`** con valori precisi: lo `switch` è più ordinato e leggibile.
6. **Perché ogni `case` ha il `break`?** Senza, dopo "Guerriero" l'eroe riceverebbe anche i bonus del Mago e del Ladro!

> 🧠 **Sfida bonus:** rendi la password insensibile alle maiuscole (suggerimento: `tolower` da `<cctype>` e un ciclo, che vedremo nel prossimo capitolo!). Accetta anche la classe in minuscolo con `case 'G': case 'g':` impilati.

---

## ✅ Ricorda

- `if` decide, `else if` offre alternative, `else` raccoglie tutto il resto.
- `=` assegna, `==` confronta.
- `&&` = e, `||` = o, `!` = non.
- `switch` è perfetto per confrontare una variabile (int/char) con valori precisi: **non dimenticare i `break`!**
- L'**ordine** delle condizioni è parte della logica.
