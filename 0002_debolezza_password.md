---
author: "#!/interrupt"
title: I pericoli delle password semplici
sub_title: Analisi pratica della sicurezza
---
<!-- 
speaker_note: |
  Probabilmente tutti sappiamo che usare password semplici è pericoloso, ma forse nessuno ci ha mai fatto vedere quanto è effettivamente facile craccarle. Oggi vi mostrerò esattamente quanto tempo ci vuole per violare le password più comuni usando solo un normale computer portatile - non un supercomputer.
  
  Il problema principale non è solo che qualcuno possa indovinare la vostra password.
-->
<!-- end_slide -->

<!-- jump_to_middle -->
<!-- alignment: center -->
<!-- font_size: 3-->
Tutto inizia dai data breach
<!-- 
speaker_note: |
   Il vero pericolo arriva quando avviene un data breach - cioè quando dei criminali violano un servizio e accedono al database ottenendo così le password. Se la vostra password è semplice e la riusate su più siti, in pochi secondi gli attaccanti avranno accesso a tutti i vostri account: email, social media, banca, tutto.
-->
<!-- end_slide -->

Data breach famosi
===
<!-- column_layout: [4,1] -->
<!-- column: 0 -->
<!-- alignment: left -->
<!-- font_size: 2 -->
<!-- pause -->
2009 - RockYou, 32 milioni di password in chiaro
<!-- pause -->
2012 - Linkedin, 6.5 milioni di hash facilmente crackabili
<!-- pause -->
2013 - Adobe, 153 milioni di record con email, password e altro
<!-- alignment: left -->
<!-- font_size: 1 -->

---
Riferimenti:
- https://en.wikipedia.org/wiki/RockYou
- https://haveibeenpwned.com/Breach/LinkedIn
- https://haveibeenpwned.com/Breach/Adobe

<!-- 
speaker_note: |
  Per capire quanto sia reale questo problema, guardiamo alcuni data breach storici che hanno scosso il mondo informatico:
  - RockYou, nel 2009: 32 milioni di password rubate e salvate in testo in chiaro - sì, avete sentito bene, senza nessuna protezione. Da questo breach abbiamo imparato che la password più usata era "123456".
  - Nel 2012, a Linkedin sono stati rubati 6,5 milioni di hash di password. L'attacco è rimasto in sordina per diversi anni, però nel 2016 è diventato di pubblico dominio scoprendo che gli hash erano molto deboli e facilmente crackabili.
  - Adobe nel 2013 ha subito la compromissione di 153 milioni di account. Oltre agli hash delle password, sono stati rubati nomi, email e anche domande di sicurezza degli utenti.
  
  Non lasciamoci ingannare dall'età di questi breach - che sono i più famosi - ogni giorno ne avvengono di nuovi.
-->

<!-- end_slide -->

<!-- alignment: center -->
<!-- font_size: 3-->
<!-- new_lines: 2 -->
Hash = Impronta Digitale

`password123`  => ?
<!-- pause -->
<!-- font_size: 1-->

| Algoritmo | Output                                                           |
| --------- | ---------------------------------------------------------------- |
| MD5       | 482c811da5d5b4bc6d497ffa98491e38                                 |
| SHA1      | cbfdac6008f9cab4083784cbd1874f76618d2a97                         |
| SHA256    | ef92b778bafe771e89245b89ecbc08a44a4e166c06659911881f383d4473e94f |

<!-- 
speaker_note: |
  Ora avrò menzionato gli hash delle password almeno un paio di volte, e dato che fra poco ci lavoreremo su, conviene dar loro un'occhiata più da vicino, e capiamo come vengono usati. La procedura di hash è una funzione matematica che trasforma qualsiasi input in un output di lunghezza fissa. È come un'impronta digitale: unica e irreversibile.

  I servizi web non dovrebbero mai salvare la vostra password in chiaro - non come nel caso di RockYou. Invece, calcolano questa sorta di impronta di ciascuna password e salvano quello. Quando facciamo il login, si calcola l'hash di quello che avete digitato e lo confrontano con quello salvato.
  
  Ci sono diversi algoritmi, con differenti pregi e velocità di calcolo, e qui vediamo la stessa password passata attraverso tre diversi algoritmi: MD5, SHA1 e SHA256. Non sono gli unici, però sono un'ottima rappresentazione di quello che era lo stato dell'arte qualche anno fa.
