---
title: Sono un bug vivente
sub_title: Storie di encoding, frustrazione e inclusività
author: Nicolò Rebughini
---
<!--
speaker_note: |
  Oggi parliamo di una questione mi perseguita fin da quando ho cominciato a usare i computer, e gradualmente ho visto sempre diventare più difficoltosa. Mi è capitato di non potermi registrare a servizi critici come PosteID per utilizzare lo SPID. In un vecchio posto di lavoro ho bloccato l'accesso con i badge all'edificio. Con Vinted non potevo depositare i pacchi presso le tabaccherie. E queste sono solo alcune delle cose che mi son successe.

  Sapete qual era sempre il problema? Mi chiamo Nicolò.
-->
<!-- jump_to_middle -->
<!-- alignment: center -->
<!-- font_size: 5-->
Sono un bug vivente
<!-- end_slide -->

<!--
speaker_note: |
  Il colpevole? Questo piccolo simbolo sopra la 'o'. Ma come può un accento creare tutti questi problemi?
-->
<!-- jump_to_middle -->
<!-- alignment: center -->
<!-- font_size: 5-->
Mi chiamo Nicolò
<!-- end_slide -->

<!--
speaker_note: |
  La risposta è semplice: i primi sistemi informatici sono stati progettati da persone che parlavano inglese. Quindi certi problemi - come la gestione degli accenti - non sono nemmeno stati considerati all'inizio. Non era cattiveria, era semplicemente che per loro il problema non esisteva.

  Quando hanno creato i primi standard per rappresentare le lettere nei computer, hanno pensato: 'Abbiamo bisogno delle lettere dalla a alla z, qualche numero, qualche simbolo. Fatto.' E per decenni ha funzionato... se parlavi e scrivevi in inglese.

  Il risultato è che ancora oggi, nel 2025, mi ritrovo a combattere con sistemi che vedono la mia 'ò' come un problema da risolvere. E la soluzione, troppo spesso, è sempre la stessa: adattarsi.
-->
<!-- jump_to_middle -->
<!-- alignment: center -->
<!-- font_size: 5-->
Accents? What are they?
<!-- end_slide -->

<!--
speaker_note: |
  Per capire cosa succede, dobbiamo pensare come funziona un computer. Per noi 'Nicolò' è semplicemente un nome. Per un computer non è altro che una sequenza di numeri:  ogni carattere, che sia minuscolo, maiuscolo o numerico ha un suo codice corrispondente.
-->
<!-- jump_to_middle -->
<!-- alignment: center -->
<!-- font_size: 5-->
Dai caratteri ai numeri
<!-- end_slide -->

<!--
speaker_note: |
  Dato che un esempio vale più di mille parole, ho preparato qualche piccolo programma per capire meglio cosa accade. Vediamo questo esempio scritto nel linguaggio di programmazione Ruby. Cosa fa questo programma? Stiamo prendendo la parola "Nicolò" e per ciascun carattere, utilizzando il metodo `each_char`, chiediamo con `puts` al computer di mostrare a video il carattere stesso e il suo codice ordinale corrispondente, usano il metodo `ord`. Niente di più.
-->
Viaggio nel mondo dei caratteri
===

<!-- alignment: center -->
<!-- font_size: 2-->
```ruby
"Nicolò".each_char do |char|
  puts "#{char} => #{char.ord}"
end
```
<!-- end_slide -->

<!--
speaker_note: |
  Vedete quel 242? Quello è il problema. Mentre N masiucola, i, c, o, l esistono nel 'vocabolario base' dei computer, quella 'ò' è stata aggiunta con un'estensione che non tutti i sistemi utilizzano in modo corretto.
-->
Viaggio nel mondo dei caratteri
===

<!-- alignment: center -->
<!-- font_size: 2-->
```ruby
"Nicolò".each_char do |char|
  puts "#{char} => #{char.ord}"
end
```
```
N => 78
i => 105
c => 99
o => 111
l => 108
ò => 242
```
<!-- end_slide -->

