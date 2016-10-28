---
layout: post
title: Configurazione Minitest, Capybara e Fixtures
categories: 
---

## UMB, Umile servo dei Binari 🚊
Essendosi il cielo svuotato di idoli da adorare mi sono rifatto nella programmazione cercando di essere il più fedele possibile ai binari imposti da DHH e dal suo team di Basecamp ⛺️.
In fin di conti scrive libri e articoli interessanti, ha una azienda che funziona alla grande e ha creato un gran framework.

Purtroppo la grande maggioranza non lo fa (maledetti ribelli!) e quindi sembra impossibile riuscire a configurare dei test di integrazione con i seguenti ingredienti:

- Capybara (con supporto Javascript)

- Fixtures 

- Minitest 

Tutto come [raccomandato da DHH](http://david.heinemeierhansson.com/2014/tdd-is-dead-long-live-testing.html); sempre sia lodato.

## Alla ricerca dei binari 🚂
Per questo motivo è stato molto difficile trovare una ricetta per la questione. Tutti gli esempi che si trovano online usano rspec e girl factory. 
Questo già succede normalmente quando cerco informazioni sui test con minitest e fixtures. In questo caso però la situazione peggiora: si arriva al deserto informativo.

C'è da dire che, in ogni caso, spiegazioni punto di vista di RSpec sul meccanismo di Capybara e selenium si trovavano e hanno funzionato come riferimento.

## Da Rack a Poltergeist, passando per Selenium
Già decidere il corretto web driver è stata esperienza non banale e travagliata. Fino a prima usavo rack come webdriver ed era tutto molto semplice e funzionante.
Purtroppo il cliente ha bisogno di Javascript e quindi devo poter testare anche questi aspetti legati direttamente al comportamento dei miei pericolosi script js.
Per questo prima ho provato con _selenium_, il più celebrato nel [readme di Capybara](https://github.com/jnicklas/capybara/). 
Era il mio primo vero tentativo di configurare una cosa del genere. Quindi sono andato incontro ad un ginepraio niente male. 

## Selenium non mi ama 😔
Installando Selenium ho scoperto che per usarlo bisognava scaricare firefox (ancora dopo ho scoperto che si poteva anche usare chrome, ma già non mi fidavo delle mie configurazioni... figuriamoci personalizzarle ulteriormente) e pure un file che non ricordo che credo facesse da proxy (evviva la precisione di questa informazione).

In ogni caso finalmente inizia la magia.
Firefox si apre e automatizza il processo.
Però ho un sacco di errori.
Il login non funziona.
Faccio mille prove, leggo i logs e scopro che (si lo so potevo leggere il readme di capybara fino in fondo avete ragione) Capybara quando usa questa tipologia di webdriver deve creare un altro applicativo e quindi le operazioni sul database non vengono "recepite"; thread differenti. In teoria dovrebbero funzionare solo le fixtures.
Ora voi direte: beh ma Ale, tu usi le fixtures quindi sei a posto!
Il cavolo!!
Per il login in particolare ero solito preparare un utente al volo ( User.create!(blabla) ), inserirlo nel db e usare quello. Tutto ciò perchè non sapevo nelle fixtures come salvare un utente admin con relativa password.

Ok. Questo mi ha obbligato a imparare ad usare un attimo meglio le Fixtures.
Ecco la soluzione.

    one:
      email: info@mail.com
      encrypted_password: <%= Devise::Encryptor.digest(Agent, 'password') %>

Le fixtures vengono lette e inserite per prime.
Poi il processo si biforca.
Da una parte rails e da un'altra parte Capybara.
A quel punto si capisce che non sarà semplice come prima.
La cosa peraltro fastidiosa è che Selenium continua a dirmi che il login non va a buon fine.
Mi viene inoltre in mente che io uso (da Bravo Servitore del Pensiero Unico Imposto da DHH) turbolinks. E quindi magari i test non fanno in tempo "a capire" quello che sta succendo.


## Poltergeist è molto simpatico e gentile 😍
Sperando di risolvere tutto in un colpo i miei problemi cambio driver ed uso il più rapido (così viene descritto in alcuni articoli online) Poltergeist.
In effetti l'approccio è più amichevole e le cose finalmente funzionano come mi aspettavo. Gioia e stupore!
Decido quindi di darci dentro e scopro (giustamente) che devo [cambiare anche qualche riga di javascript](https://simonini.github.io/articles/rails/tdd/integration-test/capybara/selectize/javascript/2016/10/28/come-interagire-con-selectize-tramite-capybara.html).

## Un nuovo nemico 😈
    class CreateBusiness < ActionDispatch::IntegrationTest
      test 'create business from admin' do
        login_admin
        fill_admin_scheda_and_submit 'azienda-x', '123fantecavalloere'
        b = Business.find_by(name: 'azienda-x')
        assert_equal current_path, business_path(b)
      end
    end

Cosa potrà mai succedere? Tutto ciò sembra molto logico.
Il primo è un metodo custom che mi logga
Poi c'è ne uno che mi riempie la scheda dell'azienda.
L'ultimo verifica solo di essere nella pagina corretta.
I log mi danno ragione.
MA.
MA.
Ho comunque un errore.
Affronto ancora una volta il problema dei thread separati.
Cari Thread non siate soli e tristi.
Rimanete assieme.
Decido finalmente di leggere fino in fondo questo link [http://technotes.iangreenleaf.com/posts/the-one-true-guide-to-database-transactions-with-capybara.html](http://technotes.iangreenleaf.com/posts/the-one-true-guide-to-database-transactions-with-capybara.html).
Alla fine scopro che bastava usare una schifosetta gemma e tutto il mondo si allineava col mio spirito 😱 e soprattutto i Thread condividevano le preziosi informazioni del mio db.

Ma si sa. È giusto soffrire.
[https://github.com/iangreenleaf/transactional_capybara]()
Ancora meglio sarebbe imparare a leggere fino alla fine gli articoli invece che scrollare a casaccio.
Caro Ale.
Insomma le cose ora funzionano.
Insomma non è che abbia esattamente capito cosa ci sta sotto.
E probabilmente questa configurazione può anche funzionare con Selenium.
Per ora diciamo che riesco ad andare avanti con i test di integrazione e che dovrò utilizzare in giro i vari helper per gestirmi le chiamata ajax derivanti dall'uso di turbolinks.

## Configurazioni corrette

Di seguito incollo il __Gemfile__  e il __test\_helper__ che fa funzionare i miei nuovi amici: _test di integrazione_.
Sono poche righe e interventi in apparenza banali ma non è stato semplice individuarli.

    # Gemfile
    group :test do
      gem 'capybara'
      gem 'poltergeist'
      gem 'transactional_capybara'
    end

--

    # test_helper.rb
    # Di seguito evidenzio solo gli interventi e non tutto il file

    [..]
    require 'capybara/poltergeist'
    [..]
    class ActionDispatch::IntegrationTest
      include Capybara::DSL
      TransactionalCapybara.share_connection
      Capybara.default_driver = :poltergeist
      [..]
      def teardown
        Capybara.reset_sessions!
        sleep(1)
      end