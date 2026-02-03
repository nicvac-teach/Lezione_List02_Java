# Costruiamo una Lista Concatenata in Java

Costruiamo passo dopo passo una struttura dati dinamica: una lista concatenata generica. Dietro a questa implementazione si nascondono i meccanismi fondamentali della programmazione a oggetti e della gestione della memoria: riferimenti, generics, scansione iterativa. Risolvi ogni esercizio autonomamente prima di guardare la soluzione!

---

## Blocco 0 — La classe Nodo

### Obiettivo

Creare la classe `Nodo<T>` che rappresenta un singolo elemento della lista.

### Ingredienti

| Elemento | Descrizione |
|----------|-------------|
| `class Nodo<T>` | Classe generica: `T` è il tipo del dato che conterrà |
| `T dato` | Attributo che memorizza il valore |
| `Nodo<T> next` | Riferimento al nodo successivo (o `null` se è l'ultimo) |
| Costruttore | Inizializza il dato e imposta `next` a `null` |

### Come combinarli

1. Dichiara la classe con il parametro generico `<T>`
2. Dichiara l'attributo `dato` di tipo `T`
3. Dichiara l'attributo `next` di tipo `Nodo<T>`
4. Nel costruttore, ricevi il dato come parametro e assegnalo all'attributo. Imposta `next` a `null`

### Esercizio

Crea la classe `Nodo<T>` nel file `Nodo.java`.

<details>
<summary>Soluzione</summary>

```java
public class Nodo<T> {
    T dato;
    Nodo<T> next;

    public Nodo(T dato) {
        this.dato = dato;
        this.next = null;
    }
}
```

</details>

---

## Blocco 1 — La classe Lista (struttura base)

### Obiettivo

Creare la classe `Lista<T>` con l'attributo `head` che punta al primo nodo.

### Ingredienti

| Elemento | Descrizione |
|----------|-------------|
| `class Lista<T>` | Classe generica che gestisce la lista |
| `Nodo<T> head` | Riferimento al primo nodo (o `null` se la lista è vuota) |
| Costruttore | Inizializza `head` a `null` (lista vuota) |

### Come combinarli

1. Dichiara la classe con il parametro generico `<T>`
2. Dichiara l'attributo privato `head` di tipo `Nodo<T>`
3. Nel costruttore, imposta `head` a `null`

### Esercizio

Crea la classe `Lista<T>` nel file `Lista.java` con la struttura base.

<details>
<summary>Soluzione</summary>

```java
public class Lista<T> {
    private Nodo<T> head;

    public Lista() {
        this.head = null;
    }
}
```

</details>

---

## Blocco 2 — isEmpty()

### Obiettivo

Verificare se la lista è vuota.

### Ingredienti

| Elemento | Descrizione |
|----------|-------------|
| `boolean isEmpty()` | Restituisce `true` se la lista non contiene nodi |
| `head == null` | Condizione che indica lista vuota |

### Come funziona

```
Lista vuota:
  HEAD
    │
    ▼
  NULL         →  isEmpty() restituisce true

Lista non vuota:
  HEAD
    │
    ▼
┌───────┬───────┐
│   A   │ NULL  │  →  isEmpty() restituisce false
└───────┴───────┘
```

### Come combinarli

1. Controlla se `head` è `null`
2. Se sì, restituisci `true`
3. Altrimenti restituisci `false`

**Costo**: O(1)

### Esercizio

Implementa il metodo `isEmpty()` nella classe `Lista`.

<details>
<summary>Soluzione</summary>

```java
public boolean isEmpty() {
    return head == null;
}
```

</details>

---

## Blocco 3 — aggiungiInTesta()

### Obiettivo

Inserire un nuovo nodo all'inizio della lista.

### Ingredienti

| Elemento | Descrizione |
|----------|-------------|
| `void aggiungiInTesta(T dato)` | Inserisce un nuovo nodo che diventa la nuova testa |
| `new Nodo<>(dato)` | Crea un nuovo nodo con il dato passato |
| `nuovoNodo.next = head` | Collega il nuovo nodo al vecchio primo elemento |
| `head = nuovoNodo` | Aggiorna HEAD per puntare al nuovo nodo |

### Come funziona

Prima:
```
  HEAD
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   B   │ NULL  │
└───────┴───────┘     └───────┴───────┘
```

1. Creo un nuovo nodo con il dato
2. Il NEXT del nuovo nodo punta al vecchio HEAD
3. HEAD punta al nuovo nodo

Dopo (inserisco X):
```
  HEAD
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐     ┌───────┬───────┐
│   X   │ NEXT  │────▶│   A   │ NEXT  │────▶│   B   │ NULL  │
└───────┴───────┘     └───────┴───────┘     └───────┴───────┘
```

### Come combinarli

1. Crea un nuovo nodo passando il dato al costruttore
2. Imposta il `next` del nuovo nodo uguale a `head` (così punta al vecchio primo)
3. Aggiorna `head` per puntare al nuovo nodo

**Costo**: O(1) — non dipende dalla lunghezza della lista.

### Esercizio

Implementa il metodo `aggiungiInTesta(T dato)` nella classe `Lista`.

<details>
<summary>Soluzione</summary>

```java
public void aggiungiInTesta(T dato) {
    Nodo<T> nuovoNodo = new Nodo<>(dato);
    nuovoNodo.next = head;
    head = nuovoNodo;
}
```

</details>

---

## Blocco 4 — aggiungiInCoda()

### Obiettivo

Inserire un nuovo nodo alla fine della lista.

### Ingredienti

| Elemento | Descrizione |
|----------|-------------|
| `void aggiungiInCoda(T dato)` | Inserisce un nuovo nodo alla fine |
| `new Nodo<>(dato)` | Crea un nuovo nodo |
| Scansione con `while` | Per trovare l'ultimo nodo |
| `corrente.next == null` | Condizione che identifica l'ultimo nodo |

### Come funziona

Prima:
```
  HEAD
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   B   │ NULL  │
└───────┴───────┘     └───────┴───────┘
```

1. Scorro la lista fino all'ultimo nodo (quello con NEXT = NULL)
2. Creo un nuovo nodo con il dato e NEXT = NULL
3. Il NEXT dell'ultimo nodo punta al nuovo nodo

Dopo (inserisco X):
```
  HEAD
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   B   │ NEXT  │────▶│   X   │ NULL  │
└───────┴───────┘     └───────┴───────┘     └───────┴───────┘
```

### Come combinarli

1. Crea un nuovo nodo con il dato
2. **Caso speciale**: se la lista è vuota (`head == null`), il nuovo nodo diventa la testa
3. Altrimenti, parti da `head` e scorri con un ciclo `while` finché `corrente.next != null`
4. Quando trovi l'ultimo nodo, imposta il suo `next` al nuovo nodo

**Costo**: O(n) — devi scorrere tutta la lista.

### Esercizio

Implementa il metodo `aggiungiInCoda(T dato)` nella classe `Lista`.

<details>
<summary>Soluzione</summary>

```java
public void aggiungiInCoda(T dato) {
    Nodo<T> nuovoNodo = new Nodo<>(dato);
    
    if (head == null) {
        head = nuovoNodo;
        return;
    }
    
    Nodo<T> corrente = head;
    while (corrente.next != null) {
        corrente = corrente.next;
    }
    corrente.next = nuovoNodo;
}
```

</details>

---

## Blocco 5 — aggiungiInPosizione()

### Obiettivo

Inserire un nuovo nodo in una posizione specifica della lista.

### Ingredienti

| Elemento | Descrizione |
|----------|-------------|
| `void aggiungiInPosizione(T dato, int posizione)` | Inserisce in posizione specifica |
| Contatore di posizione | Per sapere quando fermarsi |
| Riferimento al nodo precedente | Per poter agganciare il nuovo nodo |

### Come funziona

Prima (voglio inserire X in posizione 1, cioè tra A e B):
```
  HEAD
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   B   │ NULL  │
└───────┴───────┘     └───────┴───────┘
  pos=0                 pos=1
```

1. Scorro la lista fino al nodo in posizione (posizione - 1)
2. Creo un nuovo nodo con il dato
3. Il NEXT del nuovo nodo punta al nodo successivo
4. Il NEXT del nodo precedente punta al nuovo nodo

Dopo:
```
  HEAD
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   X   │ NEXT  │────▶│   B   │ NULL  │
└───────┴───────┘     └───────┴───────┘     └───────┴───────┘
  pos=0                 pos=1                 pos=2
```

### Come combinarli

1. **Caso speciale**: se `posizione == 0`, usa `aggiungiInTesta`
2. Altrimenti, scorri la lista con un contatore fino a raggiungere la posizione `posizione - 1`
3. Crea il nuovo nodo
4. Imposta `nuovoNodo.next = precedente.next`
5. Imposta `precedente.next = nuovoNodo`
6. Gestisci il caso in cui la posizione sia oltre la fine della lista

**Costo**: O(n) — nel caso peggiore devi scorrere tutta la lista.

### Esercizio

Implementa il metodo `aggiungiInPosizione(T dato, int posizione)` nella classe `Lista`.

<details>
<summary>Soluzione</summary>

```java
public void aggiungiInPosizione(T dato, int posizione) {
    if (posizione < 0) {
        throw new IndexOutOfBoundsException("Posizione negativa");
    }
    
    if (posizione == 0) {
        aggiungiInTesta(dato);
        return;
    }
    
    Nodo<T> corrente = head;
    int i = 0;
    
    while (corrente != null && i < posizione - 1) {
        corrente = corrente.next;
        i++;
    }
    
    if (corrente == null) {
        throw new IndexOutOfBoundsException("Posizione oltre la fine della lista");
    }
    
    Nodo<T> nuovoNodo = new Nodo<>(dato);
    nuovoNodo.next = corrente.next;
    corrente.next = nuovoNodo;
}
```

</details>

---

## Blocco 6 — leggiTesta()

### Obiettivo

Restituire il dato del primo nodo senza rimuoverlo.

### Ingredienti

| Elemento | Descrizione |
|----------|-------------|
| `T leggiTesta()` | Restituisce il dato in testa |
| `head.dato` | Accesso al dato del primo nodo |

### Come funziona

```
  HEAD
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   B   │ NULL  │
└───────┴───────┘     └───────┴───────┘
    │
    └──▶ Restituisco "A"
```

### Come combinarli

1. Verifica che la lista non sia vuota
2. Restituisci `head.dato`

**Costo**: O(1) — accesso diretto tramite HEAD.

### Esercizio

Implementa il metodo `leggiTesta()` nella classe `Lista`.

<details>
<summary>Soluzione</summary>

```java
public T leggiTesta() {
    if (head == null) {
        throw new NoSuchElementException("Lista vuota");
    }
    return head.dato;
}
```

</details>

---

## Blocco 7 — leggiCoda()

### Obiettivo

Restituire il dato dell'ultimo nodo senza rimuoverlo.

### Ingredienti

| Elemento | Descrizione |
|----------|-------------|
| `T leggiCoda()` | Restituisce il dato in coda |
| Scansione con `while` | Per raggiungere l'ultimo nodo |
| `corrente.next == null` | Condizione che identifica l'ultimo nodo |

### Come funziona

```
  HEAD
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   B   │ NULL  │
└───────┴───────┘     └───────┴───────┘
                            │
                            └──▶ Restituisco "B"
```

### Come combinarli

1. Verifica che la lista non sia vuota
2. Scorri fino all'ultimo nodo (quello con `next == null`)
3. Restituisci il suo dato

**Costo**: O(n) — devi scorrere tutta la lista.

### Esercizio

Implementa il metodo `leggiCoda()` nella classe `Lista`.

<details>
<summary>Soluzione</summary>

```java
public T leggiCoda() {
    if (head == null) {
        throw new NoSuchElementException("Lista vuota");
    }
    
    Nodo<T> corrente = head;
    while (corrente.next != null) {
        corrente = corrente.next;
    }
    return corrente.dato;
}
```

</details>

---

## Blocco 8 — leggiInPosizione()

### Obiettivo

Restituire il dato di un nodo in una posizione specifica.

### Ingredienti

| Elemento | Descrizione |
|----------|-------------|
| `T leggiInPosizione(int posizione)` | Restituisce il dato alla posizione indicata |
| Contatore di posizione | Per sapere quando fermarsi |

### Come funziona

```
  HEAD
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   B   │ NEXT  │────▶│   C   │ NULL  │
└───────┴───────┘     └───────┴───────┘     └───────┴───────┘
  pos=0                 pos=1                 pos=2
                          │
                          └──▶ leggiInPosizione(1) restituisce "B"
```

### Come combinarli

1. Verifica che la posizione sia valida (>= 0)
2. Scorri la lista contando i nodi
3. Quando raggiungi la posizione, restituisci il dato
4. Se la posizione è oltre la fine, lancia un'eccezione

**Costo**: O(n) — devi raggiungere la posizione.

### Esercizio

Implementa il metodo `leggiInPosizione(int posizione)` nella classe `Lista`.

<details>
<summary>Soluzione</summary>

```java
public T leggiInPosizione(int posizione) {
    if (posizione < 0) {
        throw new IndexOutOfBoundsException("Posizione negativa");
    }
    
    Nodo<T> corrente = head;
    int i = 0;
    
    while (corrente != null && i < posizione) {
        corrente = corrente.next;
        i++;
    }
    
    if (corrente == null) {
        throw new IndexOutOfBoundsException("Posizione oltre la fine della lista");
    }
    
    return corrente.dato;
}
```

</details>

---

## Blocco 9 — size()

### Obiettivo

Restituire il numero di nodi nella lista.

### Ingredienti

| Elemento | Descrizione |
|----------|-------------|
| `int size()` | Restituisce la dimensione della lista |
| Contatore | Per contare i nodi durante la scansione |

### Come funziona

```
  HEAD
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   B   │ NEXT  │────▶│   C   │ NULL  │
└───────┴───────┘     └───────┴───────┘     └───────┴───────┘
  cnt=1                 cnt=2                 cnt=3

Restituisco 3
```

### Come combinarli

1. Inizializza un contatore a 0
2. Scorri tutta la lista incrementando il contatore ad ogni nodo
3. Restituisci il contatore

**Costo**: O(n) — devi scorrere tutta la lista.

### Esercizio

Implementa il metodo `size()` nella classe `Lista`.

<details>
<summary>Soluzione</summary>

```java
public int size() {
    int count = 0;
    Nodo<T> corrente = head;
    
    while (corrente != null) {
        count++;
        corrente = corrente.next;
    }
    
    return count;
}
```

</details>

---

## Blocco 10 — contiene()

### Obiettivo

Verificare se un dato è presente nella lista.

### Ingredienti

| Elemento | Descrizione |
|----------|-------------|
| `boolean contiene(T dato)` | Restituisce `true` se il dato è presente |
| `equals()` | Per confrontare i dati |
| Scansione con `while` | Per esaminare ogni nodo |

### Come funziona

```
  HEAD
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   B   │ NEXT  │────▶│   C   │ NULL  │
└───────┴───────┘     └───────┴───────┘     └───────┴───────┘
                            ▲
                            │
                      Cerco "B"?
                       Trovato!
```

### Come combinarli

1. Scorri la lista nodo per nodo
2. Per ogni nodo, confronta il dato con quello cercato usando `equals()`
3. Se trovi una corrispondenza, restituisci `true`
4. Se arrivi alla fine senza trovare, restituisci `false`

**Costo**: O(n) — nel caso peggiore devi scorrere tutta la lista.

### Esercizio

Implementa il metodo `contiene(T dato)` nella classe `Lista`.

<details>
<summary>Soluzione</summary>

```java
public boolean contiene(T dato) {
    Nodo<T> corrente = head;
    
    while (corrente != null) {
        if (corrente.dato.equals(dato)) {
            return true;
        }
        corrente = corrente.next;
    }
    
    return false;
}
```

</details>

---

## Blocco 11 — indiceDi()

### Obiettivo

Restituire la posizione di un dato nella lista.

### Ingredienti

| Elemento | Descrizione |
|----------|-------------|
| `int indiceDi(T dato)` | Restituisce l'indice del dato, o -1 se non trovato |
| Contatore di posizione | Per tenere traccia dell'indice corrente |
| `equals()` | Per confrontare i dati |

### Come funziona

```
  HEAD
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   B   │ NEXT  │────▶│   C   │ NULL  │
└───────┴───────┘     └───────┴───────┘     └───────┴───────┘
  pos=0                 pos=1                 pos=2

indiceDi("B") → restituisce 1
indiceDi("Z") → restituisce -1
```

### Come combinarli

1. Inizializza un contatore a 0
2. Scorri la lista confrontando ogni dato con quello cercato
3. Se trovi una corrispondenza, restituisci il contatore
4. Se arrivi alla fine, restituisci -1

**Costo**: O(n) — nel caso peggiore devi scorrere tutta la lista.

### Esercizio

Implementa il metodo `indiceDi(T dato)` nella classe `Lista`.

<details>
<summary>Soluzione</summary>

```java
public int indiceDi(T dato) {
    Nodo<T> corrente = head;
    int indice = 0;
    
    while (corrente != null) {
        if (corrente.dato.equals(dato)) {
            return indice;
        }
        corrente = corrente.next;
        indice++;
    }
    
    return -1;
}
```

</details>

---

## Blocco 12 — cancella()

### Obiettivo

Rimuovere il primo nodo che contiene un dato specifico.

### Ingredienti

| Elemento | Descrizione |
|----------|-------------|
| `boolean cancella(T dato)` | Rimuove il nodo e restituisce `true` se trovato |
| Riferimento al nodo precedente | Per poter "scavalcare" il nodo da eliminare |
| `equals()` | Per trovare il nodo giusto |

### Come funziona

Prima (voglio cancellare B):
```
  HEAD
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   B   │ NEXT  │────▶│   C   │ NULL  │
└───────┴───────┘     └───────┴───────┘     └───────┴───────┘
```

1. Scorro la lista cercando il nodo con il dato, tenendo traccia del nodo precedente
2. Il NEXT del nodo precedente punta al nodo successivo a quello da cancellare

Dopo:
```
  HEAD
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   C   │ NULL  │
└───────┴───────┘     └───────┴───────┘
```

### Come combinarli

1. **Caso speciale**: se il dato è in testa, aggiorna `head = head.next`
2. Altrimenti, scorri tenendo traccia del nodo precedente
3. Quando trovi il dato, imposta `precedente.next = corrente.next`
4. Restituisci `true` se trovato, `false` altrimenti

**Costo**: O(n) — devi cercare il nodo.

### Esercizio

Implementa il metodo `cancella(T dato)` nella classe `Lista`.

<details>
<summary>Soluzione</summary>

```java
public boolean cancella(T dato) {
    if (head == null) {
        return false;
    }
    
    // Caso speciale: il dato è in testa
    if (head.dato.equals(dato)) {
        head = head.next;
        return true;
    }
    
    Nodo<T> precedente = head;
    Nodo<T> corrente = head.next;
    
    while (corrente != null) {
        if (corrente.dato.equals(dato)) {
            precedente.next = corrente.next;
            return true;
        }
        precedente = corrente;
        corrente = corrente.next;
    }
    
    return false;
}
```

</details>

---

## Blocco 13 — cancellaInPosizione()

### Obiettivo

Rimuovere il nodo in una posizione specifica e restituire il suo dato.

### Ingredienti

| Elemento | Descrizione |
|----------|-------------|
| `T cancellaInPosizione(int posizione)` | Rimuove e restituisce il dato |
| Riferimento al nodo precedente | Per poter "scavalcare" il nodo da eliminare |
| Contatore di posizione | Per raggiungere la posizione giusta |

### Come funziona

Prima (cancello posizione 1):
```
  HEAD
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   B   │ NEXT  │────▶│   C   │ NULL  │
└───────┴───────┘     └───────┴───────┘     └───────┴───────┘
  pos=0                 pos=1                 pos=2
```

Dopo:
```
  HEAD
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   C   │ NULL  │
└───────┴───────┘     └───────┴───────┘

Restituisco "B"
```

### Come combinarli

1. Verifica che la posizione sia valida
2. **Caso speciale**: se `posizione == 0`, salva il dato, aggiorna `head = head.next`, restituisci il dato
3. Altrimenti, scorri fino alla posizione `posizione - 1`
4. Salva il dato del nodo da eliminare
5. Imposta `precedente.next = corrente.next`
6. Restituisci il dato salvato

**Costo**: O(n) — devi raggiungere la posizione.

### Esercizio

Implementa il metodo `cancellaInPosizione(int posizione)` nella classe `Lista`.

<details>
<summary>Soluzione</summary>

```java
public T cancellaInPosizione(int posizione) {
    if (posizione < 0 || head == null) {
        throw new IndexOutOfBoundsException("Posizione non valida");
    }
    
    // Caso speciale: cancellazione in testa
    if (posizione == 0) {
        T dato = head.dato;
        head = head.next;
        return dato;
    }
    
    Nodo<T> precedente = head;
    int i = 0;
    
    while (precedente.next != null && i < posizione - 1) {
        precedente = precedente.next;
        i++;
    }
    
    if (precedente.next == null) {
        throw new IndexOutOfBoundsException("Posizione oltre la fine della lista");
    }
    
    T dato = precedente.next.dato;
    precedente.next = precedente.next.next;
    return dato;
}
```

</details>

---

## Blocco 14 — concatena()

### Obiettivo

Aggiungere tutti i nodi di un'altra lista alla fine di questa lista.

### Ingredienti

| Elemento | Descrizione |
|----------|-------------|
| `void concatena(Lista<T> altraLista)` | Concatena l'altra lista a questa |
| Scansione per trovare l'ultimo nodo | Per agganciare la seconda lista |
| `altraLista.head` | Punto di inizio della lista da concatenare |

### Come funziona

Prima:
```
Lista 1:
  HEAD1
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   B   │ NULL  │
└───────┴───────┘     └───────┴───────┘

Lista 2:
  HEAD2
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐
│   X   │ NEXT  │────▶│   Y   │ NULL  │
└───────┴───────┘     └───────┴───────┘
```

Dopo:
```
  HEAD1
    │
    ▼
┌───────┬───────┐     ┌───────┬───────┐     ┌───────┬───────┐     ┌───────┬───────┐
│   A   │ NEXT  │────▶│   B   │ NEXT  │────▶│   X   │ NEXT  │────▶│   Y   │ NULL  │
└───────┴───────┘     └───────┴───────┘     └───────┴───────┘     └───────┴───────┘
```

### Come combinarli

1. Se l'altra lista è vuota, non fare nulla
2. Se questa lista è vuota, copia il riferimento `head` dell'altra lista
3. Altrimenti, scorri questa lista fino all'ultimo nodo
4. Imposta `ultimoNodo.next = altraLista.head`

**Nota**: questa implementazione "ruba" i nodi dell'altra lista. L'altra lista rimane collegata agli stessi nodi.

**Costo**: O(n) — devi raggiungere l'ultimo nodo della prima lista.

### Esercizio

Implementa il metodo `concatena(Lista<T> altraLista)` nella classe `Lista`.

<details>
<summary>Soluzione</summary>

```java
public void concatena(Lista<T> altraLista) {
    if (altraLista == null || altraLista.head == null) {
        return;
    }
    
    if (head == null) {
        head = altraLista.head;
        return;
    }
    
    Nodo<T> corrente = head;
    while (corrente.next != null) {
        corrente = corrente.next;
    }
    corrente.next = altraLista.head;
}
```

</details>

---

## Blocco 15 — toString()

### Obiettivo

Restituire una rappresentazione testuale della lista (utile per debug e stampa).

### Ingredienti

| Elemento | Descrizione |
|----------|-------------|
| `String toString()` | Override del metodo di Object |
| `StringBuilder` | Per costruire la stringa in modo efficiente |

### Come combinarli

1. Crea un `StringBuilder`
2. Scorri la lista aggiungendo ogni elemento
3. Separa gli elementi con " -> "
4. Aggiungi "NULL" alla fine
5. Restituisci la stringa

### Esercizio

Implementa il metodo `toString()` nella classe `Lista`.

Esempio di output: `A -> B -> C -> NULL`

<details>
<summary>Soluzione</summary>

```java
@Override
public String toString() {
    StringBuilder sb = new StringBuilder();
    Nodo<T> corrente = head;
    
    while (corrente != null) {
        sb.append(corrente.dato);
        sb.append(" -> ");
        corrente = corrente.next;
    }
    sb.append("NULL");
    
    return sb.toString();
}
```

</details>

---

## Codice Completo

Prima di passare all'esercizio finale, ecco il codice completo delle due classi.

### Nodo.java

```java
public class Nodo<T> {
    T dato;
    Nodo<T> next;

    public Nodo(T dato) {
        this.dato = dato;
        this.next = null;
    }
}
```

### Lista.java

```java
import java.util.NoSuchElementException;

public class Lista<T> {
    private Nodo<T> head;

    public Lista() {
        this.head = null;
    }

    public boolean isEmpty() {
        return head == null;
    }

    public void aggiungiInTesta(T dato) {
        Nodo<T> nuovoNodo = new Nodo<>(dato);
        nuovoNodo.next = head;
        head = nuovoNodo;
    }

    public void aggiungiInCoda(T dato) {
        Nodo<T> nuovoNodo = new Nodo<>(dato);
        
        if (head == null) {
            head = nuovoNodo;
            return;
        }
        
        Nodo<T> corrente = head;
        while (corrente.next != null) {
            corrente = corrente.next;
        }
        corrente.next = nuovoNodo;
    }

    public void aggiungiInPosizione(T dato, int posizione) {
        if (posizione < 0) {
            throw new IndexOutOfBoundsException("Posizione negativa");
        }
        
        if (posizione == 0) {
            aggiungiInTesta(dato);
            return;
        }
        
        Nodo<T> corrente = head;
        int i = 0;
        
        while (corrente != null && i < posizione - 1) {
            corrente = corrente.next;
            i++;
        }
        
        if (corrente == null) {
            throw new IndexOutOfBoundsException("Posizione oltre la fine della lista");
        }
        
        Nodo<T> nuovoNodo = new Nodo<>(dato);
        nuovoNodo.next = corrente.next;
        corrente.next = nuovoNodo;
    }

    public T leggiTesta() {
        if (head == null) {
            throw new NoSuchElementException("Lista vuota");
        }
        return head.dato;
    }

    public T leggiCoda() {
        if (head == null) {
            throw new NoSuchElementException("Lista vuota");
        }
        
        Nodo<T> corrente = head;
        while (corrente.next != null) {
            corrente = corrente.next;
        }
        return corrente.dato;
    }

    public T leggiInPosizione(int posizione) {
        if (posizione < 0) {
            throw new IndexOutOfBoundsException("Posizione negativa");
        }
        
        Nodo<T> corrente = head;
        int i = 0;
        
        while (corrente != null && i < posizione) {
            corrente = corrente.next;
            i++;
        }
        
        if (corrente == null) {
            throw new IndexOutOfBoundsException("Posizione oltre la fine della lista");
        }
        
        return corrente.dato;
    }

    public int size() {
        int count = 0;
        Nodo<T> corrente = head;
        
        while (corrente != null) {
            count++;
            corrente = corrente.next;
        }
        
        return count;
    }

    public boolean contiene(T dato) {
        Nodo<T> corrente = head;
        
        while (corrente != null) {
            if (corrente.dato.equals(dato)) {
                return true;
            }
            corrente = corrente.next;
        }
        
        return false;
    }

    public int indiceDi(T dato) {
        Nodo<T> corrente = head;
        int indice = 0;
        
        while (corrente != null) {
            if (corrente.dato.equals(dato)) {
                return indice;
            }
            corrente = corrente.next;
            indice++;
        }
        
        return -1;
    }

    public boolean cancella(T dato) {
        if (head == null) {
            return false;
        }
        
        if (head.dato.equals(dato)) {
            head = head.next;
            return true;
        }
        
        Nodo<T> precedente = head;
        Nodo<T> corrente = head.next;
        
        while (corrente != null) {
            if (corrente.dato.equals(dato)) {
                precedente.next = corrente.next;
                return true;
            }
            precedente = corrente;
            corrente = corrente.next;
        }
        
        return false;
    }

    public T cancellaInPosizione(int posizione) {
        if (posizione < 0 || head == null) {
            throw new IndexOutOfBoundsException("Posizione non valida");
        }
        
        if (posizione == 0) {
            T dato = head.dato;
            head = head.next;
            return dato;
        }
        
        Nodo<T> precedente = head;
        int i = 0;
        
        while (precedente.next != null && i < posizione - 1) {
            precedente = precedente.next;
            i++;
        }
        
        if (precedente.next == null) {
            throw new IndexOutOfBoundsException("Posizione oltre la fine della lista");
        }
        
        T dato = precedente.next.dato;
        precedente.next = precedente.next.next;
        return dato;
    }

    public void concatena(Lista<T> altraLista) {
        if (altraLista == null || altraLista.head == null) {
            return;
        }
        
        if (head == null) {
            head = altraLista.head;
            return;
        }
        
        Nodo<T> corrente = head;
        while (corrente.next != null) {
            corrente = corrente.next;
        }
        corrente.next = altraLista.head;
    }

    @Override
    public String toString() {
        StringBuilder sb = new StringBuilder();
        Nodo<T> corrente = head;
        
        while (corrente != null) {
            sb.append(corrente.dato);
            sb.append(" -> ");
            corrente = corrente.next;
        }
        sb.append("NULL");
        
        return sb.toString();
    }
}
```

---

## Esercizio Finale — Usa la tua Lista!

### Obiettivo

Creare una classe `Main` che utilizza la lista appena implementata per gestire una rubrica di contatti.

### Requisiti

1. Crea una `Lista<String>` per memorizzare nomi di contatti
2. Aggiungi almeno 5 contatti usando i vari metodi di inserimento
3. Stampa la lista
4. Cerca un contatto e stampa la sua posizione
5. Rimuovi un contatto per nome
6. Rimuovi un contatto per posizione
7. Stampa la dimensione finale
8. Verifica se un contatto è presente

### Esercizio

Scrivi la classe `Main.java` che esegue tutte le operazioni richieste.

<details>
<summary>Soluzione</summary>

```java
public class Main {
    public static void main(String[] args) {
        Lista<String> rubrica = new Lista<>();
        
        System.out.println("=== RUBRICA CONTATTI ===\n");
        
        // Aggiungo contatti con vari metodi
        rubrica.aggiungiInTesta("Alice");
        rubrica.aggiungiInCoda("Bob");
        rubrica.aggiungiInCoda("Charlie");
        rubrica.aggiungiInTesta("Zoe");
        rubrica.aggiungiInPosizione("Marco", 2);
        
        System.out.println("Lista iniziale:");
        System.out.println(rubrica);
        System.out.println("Dimensione: " + rubrica.size());
        
        // Cerco un contatto
        String cercato = "Marco";
        int posizione = rubrica.indiceDi(cercato);
        System.out.println("\nPosizione di " + cercato + ": " + posizione);
        
        // Leggo in varie posizioni
        System.out.println("Primo contatto: " + rubrica.leggiTesta());
        System.out.println("Ultimo contatto: " + rubrica.leggiCoda());
        System.out.println("Contatto in posizione 2: " + rubrica.leggiInPosizione(2));
        
        // Rimuovo per nome
        System.out.println("\nRimuovo 'Bob'...");
        rubrica.cancella("Bob");
        System.out.println(rubrica);
        
        // Rimuovo per posizione
        System.out.println("\nRimuovo posizione 1...");
        String rimosso = rubrica.cancellaInPosizione(1);
        System.out.println("Rimosso: " + rimosso);
        System.out.println(rubrica);
        
        // Verifico presenza
        System.out.println("\nContiene 'Alice'? " + rubrica.contiene("Alice"));
        System.out.println("Contiene 'Bob'? " + rubrica.contiene("Bob"));
        
        // Dimensione finale
        System.out.println("\nDimensione finale: " + rubrica.size());
        System.out.println("Lista vuota? " + rubrica.isEmpty());
        
        // Concateno un'altra lista
        Lista<String> altriContatti = new Lista<>();
        altriContatti.aggiungiInCoda("Diana");
        altriContatti.aggiungiInCoda("Eva");
        
        System.out.println("\nConcateno altri contatti...");
        rubrica.concatena(altriContatti);
        System.out.println(rubrica);
        System.out.println("Dimensione finale: " + rubrica.size());
    }
}
```

</details>

---

## Esercizi Extra

Se hai finito e vuoi metterti alla prova:

1. **Inverti la lista**: implementa un metodo `void inverti()` che inverte l'ordine dei nodi
2. **Rimuovi duplicati**: implementa un metodo `void rimuoviDuplicati()` che elimina i nodi con dati ripetuti
3. **Ottimizza con tail**: aggiungi un attributo `tail` che punta sempre all'ultimo nodo, e modifica i metodi per mantenerlo aggiornato. Quanto costa ora `aggiungiInCoda()`?
4. **Lista doppiamente concatenata**: modifica `Nodo` per avere anche un riferimento `prev` al nodo precedente

---

## Riepilogo Costi Computazionali

| Operazione | Costo |
|------------|-------|
| `isEmpty()` | O(1) |
| `aggiungiInTesta()` | O(1) |
| `aggiungiInCoda()` | O(n) |
| `aggiungiInPosizione()` | O(n) |
| `leggiTesta()` | O(1) |
| `leggiCoda()` | O(n) |
| `leggiInPosizione()` | O(n) |
| `size()` | O(n) |
| `contiene()` | O(n) |
| `indiceDi()` | O(n) |
| `cancella()` | O(n) |
| `cancellaInPosizione()` | O(n) |
| `concatena()` | O(n) |