<!--
speaker_note: |
  Questo vocabolario base è chiamato ASCII, ed essendo una codifica a 7 bit, al massimo potrà rappresentare 128 caratteri, con codici che vanno da 0 a 127. Quel 242 è chiaramente fuori scala per questo standard. Ruby è un linguaggio moderno, e di default funziona con codifiche di caratteri moderne come Unicode, rappresentato dalla sigla UTF-8.
-->
Viaggio nel mondo dei caratteri
===

<!-- alignment: center -->
<!-- font_size: 2-->
```ruby
name = "Nicolò"
puts name.encoding
```
```
UTF-8
```
<!-- end_slide -->

<!--
speaker_note: |
  Proviamo dunque a codificare il mio nome con il set ASCII.
-->
Viaggio nel mondo dei caratteri
===

<!-- alignment: center -->
<!-- font_size: 2-->
```ruby
name = "Nicolò"
puts name.encode("ASCII")
```
<!-- end_slide -->

<!--
speaker_note: |
  Questo causerà un crash del programma, perchè effettivamente quella dannata o accentata non è inclusa in questo dizionario di base. Per aggirare il problema, invece di trattare le cose correttamente, un team pigro potrebbe utilizzare tecniche per sostituire i caratteri problematici con quelli consentiti.
-->
Viaggio nel mondo dei caratteri
===

<!-- alignment: center -->
<!-- font_size: 2-->
```ruby
name = "Nicolò"
puts name.encode("ASCII")
```
```
Error in 'String#encode': U+00F2 from UTF-8 to US-ASCII (Encoding::UndefinedConversionError)
```
<!-- end_slide -->

<!--
speaker_note: |
  Qui vediamo come possiamo fornire una tabella di ripiego dove specifichiamo come gestire questi problemi, evitare i crash e convertire i caratteri in qualcosa che assomiglia a quello di origine.
-->
Viaggio nel mondo dei caratteri
===

<!-- alignment: center -->
<!-- font_size: 2-->
```ruby
name = "Nicolò"
puts name.encode("ASCII", undef: :replace)

fallback = {
  'À' => 'A', 'Á' => 'A', 'Â' => 'A', 'Ã' => 'A', 'Ä' => 'A',
  # molte altre...
  'ò' => 'o', 'ó' => 'o', 'ô' => 'o', 'õ' => 'o', 'ö' => 'o',
  # molte altre ancora...
}
puts name.encode("ASCII", fallback:)
```
<!-- end_slide -->

<!--
speaker_note: |
  E fu così, che il mio nome divenne Nicol punto di domanda oppure Nicolo.

  Per mancanza di tempo, o per pigrizia, questo viene considerato sufficiente. Dato che sento già le persone nei commenti che dicono "eh mi piacerebbe, ma devo integrarmi con servizi dell'età della pietra e non posso fare altrimenti", a loro dico: "lo so, purtroppo ci son passato anch'io". Con certi sistemi giurassici certe cose sono inevitabili.
-->
Viaggio nel mondo dei caratteri
===

<!-- alignment: center -->
<!-- font_size: 2-->
```ruby
name = "Nicolò"
puts name.encode("ASCII", undef: :replace)

fallback = {
  'À' => 'A', 'Á' => 'A', 'Â' => 'A', 'Ã' => 'A', 'Ä' => 'A',
  # molte altre...
  'ò' => 'o', 'ó' => 'o', 'ô' => 'o', 'õ' => 'o', 'ö' => 'o',
  # molte altre ancora...
}
puts name.encode("ASCII", fallback:)
```
```
Nicol?
Nicolo
```
<!-- end_slide -->

<!--
speaker_note: |
  Ma la storia non finisce qui. Perché a volte non sparisce semplicemente l'accento - a volte succedono cose ancora più strane. Vi ricordate che vi ho detto che ogni carattere ha un numero? Bene, quando mi iscrivevo agli esami dell'università invece succedeva questo
-->
Viaggio nel mondo dei caratteri
===

<!-- alignment: center -->
<!-- font_size: 2-->
```ruby
puts "Nicolò".force_encoding("ISO-8859-1")
```
<!-- end_slide -->