-->

<!-- end_slide -->

<!-- alignment: center -->
<!-- font_size: 3-->
<!-- newlines: 2 -->
Gli hash sono sicuri?
<!-- font_size: 2-->
Se ho un hash come questo

`482c811da5d5b4bc6d497ffa98491e38`

Come posso risalire a `password123`?
<!-- pause -->
Un hash è sicuro finchè è "computationally secure"
<!-- 
speaker_note: |
  Il bello dell'hash è che dall' "impronta" non si può risalire alla password originale... o almeno, così dovrebbe essere - in teoria.
  
  Quando in informatica si dice "non si può", non sempre vuol dire che è impossibile. Di solito bisognerebbe dire "non si può... in tempi sostenibili" oppure "non si può... con le risorse a disposizione", perchè è proprio questo quello che vedremo fra poco.

  Infatti, prendendo in prestito un termine dalla crittografia, possiamo dire che sotto alcune condizioni gli hash sono "computationally secure". Adesso vediamo invece come NON lo sono quando queste condizioni non ci sono.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- font_size: 3-->
Forza Bruta
<!-- font_size: 2-->
<!-- alignment: left -->
<!-- incremental_lists: true -->
* a ❌ , b, ❌ ...
* aa ❌, ab ❌, ...
* aaaaaaa000 ❌, aaaaaaa001 ❌, ...

... molti tentativi dopo ...

* password121 ❌, password122 ❌, password123 ✅

<!-- 
speaker_note: |
  Il primo metodo di attacco è il brute force - letteralmente "forza bruta". L'idea è semplice: provo tutte le combinazioni possibili, partendo da "a", poi "b", poi "c", fino ad arrivare a "aaa", "aab", e così via.

  È come avere una serratura a combinazione e provare 0000, 0001, 0002... prima o poi la apri. Il vantaggio è che è matematicamente garantito che funzioni. Lo svantaggio è che può richiedere molto tempo... o forse no, come vedremo.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- font_size: 3-->
Forza Bruta
<!-- font_size: 1 -->

```bash +exec +line_numbers
rm -f ~/snap/john-the-ripper/current/.john/john.*
echo -n "password" | md5sum | awk '{print $1}' | tee test.txt
john --format=Raw-MD5 --incremental test.txt
```

<!-- 
speaker_note: |
  Ora vediamo in pratica quanto tempo serve per craccare password di diverse lunghezze con un attacco brute force su un normale computer portatile.
  
  Per chi non è avvezzo alla riga di comando vediamo nel dettaglio cosa sto per chiedere di fare al mio computer. Alla prima riga faccio pulizia nella cartella di lavoro di john. John è il programma che farà il grosso del lavoro di cracking, ed è progettato per non sprecare risorse quando ha già individuato una password. Facendo la pulizia, lo facciamo ricominciare da zero ogni volta, così da vedere in modo affidabile quanto impiega.

  La seconda riga scrive sul file chiamato test.txt l'hash nel formato MD5, facendolo anche vedere a schermo.

  L'ultima riga è quella che fa il lavoro sporco: chiediamo a john di "rompere" gli hash trovati nel file chiamato test.txt, e per farlo gli passiamo due opzioni. La prima, dato il nome, può risultare intuitiva: stiamo specificando a john che gli hash che troverà all'interno del file sono stati calcolati con l'agoritmo MD5, quindi dovrà usare lo stesso. L'altra opzione è quella che attiva la modalità "forza bruta": incremental dirà a john di provare tutte le combinazioni di caratteri, partendo da quelli alfabetici, numerici e caratteri speciali.

  Ora, ammettiamo che dei criminali riescano a individuare degli hash in MD5 su un server, e abbiano fra le mani anche l'hash della nostra robustissima password. Vediamo quanto ci mette.

  In circa 0 secondi, il mio computer ha individuato una password di 8 caratteri alfabetici provando tutte le combinazioni possibili di caratteri fino a quella individuata. E in questo caso ha usato solo un core del processore, nemmeno tutti.
-->
<!-- end_slide -->

<!-- alignment: center -->
<!-- font_size: 3-->
Forza Bruta
<!-- font_size: 2 -->
Aggiungiamo qualche carattere
<!-- font_size: 1 -->

