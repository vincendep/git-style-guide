# Git Style Guide

Questa è una guida di stile Git ispirata da [*How to Get Your Change Into the Linux
Kernel*](https://kernel.org/doc/html/latest/process/submitting-patches.html),
dalle pagine del [manuale git](http://git-scm.com/doc) e altre pratiche diffuse
nella community.

Sono disponibili traduzioni nelle seguenti lingue:

* [Cinese (Semplificato)](https://github.com/aseaday/git-style-guide)
* [Cinese (Tradizionale)](https://github.com/JuanitoFatas/git-style-guide)
* [Francese](https://github.com/pierreroth64/git-style-guide)
* [Tedesco](https://github.com/runjak/git-style-guide)
* [Greco](https://github.com/grigoria/git-style-guide)
* [Italiano](https://github.com/vincendep/git-style-guide)
* [Giapponese](https://github.com/objectx/git-style-guide)
* [Coreano](https://github.com/ikaruce/git-style-guide)
* [Polacco](https://github.com/mbiesiad/git-style-guide/tree/pl_PL)
* [Portoghese](https://github.com/guylhermetabosa/git-style-guide)
* [Russo](https://github.com/alik0211/git-style-guide)
* [Spagnolo](https://github.com/jeko2000/git-style-guide)
* [Tailandese](https://github.com/zondezatera/git-style-guide)
* [Turco](https://github.com/CnytSntrk/git-style-guide)
* [Ucraino](https://github.com/denysdovhan/git-style-guide)

Se ti senti di contribuire, per favore fallo! Fai un fork del progetto e apri
una pull request.

# Sommario

1. [Branch](#branch)
2. [Commit](#commit)
  1. [Messaggi](#messaggi)
3. [Merge](#merge)
4. [Misc.](#misc)

## Branch

* Scegli dei nomi *corti* e *descrittivi*:

  ```shell
  # good
  $ git checkout -b oauth-migration

  # bad - troppo vago
  $ git checkout -b login_fix
  ```

* Identificatori di ticket appartenenti a servizi esterni (eg. Github 
  issue) sono ottimi candidati per i nomi di un branch. Ad esempio:

  ```shell
  # GitHub issue #15
  $ git checkout -b issue-15
  ```

* Usa il lowercase nei nomi dei branch. Gli identificatori dei ticket esterni
  in uppercase sono una valida eccezione. Usa i *trattini* per separare le parole.

  ```shell
  $ git checkout -b new-feature      # good
  $ git checkout -b T321-new-feature # good (ID task Phabricator)
  $ git checkout -b New_Feature      # bad
  ```

* Quando più persone lavorano sullo *stesso* branch, potrebbe essere conveniente
avere dei feature branch *personali* ed un feature branch *condiviso dal team*.

  ```shell
  $ git checkout -b feature-a/master # Branch condiviso dal teamn
  $ git checkout -b feature-a/maria  # Branch personale di Maria
  $ git checkout -b feature-a/nick   # Branch personale di Nick
  ```
  
  Sarà possibile effetuare dei merge dei branch personali nel branch condiviso dal team (vedi ["Merge"](#merge)).
  Eventualmente, verrà eseguito il merge del branch condiviso dal team nel "master".

* Elimina il tuo branch dall'upstream repository una volta effettuato il merge,
  a meno che non ci sia una ragione specifica per non farlo.

  Tip: Per elencare i branch reintegrati nel master usa 
  il seguente comando:

  ```shell
  $ git branch --merged | grep -v "\*"
  ```

## Commit

* Ogni commit dovrebbe essere un unico *cambiamento logico*. Non effettuare
  più *cambiamenti logici* in un unico commit. Ad esempio, se una patch
  elimina un bug ed ottimizza le performance di una feature, dividila in
  due commit separati.

  *Tip: Usa `git add -p` per effettuare uno stage interattivo di specifiche
  porzioni dei file modificati.*
  
* Non dividere un unico *cambiamento logico* in più di un commit. Ad esempio,
  l'implementazione di una feature e i corrispondenti test dovrebbero essere
  nello stesso commit.

* Effettua i commit in *anticipo* e *spesso*. Commit piccoli e auto-contenuti
  sono più facile da capire e annullare quando qualcosa va storto.

* I commit dovrebbero essere ordinati *logicamente*. Ad esempio, se il *commit X*
  dipende dai cambiamenti introdotti nel *commit Y*, allora il *commit Y* dovrebbe
  venire prima del *commit X*.

Nota: Mentre lavori in solitaria in branch locali che *non sono stati pushati*,
è consentito fare dei commit come degli snapshot temporanei del tuo lavoro. 
Tuttavia, rimane il fatto che bisogna applicarli *prima* di effettuare il push;

### Messaggi

* Usa l'editor, non il terminale, quando scrivi i messaggi dei commit:

  ```shell
  # good
  $ git commit

  # bad
  $ git commit -m "Quick fix"
  ```

  Fare i commit dal terminali incoraggia a comprimere
  il messaggio in una singola linea, il che solitamente lo rende ambiguo 
  e non informativo.

* Il sommario (ie. la prima riga del commit) dovrebbe essere *descrittivo*
  e *succinto*. Idealmente, non dovrebbe essere più lungo di *50 caratteri*.
  Dovrebbe essere scritto  con l'iniziale maiuscola e nel tempo imperativo presente.
  Non dovrebbe terminare con un punto dato che è il *titolo* del commit.

  ```shell
  # good - tempo presente imperativo, maiuscola, meno di 50 caratteri
  Mark huge records as obsolete when clearing hinting faults

  # bad
  fixed ActiveModel::Errors deprecation messages failing when AR was used outside of Rails.
  ```

* Dovrebbe seguire una riga bianca a sua volta seguita da una descrizione
  più dettagliata. Essa dovrebbe essere contenuta in *72 caratteri* e spiegare
  *perchè* i cambiamenti erano necessari, *come* risolvono il problema e quali
  *effetti collaterali* potrebbero avere.

  Dovrebbe inoltre fornire degli indicatori a risorse correlate (eg. link 
  al corrispondente bug tracker):

  ```text
  Breve (50 caratteri o meno) intro ai cambiamenti

  Testo più dettagliato ed esplicativo, se necessario. 
  Racchiudilo in 72 caratteri. In certi contesti, la prima
  riga è considerata come il soggetto di una email ed il 
  resto del testo come il corpo. La riga bianca che separa
  il titolo dal corpo è di importanza critica (a meno che non
  ometti il corpo completamente); alcuni strumenti come potrebbero
  essere confusi se non li separi.

  Dopo la riga bianca sono presenti ulteriori paragrafi.

  - I punti di elenco sono consentiti

  - Usa un trattino oppure un asterisco per il punto,
    seguito da uno spazio singolo, con righe bianche nel mezzo

  Gli indicatori alle risorse correlate possono servire come footer
  per i tuoi messaggi dei commit. In questo esempio ci sono dei
  riferimenti alle issue di bug tracker:

  Risolve: #56, #78
  Vedi anche: #12, #34

  Fonte http://tbaggery.com/2008/04/19/a-note-about-git-commit-messages.html
  ```

  In ultimo, mentre scrivi il messaggio del commit, pensa a cosa vorresti 
  sapere se ti imbattessi in questo commit fra un anno.
* Se il *commit A* dipende dal *commit B*, la dipendenza dovrebbe
  essere specificata nel messaggi del *commit A*. Usa l'hash quando 
  fai un riferimento a dei commit.

  Similmente, se il *commit A* risolve un bug introdotto con il *commit B*,
  dovrebbe essere espresso nel messaggio del *commit A*.
  
* Se effettui uno squash dei commit usa le opzioni `--squash` e `--fixup`,
  al fine di rendere l'intezione più chiara.
  
  ```shell
  $ git commit --squash f387cab2
  ```

  *(Tip: Usa l'opzione `--autosquash` quando effettui un rebase. I commit segnati
  saranno compressi automaticamente.)*

## Merge

* **Non modificare la storia pubblica.** La storia del repository è preziosa
  ed è importante poter definire *cos'è successo veramente*. Modificare la storia
  pubblicata è fonte comune di problemi per chiunque lavori sul progetto.

* Tuttavia, ci sono casi in cui riscrivere la storia è legittimo. Questi sono
  quando:

  * Sei l'unico a lavorare su un branch non in fase di revisione.

  * Vuoi riordinare il tuo branch (eg. squash dei commit) e/o eseguire un rebase
    sul "master" al fine di effetuare un merge succesivamente.

  Quindi, *non riscrivere mai la storia del branch "master"* o qualsiasi altro
  branch speciale (ie. utilizzati dalla produzione oppure dai server CI).

* Mantieni la storia *pulita* e *semplice*. *Prima di eseguire il merge* del tuo branch:
  
    1. Assicurati che esso sia conforme allo stile guida ed esegui ogni
      azione necessaria (squash/riordinamento commit, correzione messaggi etc.)

    2. Esegui un rebase sul branch in cui verrà effettuato il merge:

       ```shell
       [my-branch] $ git fetch
       [my-branch] $ git rebase origin/master
       # poi merge
       ```
      
       Succesivamente il branch potrà essere applicato direttamente alla 
       fine del "master" e la storia risulerà semplice e pulita.
       
       *(Nota: Questa strategia è efficace per i progetti con merge frequenti.
       Altrimenti potrebbe essere meglio eseguire occasionalmente un merge nel "master" 
       invece che un rebase.)*

* Se il tuo branch include più di un commit, non eseguire un merge con fast-forward:

  ```shell
  # good - assicurati che il commit di merge venga creato
  $ git merge --no-ff my-branch

  # bad
  $ git merge my-branch
  ```

## Misc.

* Ci sono vari workflow ed ognuno ha i suoi punti di forza e debolezze.
  La scelta del workflow potrebbe dipendere dal team, dal progetto e 
  e dalle tue procedure di sviluppo.

  Detto questo, è importante *scegliere* un workflow e rimanere consistenti.
  
* *Sii coerente.* Questo si applica al workflow ma può essere espanso
  ad altre cose come i messaggi dei commit, i nomi dei branch e i tag. 
  Avere uno stile consistente per tutto il repository permette di capire
  facilmente cosa sta succedendo guardando un log, un messaggio di un commit ecc.

* *Esegui i test prima di un push.* Non pubblicare lavoro svolto a metà.

* Usa gli [annotated_tag](https://git-scm.com/book/en/v2/Git-Basics-Tagging#_annotated_tags)
  per segnare le release oppure altri punti importanti nella storia. Preferisci i
  [lightweight tags](https://git-scm.com/book/en/v2/Git-Basics-Tagging#_lightweight_tags)
  per uso personale, come evidenziare dei commit per un futuro riferimento.

* Mantieni i tuoi repository in buona forma eseguendo occasionalmente
  task di mantenimento:

  * [`git-gc(1)`](http://git-scm.com/docs/git-gc)
  * [`git-prune(1)`](http://git-scm.com/docs/git-prune)
  * [`git-fsck(1)`](http://git-scm.com/docs/git-fsck)

# Licenza

![cc license](http://i.creativecommons.org/l/by/4.0/88x31.png)

Questo lavoro è sotto la licenza [Creative Commons Attribution 4.0
International license](https://creativecommons.org/licenses/by/4.0/).

# Credits

Agis Anastasopoulos / [@agisanast](https://twitter.com/agisanast) / http://agis.io
... and [contributors](https://github.com/agis-/git-style-guide/graphs/contributors)!