<!--
speaker_note: |
  Da qualche parte, il mio nome codificato correttamente in Unicode è stato interpretato come se fosse scritto con un altra codifica, e il risultato è decisamente non il mio nome. È come se avessi chiesto a qualcuno di leggere una parola italiana usando la pronuncia inglese.
-->
Viaggio nel mondo dei caratteri
===

<!-- alignment: center -->
<!-- font_size: 2-->
```ruby
puts "Nicolò".force_encoding("ISO-8859-1")
```
```
NicolÃ²
```
<!-- end_slide -->

<!--
speaker_note: |
  Sempre sulla stessa onda, questa è un' email di un noto servizio dove addirittura la o accentata è diventata radice-quadrata-minore-uguale.
-->
Viaggio nel mondo dei caratteri
===

<!-- jump_to_middle -->
<!-- alignment: center -->
Shopify email
<!-- end_slide -->

<!--
speaker_note: |
  Ora che abbiamo capito cosa succede 'sotto il cofano', torniamo agli scenari che ho descritto all'inizio. Perché questi non sono solo problemi tecnici - diventano vere e proprie barriere all'accessibilità.
-->
<!-- jump_to_middle -->
<!-- alignment: center -->
<!-- font_size: 5-->
Quando le traduzioni vanno storte
<!-- end_slide -->

<!--
speaker_note: |
  Pensate all'ironia: un servizio dello Stato italiano che non riesce a gestire nomi italiani! Agli albori del servizio ho cercato più volte di effettuare la procedura presso gli uffici postali per ottenere questa identità digitale, ma gli operatori non capivano perchè non potessero proseguire con la procedura. Nella confusione, mi diedero appuntamento con la direzione dell'ufficio, ma anche in questo caso nulla di fatto. Risultato? Almeno quattro ore perse, e ora uso un fornitore diverso per lo SPID. Spero che nel frattempo abbiano risolto questi problemi.
-->
Quando le traduzioni vanno storte
===
<!-- jump_to_middle -->
<!-- alignment: center -->
<!-- font_size: 5-->
PosteID per lo SPID
<!-- end_slide -->

<!--
speaker_note: |
  Il badge dell'ufficio è stato il più spettacolare. Un collega inserisce il mio nome nel sistema per associarlo a un badge specifico e... crash totale. Il gestionale non riusciva più nemmeno a ripartire. Abbiamo dovuto ripristinare il backup del giorno precendente, prima che il virus dell'accento infettasse il database. Un singolo carattere accentato che ha mandato KO l'intero sistema di accesso dell'edificio.
-->
Quando le traduzioni vanno storte
===
<!-- jump_to_middle -->
<!-- alignment: center -->
<!-- font_size: 5-->
Per te niente badge
<!-- end_slide -->

<!--
speaker_note: |
  Con Vinted è stato più sottile ma altrettanto frustrante. Io vendo, e per evitare le code alle poste porto i pacchi in tabaccheria. Il sistema genera dei QR che devono poi essere scansionati, ma le etichette con il mio nome come mittente non venivano lette dal terminale della tabaccheria. Non un errore, non un crash crash - semplicemente il sistema non reagiva. Dopo diversi giorni di infruttuose discussioni con il supporto di Vinted, ho deciso di menomare il mio nome e impostare Nicolo come mittente. Indovinate pure chi può portare i pacchi in tabaccheria. Comincio a vedere uno schema...
-->
Quando le traduzioni vanno storte
===
<!-- jump_to_middle -->
<!-- alignment: center -->
<!-- font_size: 5-->
Vinted + Poste
<!-- end_slide -->

<!--
speaker_note: |
  Questi episodi evidenziano bene la dualità del problema: a volte è un silenzioso "non funziona e basta", altre volte è un crash spettacolare. In entrambi i casi, la soluzione è sempre la stessa: rimuovi il carattere illegale e tutto torna a funzionare.
-->
Quando le traduzioni vanno storte
===
<!-- jump_to_middle -->
<!-- alignment: center -->
<!-- font_size: 5-->
Crash oppure silenzio assordante
<!-- end_slide -->

