# h4 Pizza Fantasia

## x) Lue ja tiivistä.

### Size and Complexity of Some DSLs

- Salt sisältää 510 erilaista funktiota, joiden avulla järjestelmävalvoja voi hallita agenttikoneita. Dokumentaatio on 20000 riviä, ja yli 75000 sanaa, joka ei sisällä erillisiä ohjausrakenteita muun muassa haarautumisia tai looppeja. Toimitan toteutetaan käyttäen Jinja2-mallia, joka muutetaan python muotoon suorittaakseen funktio.
- Puppet sisältää 113 erilaista funktiota, johon ei sisälly ohjausrakenteita. Esimerkiksi, jos järjestelmävalvoja haluaa muodostaa uusia käyttäjiä siten, että kaikilla on samanlaiset asetukset. Tavallisesti ohjelmointikielet muodoostaa funktion tästä, mutta Puppet määrittelee näihin omat käsitteet. (Karvinen 2023, 112)

### Use of DSL Functions in Case Configuration

- Tiivistettynä mikä itelle jäi mielee, oli taulukossa esitetty Mozilla toimintaa, josta 49% käsitteli komentoja: Case, File ja Package. Kokonaisuudessaan näillä pärjää. Tekstissä mainitaan myös, exec-toiminta, jos on jotain valmiita funktioita, jotka eivät sovellu tehtävään. Tätä suositellaan käyttämään, kun tila on idempotentti.
- Yhdysvallan hallinnon konfiguraation perustasolla taas suurimmaksi osaksi käytettiin: augeas, file, service, exec ja package. augeasilla voidaan muokata tiedostoja.
- yhteenveto itselle on se, että muutamilla toiminnolla voi saada jo toimivan ohjelmiston. (Karvinen 2023, 112-115)

### Dependencies Between Main Functions

- Tärkempinä funktioina on package-file-service, user, group ja exec. Näitä käytetään yleensä demonien määrittämiseen.
- Artikkelissa avaataan se, mitä olen jo aikaisemmin maininnut, että pienillä funktioilla saadaan jo toimiva rakenne. (karvinen 2023, 115-117)

## a) Räpylä. Asenna itse valitsemasi demoni käsin

Lähdin asentamaan Mariadb:tä, joka on myös tietokantaohjelmisto. Päivitin paketit eka komennolla `sudo apt-get update`, ja tämän jälkeen lähdin asentamaan ohjelmaa komennolla `sudo apt install mariadb-server mariadb-client galera-4`. Oheistuksissa oli käyttää tätä komentoa `sudo mariadb-secure-installation`, mutta jätin sen välistä, koska ohjeistuksessa sanottiin MariaDB:n olevan Debianissa oletuksissa suojattu, eikä sitä välttämättä suositella ajamaan.

<img width="705" height="157" alt="kuva" src="https://github.com/user-attachments/assets/fa63bf2c-d14e-45a6-ab8d-88d16fd4e6f4" />

katsoin seuraavaksi, onko mariadb aktiivisena komennolla `sudo systemctl status mariadb`, ja huomaamme, että tila on active eli se on käynnissä. (Mariadb s.a)

Komennolla `sudo mariadb` saimme tietokantaohjelman käytiin. Testasin vielä komennolla `SHOW DATABASES;`, tulee tietokannat näkyviin ja ne tulivat. Onnistunut asennus siis!

<img width="949" height="195" alt="kuva" src="https://github.com/user-attachments/assets/d98a342b-b4bf-4e92-9a0a-5082d30821ea" />


<img width="896" height="213" alt="kuva" src="https://github.com/user-attachments/assets/b8eb9fef-5f34-4df9-8c1d-54e4f862f5b0" />

## b) Automaatti. Automatisoi valitsemasi demonin asennus Ansiblella.

Aloitin tehtävän menemällä ensimmäisenä ansible hakemistoon, ja luomalla täällä hakemistorakenteen komennolla `mkdir -p roles/mariadb/tasks`

<img width="342" height="41" alt="kuva" src="https://github.com/user-attachments/assets/1e05e502-8a2d-41d8-af3c-390ed35ecb7c" />

Tämän jälkeen loin main.yml tiedoston komennolla `micro roles/mariadb/tasks/main.yml`. Tiedostoasennus näytti tältä:

<img width="897" height="130" alt="kuva" src="https://github.com/user-attachments/assets/cc098e71-0a8a-4b4b-87a1-47520caf72e0" />


- apt: asentaa paketin
- name: paketin nimi
- state present: varmistaa sen, että paketti on asennettu.