```bash +exec +line_numbers
rm -f ~/snap/john-the-ripper/current/.john/john.*
echo -n "password1" | md5sum | awk '{print $1}' | tee test.txt
john --format=Raw-MD5 --incremental test.txt
```

<!-- 
speaker_note: |
  Ora invece vediamo quanto ci mette se aggiungiamo un numero al mix.

  Già si può vedere che ci sta pensando un po' di più... E allungando la password di un solo carattere - che sia numerico o un carattere speciale cambia poco - siamo già passati da 0 a circa 12 secondi. Ora potrei aggiungerne un altro e arrivare a 10, ma so già che per MD5 un solo processore del mio portatile non è abbastanza per romperlo senza annoiarvi qui a fissare lo schermo.
-->
<!-- end_slide -->

<!-- alignment: center -->
<!-- font_size: 3-->
Forza Bruta
<!-- font_size: 2 -->
SHA256
<!-- font_size: 1 -->

```bash +exec +line_numbers
rm -f ~/snap/john-the-ripper/current/.john/john.*
echo -n "isola" | sha256sum | awk '{print $1}' | tee test.txt
john --format=Raw-SHA256 --incremental test.txt
```

<!-- 
speaker_note: |
  Giustamente mi si può dire che MD5 non è più usato da nessuno perchè è vecchio, non è sicuro, è troppo veloce da rompere e che ha poco senso farlo vedere. Tuttavia, purtroppo, di fornitori di servizi online che tutt'ora usano MD5, senza nemmeno il salting, per salvare le credenziali degli utenti sono più di quanti si pensi.

  Ad ogni modo, diamo uno sguardo anche al più moderno SHA256. Nel 2025 non è l'ultimo grido, però è già generazioni oltre MD5.

  In questo caso ho dovuto accorciare la password a soli 5 caratteri, altrimenti saremmo stati qua a guardare la vernice che si asciuga per un po'. Tuttavia, già possiamo apprezzare la difficoltà maggiore perchè per indovinare "isola", ci ha messo due secondi.
-->
<!-- end_slide -->

<!-- alignment: center -->
<!-- font_size: 3-->
Forza Bruta
<!-- font_size: 2 -->
SHA256
<!-- font_size: 1 -->

```bash +exec +line_numbers
rm -f ~/snap/john-the-ripper/current/.john/john.*
echo -n "isolet" | sha256sum | awk '{print $1}' | tee test.txt
john --format=Raw-SHA256 --incremental test.txt
```

<!-- 
speaker_note: |
  Proviamo ad aggiungerne uno, e così vediamo che sta macinando un po' più a lungo.

  Adesso siamo a sette caratteri, e già con questo algoritmo il mio povero portatile ci ha messo circa 8 secondi. Ora potremmo andare avanti e misurare tutte le lunghezze e combinazioni di caratteri, ma già aumentando i caratteri a 8, dovrei ricorrere a parallelizzare il calcolo su più processori o schede video per non stare qui a fare la muffa.

  Io non ho queste risorse a disposizione, ma criminali ben determinati, possono investire qualche centinaio di euro in componenti e ridurre quello che un computer normale farebbe in 5 minuti a nemmeno qualche secondo.

  Ovviamente non è finita qui...
-->
<!-- end_slide -->


<!-- alignment: center -->
<!-- font_size: 3-->
Attacco a dizionario
<!-- font_size: 2-->
<!-- alignment: left -->
<!-- incremental_lists: true -->
* 123456 ❌ 
* 343434 ❌
* mmmmmm ❌
* password ❌
* qwert1 ❌

... molti tentativi dopo ...

* password123 ✅

(Queste sono password reali estratte da `rockyou.txt`)

<!-- 
speaker_note: |
  Noi umani siamo abitudinari e abbiamo la tendenza a comportarci in modo simile anche con le password. L'attacco a dizionario è molto più furbo: sfrutta questa debolezza. Invece di provare tutte le combinazioni, si parte dalle password più probabili: "123456", "password", "qwerty", "admin", "welcome"...

  È come se, invece di provare tutte le combinazioni della serratura, provassi prima i codici più ovvi: 0000, 1234, 4321... Molto spesso funziona rapidamente.

  Esistono liste di milioni di password già violate in passato. Gli attaccanti le usano perché sanno che le persone tendono a riutilizzare le stesse password comuni.

  Se le password che sono ora sullo schermo sembrano troppo semplici per essere vere, sono password reali presenti nel data breach di RockYou di cui parlavo prima.