<!--
speaker_note: |
  Ma non è solo un problema nostro italiano. Questo bug colpisce mezzo mondo, e quando dico mezzo mondo, non esagero.

  José, nome spagnolo comunissimo, non riesce a prenotare voli online perché il sistema della compagnia aerea interpreta la 'é' come carattere non valido. François scopre che il sistema della sua banca ha trasformato il suo nome in 'Francois' - senza cediglia - e ora non riesce più ad accedere al conto perché il nome sulla carta d'identità non corrisponde. Björn in Svezia, non può registrarsi a un servizio di streaming perché quei due puntini sulla o non sono validi. Non oso nemmeno immaginare chi usa sistemi con caratteri cinesi, giapponesi o arabi.
-->
È (anche) una questione di accessibilità
===
<!-- jump_to_middle -->
<!-- alignment: center -->
<!-- font_size: 5-->
Josè, François, Björn
<!-- end_slide -->

<!--
speaker_note: |
  Cosa significa questo per le aziende? Potenzialmente clienti che abbandonano il carrello perché non riescono a completare la registrazione. Call center che devono gestire chiamate di persone confuse la cui risoluzione non è semplice finchè non salta fuori il problema dell'accento.
-->
<!-- jump_to_middle -->
<!-- alignment: center -->
<!-- font_size: 5-->
Utenti confusi = meno clienti
<!-- end_slide -->

<!--
speaker_note: |
  Il vero problema è che quando si sviluppa software, si testa sempre con gli stessi dati finti. 'John Smith', 'Mario Rossi', 'Anna Bianchi'. Dati puliti, perfetti, che esistono nel mondo reale, ma non sono i soli. A meno di essere Josè, François o Nicolò, non testeremmo mai questi dati. Quindi ogni volta che incontrate un form che non accetta il vostro nome, ricordatevi: non è colpa vostra. Da qualche parte nella catena di software sottostanti, c'è sicuramente una parte che è stata scritta da qualcuno che non ha mai pensato che il mondo fosse più complicato di 'Mario Rossi', oppure che non viene mai aggiornata dal 1970.
-->
È (anche) una questione di accessibilità
===
<!-- jump_to_middle -->
<!-- alignment: center -->
<!-- font_size: 5-->
"A Mario funziona" (cit.)
<!-- end_slide -->

<!--
speaker_note: |
  Cosa abbiamo imparato oggi? Che dietro ogni 'errore nel campo nome' potrebbe esserci una storia di encoding, di sistemi che non si parlano o di sviluppatori impotenti davanti a pachidermi non aggiornati.

  La prossima volta che vedete qualcuno che scrive il proprio nome senza accenti, senza apostrofi, senza caratteri speciali - ora sapete perché. Non è pigrizia, non è disattenzione. È sopravvivenza digitale.
-->
<!-- jump_to_middle -->
<!-- alignment: center -->
<!-- font_size: 5-->
Sopravvivenza digitale
<!-- end_slide -->

<!--
speaker_note: |
  E se siete tra quelli che sviluppano software, per favore: testate con nomi veri. Mettete dentro qualche José, qualche François, qualche Nicolò. Non costano niente, ma possono evitare utenti confusi, clienti persi o addirittura backup da ripristinare.
-->
<!-- jump_to_middle -->
<!-- alignment: center -->
<!-- font_size: 5-->
Più test per Josè, François e Nicolò
<!-- end_slide -->

<!--
speaker_note: |
  Io intanto continuo la mia battaglia quotidiana armato di o accentata e determinazione. E voi? Avete mai avuto problemi simili con i vostri nomi? Scrivetelo nei commenti - sono curioso di scoprire quanti altri 'bug viventi' ci sono là fuori.

  Ci vediamo al prossimo video, e ricordate: il vostro nome è perfetto così com'è. Sono i computer che devono adattarsi.
-->
<!-- jump_to_middle -->
<!-- alignment: center -->
<!-- font_size: 5-->
Alla prossima!
<!-- end_slide -->
