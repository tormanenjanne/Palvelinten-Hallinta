# h4 Pizza Fantasia

## x) Lue ja tiivistä.

### Size and Complexity of Some DSLs
Karvinen 2023: Configuration Management of Distributed Systems over Unreliable and Hostile Networks (pdf, ̣mirrors: local, archive.org), vain nämä kohdat:

    4.12.1 Size and Complexity of Some DSLs (112. Ominaisuuksien määrä.)
    4.12.2 Use of DSL Functions in Case Configuration (112-115. Mitä oikeasti käytetään.)
    4.12.3.1 Dependencies Between Main Functions (115-117. Tärkeimmät rakennuspalikat.)


- Salt sisältää 510 erilaista funktiota, joiden avulla järjestelmävalvoja voi hallita agenttikoneita. Dokumentaatio on 20000 riviä, ja yli 75000 sanaa, joka ei sisällä erillisiä ohjausrakenteita muun muassa haarautumisia tai looppeja. Toimitan toteutetaan käyttäen Jinja2-mallia, joka muutetaan python muotoon suorittaakseen funktio
- Puppet sisältää 113 erilaista funktiota, johon ei sisälly ohjausrakenteita. Esimerkiksi, jos järjestelmävalvoja haluaa muodostaa uusia käyttäjiä siten, että kaikilla on samanlaiset asetukset. Tavallisesti ohjelmointikielet muodoostaa funktion tästä, mutta Puppet määrittelee näihin omat käsitteet.

### Use of DSL Functions in Case Configuration

- Tiivistettynä mikä itelle jäi mielee, oli taulukossa esitetty Mozilla toimintaa, josta 49% käsitteli komentoja: Case, File ja Package. Kokonaisuudessaan näillä pärjää. Tekstissä mainitaan myös, exec-toiminta, jos on jotain valmiita funktioita, jotka eivät sovellu tehtävään. Tätä suositellaan käyttämään, kun tila on idempotentti.
- Yhdysvallan hallinnon konfiguraation perustasolla taas suurimmaksi osaksi käytettiin: augeas, file, service, exec ja package. augeasilla voidaan muokata tiedostoja.
- yhteenveto itselle on se, että muutamilla toiminnolla voi saada jo toimivan ohjelmiston.

### Dependencies Between Main Functions

- Tärkempinä funktioina on package-file-service, user, group ja exec. Näitä käytetään yleensä demonien määrittämiseen.
- Artikkelissa avaataan se, mitä olen jo aikaisemmin maininnut, että pienillä funktioilla saadaan jo toimiva rakenne.

## a) Räpylä. Asenna itse valitsemasi demoni käsin

Lähdin asentamaan Mariadb:tä, joka on myös tietokantaohjelmisto. Päivitin paketit eka komennolla `sudo apt-get update`, ja tämän jälkeen lähdin asentamaan ohjelmaa komennolla `sudo apt install mariadb-server mariadb-client galera-4`. Oheistuksissa oli käyttää tätä komentoa `sudo mariadb-secure-installation`, mutta jätin sen välistä, koska ohjeistuksessa sanottiin MariaDB:n olevan Debianissa oletuksissa suojattu, eikä sitä välttämättä suositella ajamaan.

<img width="705" height="157" alt="kuva" src="https://github.com/user-attachments/assets/fa63bf2c-d14e-45a6-ab8d-88d16fd4e6f4" />

katsoin seuraavaksi, onko mariadb aktiivisena komennolla `sudo systemctl status mariadb`, ja huomaamme, että tila on active eli se on käynnissä.

Komennolla `sudo mariadb` saimme tietokantaohjelman käytiin. Testasin vielä komennolla `SHOW DATABASES;`, tulee tietokannat näkyviin ja ne tulivat. Onnistunut asennus siis!

<img width="949" height="195" alt="kuva" src="https://github.com/user-attachments/assets/d98a342b-b4bf-4e92-9a0a-5082d30821ea" />


<img width="896" height="213" alt="kuva" src="https://github.com/user-attachments/assets/b8eb9fef-5f34-4df9-8c1d-54e4f862f5b0" />