-->
<!-- end_slide -->

<!-- alignment: center -->
<!-- font_size: 3-->
Attacco a Dizionario
<!-- font_size: 1 -->

```bash +exec +line_numbers
rm -f ~/snap/john-the-ripper/current/.john/john.*
echo -n "password123" | sha256sum | awk '{print $1}' | tee test.txt
john --format=Raw-SHA256 --wordlist=/home/nirebu/tools/rockyou.txt test.txt
```

<!-- 
speaker_note: |
  Sul mio computer ho scaricato rockyou.txt, che è la lista di password rubate dall'ominimo servizio nel 2009, che nel frattempo è stato anche arricchito con nuovi breach di password ottenuti in giro per internet fino ad arrivare a oltre 14 milioni. Questo fa di esso un enorme finestra sulle abitudini delle persone su come vengono scelte le password.

  Qui ho dovuto modificare leggermente il comando per eseguire john, in quanto ora devo dirgli "per favore, invece di generare tu le password, guarda questa lista che trovi a questo percorso". Vediamo cosa ci risponde.
  
  In questo modo, un hash codificato con SHA256 di una password di 8 caratteri alfabetici e 3 numerici, che con un metodo a forza bruta ci avrebbe messo un sacco di tempo, è stata individuata istantaneamente.
-->
<!-- end_slide -->

<!-- alignment: center -->
<!-- jump_to_middle -->
<!-- font_size: 4-->
Contromisure

<!-- 
speaker_note: |
  Forza bruta e dizionario sono solo due dei metodi che vengono usati, e sono solo i più semplici. Ce ne sono di più sofisticati, ma per tenere le cose semplici, per oggi vedremo solo questi.

  Vediamo se c'è qualche cntromisura che può essere implementata per rallentare gli attaccanti.
-->

<!-- end_slide -->
<!-- alignment: center -->
<!-- font_size: 3-->
Salting
<!-- font_size: 2-->
password + salt calcolato su dati utente
<!-- pause -->

in MD5

`password123u1` => `6f9a2be7ccc3a6160ce53443ac4fcaf7`

`password123u2` => `a33292398f16d6625b69fb8ca15f5157`

<!-- 
speaker_note: |
  Una delle difese che si possono implementare si chiama "salting". Prima di calcolare l'hash, si aggiunge una stringa generata in base ai dati dell'utente - il "sale" - alla password.

  Questo rende molto più difficile l'attacco perché anche se due utenti hanno la stessa password, avranno hash completamente diversi. Inoltre, gli attaccanti non possono usare tabelle precalcolate di hash comuni, sventando l'attacco "arcobaleno". Inoltre, aumenta anche drasticamente il tempo per la rottura a forza bruta perchè le stringhe sorgenti diventano più lunghe.

  Possiamo notare come, cambiando anche solo un carattere dell'input, i due hash ottenuti sono radicalmente diversi.
-->

<!-- end_slide -->

<!-- font_size: 3-->
Complessità e lunghezza
<!-- font_size: 2-->
Oltre i 10 caratteri

Usiamo sia caratteri minuscoli, maiuscoli, numerici e di punteggiatura

<!-- pause -->

Evitiamo password che assomigliano a `Password8!`, anche queste sono attaccabili.

<!-- 
speaker_note: |
  Lo abbiamo visto benissimo: più aggiungiamo caratteri alla password, più i metodi semplicistici come la forza bruta perdono di efficacia. Attenzione che anche come questi caratteri vengono disposti è importante.

  Da quando siamo stati obbligati ad usare almeno un carattere maiuscolo, un numero e un carattere speciale, quante delle vostre password cominciano con la lettera maiuscola e finiscono col punto esclamativo?
  
  Fatemi sapere nei commenti se siete interessati a vedere come queste debolezze delle nostre abitudini vengono sfruttate per rompere le password velocemente.

  Se avete trovato interessante questo video, lasciate un like e iscrivetevi per non perdervi i prossimi. Ciao!
-->

<!-- end_slide -->