- service: Hallitsee tässä tapauksessa mariadb:tä eli voi jokoa käynnistää, stoppaa tai uudelleenkäynnistää.
- name: mariadb eli tämä ohjelma
- state: started: käynnistetään mariadb, ellei se ole jo käytössä.

Lopuksi lisäsin vielä site.yml tiedostoon mariadb roolin `micro site.yml`.

<img width="884" height="216" alt="kuva" src="https://github.com/user-attachments/assets/114482f0-e7e5-4964-a31e-42857bcfc676" />

Sitten ansible ajoa. Käytin tässä komentoa `ansible-playbook site.yml -K`.

Ajossa huomasin, että mitään ei tapahtunut mariadb:seen viittaavaa asiaa, joten lähdin tarkistamaan, ja huomasin, että main.yml tiedosto ei ole tasks hakemistossa. korjataan...

Siirsin siis tiedoston komennolla mv roles/mariadb/main.yml roles/mariadb/tasks/main.yml. Tässä siis ensimmäinen osa on tuo alkuperäinen polku, ja toisessa välilyönnin jälkeen on uusi polku. Kokeillaan uudestaan.

<img width="935" height="168" alt="kuva" src="https://github.com/user-attachments/assets/7e60d450-7611-4268-876f-1840b8a97471" />

Näyttää tapahtuvan ei mitään eli onnistunut "asennus". Tämän asennus oltiin siis jo tehty aikasemmin käsin, joten muutoksia tähän ei tullut.

## c) Asetus. Muuta asetustiedostoa herralla (master, "control node") ja aja ansible uudestaan. Osoita, että asetukset tulivat käyttöön.

Lähdin suorittamaan tehtävää luomalla ansibleen files hakemiston komennolla `mkdir -p roles/mariadb/files`

Seuraavaksi tein cnf tiedoston komennolla `micro roles/mariadb/files/asetukset.cnf`. cnf pääte siksi, koska katsoin aikasemmin `/etc/mysql/mariadb/mariadb.conf.d` hakemistosta, mikä on tämän tiedoston loppupääte.

<img width="683" height="217" alt="kuva" src="https://github.com/user-attachments/assets/25b59956-5414-4b94-9b07-5a04e31b26ea" />


Micro tiedostoon lisäsin nämä asetukset. Kyseinen asetus sulkee yhteyden mariadb:ssä, jos tietokantapalvelussa ei tehdä mitään toimenpiteitä 100 sekunnin aikana. Kyseeessä on globaali arvo (mariadb s.a;Sharma 2023) Toisaalta tämä on vähän hölmö asetus, mutta tässä nähdään ainakin se, että toimiiko tämä.

<img width="269" height="50" alt="kuva" src="https://github.com/user-attachments/assets/ddd60cb2-d49c-4b76-b36b-e05af8705d9e" />

Seuraavaksi lisäsin copy tiedot `mariadb/tasks/main.yml` tiedostoon. Tässä käytin Teron ohjeistuksia Ansible tehtävssä hieman mukaillen. Tässä siis asetukset.cnf tiedosto kopioidaan etc/mysql/mariadb/mariadb.conf.d/asetukset.cnf hakemistoon, sekä määritellään käyttäjälle sekä ryhmälle root oikeudet. Vielä seuraavaksi pitää Handleri asentaan, kun tulee muutoksia niin uudet asetukset tulevat voimaan. (karvinen 2026)


<img width="556" height="286" alt="kuva" src="https://github.com/user-attachments/assets/cffce2f8-3ff1-4ccc-bd70-d37a2b2f9ca3" />

Eli seuraavaksi Handerin kimppuun, ja sama loru kuin aikasemmissa hakemistorakenteen luomisissa. Eli luodaan hakemistopolku `mkdir -p roles/mariadb/handlers` ja microssa main.yml tiedosto `micro roles/mariadb/handlers/main.yml`. Tiedoston luomisessa hyödynsin myös Teron ansiblen ohjeistuksia mukautuen mariadb asetuksiin.

<img width="463" height="89" alt="kuva" src="https://github.com/user-attachments/assets/40c50818-a537-479c-93b4-0cea6a1b99f5" />

Sitten ajetaan playbook.

<img width="926" height="182" alt="kuva" src="https://github.com/user-attachments/assets/bd6ab7e2-51de-45c0-80c5-cbaa9db3a540" />

Huomaamme muutoksen tässä, joten tehtävä on onnistunut! Handleri ei toiminut, koska asetuksista puuttu state: restarted. Tämän takia se näytti ok.

Katsoin vielä `/etc/mysql/mariadb.conf.d` hakemistosta, tuliko asetukset.cnf tiedosto tänne, ja siellähän se näkyi!

<img width="684" height="228" alt="kuva" src="https://github.com/user-attachments/assets/05961709-11f9-4460-974e-6befe2eddf83" />

Toisaalta mariadb:ssä tämä näytti edelleen oletusarvoa 28800 sekuntia, koitetaan ajaa uudestaan playbook, ja muutetaan asetukset.cnf asetuksen arvoa 100 -> 200

<img width="387" height="83" alt="kuva" src="https://github.com/user-attachments/assets/af0cd691-e392-4553-b968-b8b1588d2d72" />

Ajetaan playbook, ja katsotaan tuleeko muutokset.

<img width="939" height="164" alt="kuva" src="https://github.com/user-attachments/assets/c18e31dd-4684-457c-b24d-97f70599b6e2" />

ainakin Mariadb käynnistettiin uudelleen, sekä tiedosto on kopioitu uudelleen hakemistorakenteeseen. katsotaan mariadb:ssä näkyykö asetukset.

Mariadb:seen pääsee kirjautumaan komennolla `sudo mariadb`, ja täällä pystyy katsomaan onnistuiko timeout funktio komennolla `SHOW GLOBAL VARIABLES LIKE 'wait_timeout'`. (Sharma 2023)
<img width="685" height="125" alt="kuva" src="https://github.com/user-attachments/assets/d1cdeeb4-25c4-4a34-87a1-4dd72d8f2316" />

Muutos näkyi, joten tehtävä on nyt onnistunut.

## d) Paikka remonttiin. Riko jotain asetuksia sekä e) Idempotentti. Osoita, että tilasi on idempotentti.

Lähdin muokkaamaan tässä etc/mysql/mariadb/mariadb.conf.d/asetukset.cnf tiedostoa, ja otin kokonaan wait_timeout = 200 pois sieltä. Seuraavaksi toivotaan, että ansible korjaa tämän tilanteen...

<img width="861" height="106" alt="kuva" src="https://github.com/user-attachments/assets/ccd334fb-975e-4788-925e-b2aa682a1bc3" />


Ajetaan playbook, ja katsotaan tilanne... Muutoksia on tullut ja hyvä niin. Katsotaan vielä asetukset.cnf tiedostoa komennolla `etc/mysql/mariadb/mariadb.conf.d/asetukset.cnf` miltä siellä näyttää. Kuvassa huomaamme, että ansible on päivittänyt tämän tiedoston. 

<img width="954" height="178" alt="kuva" src="https://github.com/user-attachments/assets/4f4e0a7c-3d31-46bc-8702-ffec844bd8f1" />

<img width="763" height="287" alt="kuva" src="https://github.com/user-attachments/assets/8b15f481-9e01-4953-a707-d85fd4e2d7e0" />

Lopuksi vielä Mariadb:ssä, että wait_timeout 200 sekuntia näkyy siellä. Voidaan todeta tässä vaiheessa, että ansible osasi korjata tämän tilanteen.

<img width="763" height="152" alt="kuva" src="https://github.com/user-attachments/assets/01e8c5fc-e287-4692-8a91-6299988b3642" />

Vielä lopuksi osoitetaan se, että idempotentti varmistuu tässä, eli mitään muutoksia ei tule. Ajetaan vielä kerran playbook.


<img width="948" height="180" alt="kuva" src="https://github.com/user-attachments/assets/447a7ea7-0ef0-42fe-a5c2-a0c2d3f36f3f" />

Mikään ei muuttunut, joten idempotentti on varmistunut tässä.

## f) hanki pari Moodlen kautta yhteiseen projektiin

Viime tunnilla keskustelin vieruskaverin kanssa, haluaisiko hän tehdä kanssani projektin, ja vastaus oli myönteinen. Ainakin puheiden perusteella minulla on jo pari.


## Lähteet
- karvinen 2026 Luettavissa: Apache installed with Ansible - quick notes. Luettavissa. https://terokarvinen.com/apache-ansible/. Luettu: 20.11.2026.
- Karvinen, T. 2023. Väitöskirja.  Configuration Management of Distributed Systems over Unreliable and Hostile Networks. University of Westminster. Luettu: 20.11.2026
- Mariadb s.a. Installing MariaDB Server Guide. Luettavissa: https://mariadb.com/docs/server/mariadb-quickstart-guides/installing-mariadb-server-guide. Luettu: 20.11.2026.
- Mariadb s.a. Server System Variables. Luettavissa: https://mariadb.com/docs/server/server-management/variables-and-modes/server-system-variables#wait_timeout. Luettu: 20.11.2026.
- Sharma, A. 2023. MariaDB wait_timeout. https://www.educba.com/mariadb-wait_timeout/. Luettu: 20.11.2026.



