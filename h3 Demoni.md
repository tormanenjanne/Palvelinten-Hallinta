# h3 Demoni

## x) Lue ja tiivistä

### Apache installed with Ansible

Artikkelissa käydään läpi, kuinka apache2 webpalvelin voidaan asentaa automaattisesti. 

- `tasks` hakemistossa on kokonaisuudessaan ansiblen koodi apachelle. Mikä ohjelma asennetaan, minne tiedosto kopioidaan ja mikä on sen lähde sekä kenelle kuuluu oikeudet. Myös huomautuksena annetaan, että Apache2 pitää käynnistää uudelleen.
- `file` Sisältää tiedoston, jossa määritellään tiedoston sijainti, omistajat sekä ryhmät, link kohta kuvaa sen, että tästä tehdään symboliinen linkki jos sitä ei ole luotu tai sitä muokataan, jos se on jo olemassa.
- `handlers` käynnistää palvelun uudestaan, jos asetuksissa on muutoksia esimerkiksi jos uusi päivitys on tullut tähän. Tämä vaatii `notify` komennon tasks hakemiston main.yml tiedostoon.
- Kaikki nämä hakemistot sijaitsevat Apache2 roolin hakemistossa. (Ansible doc file; Ansible doc handlers; karvinen 2026)

### Handlers

- Viittaan edellisen apache 2 tehtävään, jossa vähän avasin jo handlerissa: "käynnistää palvelun uudestaan, jos asetuksissa on muutoksia esimerkiksi jos uusi päivitys on tullut tähän. Tämä vaatii `notify` komennon tasks hakemiston main.yml tiedostoon."
- handleri sisällytetään tasks hakemistoon esimerkiksi main.yml tiedostoon, jonne lisätään komento `notify`. Notifyyn pitää määritellä ne palvelimet, jotka halutaan uudelleen käynistyvän, jos jotain muutoksia on tullut. esimerkiksi `restart apache2`. (Ansible 2026)

### Service

Tämä moduuli toimii välityspalvelimena palvelunhallintamoduuleille, kuten esimerkiksi apache2.
- enabled: Tällä kehotetaan, pitääkö ohjelman startata käynnistyksen aikana. joko `enable` tai `state` atribuutti vähintää tarvitaan.
- name: pavelun nimi
- state: `started` käynnistää palvelun  ja `stopped` ei käynnistä. Nämä toteutuu vain tarvittaessa, jos on muutoksia. `restarted` polkaisee palvelimen käytiin aina uudestaan ja `reloaded` päivittää aina.

EXAMPLES: yksi esimerkki on tässä, jolla käynnistetään httpd, jos se ei ole jo käynnissä:
```markdown
- name: Start service httpd, if not started
  ansible.builtin.service:
    name: httpd
    state: started
```

- name: kerrotaan, mitä tässä kehotteessa tapahtuu
- ansible.builtin.service: Service moduuli
- name: mikä palvelin on kyseessä
- state: määritellään tila, eli tässä tapauksessa se käynnistetään. (ansible-doc service)

### Hacker news


## Asenna Apache2 käsin

Lähdin aloittamaan tehtävän päivittämällä paketit, ja sitten asentamaan Apache2 käsin näillä komennoilla

```markdown
sudo apt-get update
sudo apt-get upgrade
sudo apt install apache23
```

Seuraavaksi testasin komennolla `sudo apt systemctl apache2`, että ohjelma pyörii ja kokeilin vielä selaimessa, jotta saan auki apache2 oletussivun auki.

<img width="715" height="332" alt="kuva" src="https://github.com/user-attachments/assets/a2def39b-cdd7-411a-a374-fbb3cc27ef9c" />

<img width="933" height="737" alt="kuva" src="https://github.com/user-attachments/assets/b17ddca9-7267-4bc1-b83d-2794a754bf5a" />

Apache2 toimii normaalisti, ja seuraavaksi lähdemme muokkaamaan asetuksia siten, että voimme tavallisella käyttäjällä muokkata oikeuksia ilma sudo- ja root-oikeuksia.

Lähdin tekemään uuden hakemiston komennolla `sudo mkdir -p /var/www/testisivu`, sekä omistajuuden testikayttajalle, jonka olin luonut viime tehtävässä. Käytin tässä komentoa `sudo chown -R testikayttaja:testikayttaja /var/www/testisivu/`

Seuraavaksi kirjauduin testikayttajalla sisään `ssh testikayttaja@localhost`, ja loin tänne uuden index.html tiedoston komennolla `nano /var/www/testisivu/index.html`. nanossa tein tällainen sivun, joka pitäisi nyt näkyä.

<img width="727" height="404" alt="kuva" src="https://github.com/user-attachments/assets/10374825-955e-41b6-b150-621c5670fe7b" />

Sitten lähdin suorittamaan komennolla `sudo nano /etc/apache2/sites-available/testisivu.conf`, Teron ohjeistuksien mukaan.

<img width="711" height="221" alt="kuva" src="https://github.com/user-attachments/assets/d7157554-91e7-4020-95be-b09dbfb07d7a" />

Seuraavaksi otin sivun käyttöön komennolla `sudo a2ensite testisivu.conf`, ja oletussivun otin pois käytöstä komennolla `sudo a2dissite 000-default.conf`.

<img width="634" height="163" alt="kuva" src="https://github.com/user-attachments/assets/e608304f-b041-4f4b-9fff-016ddc6873d1" />

Lopuksi päivitin Apache2 komennolla `sudo systemctl reload apache2`. ja näin sivusto päivittyi!

<img width="951" height="238" alt="kuva" src="https://github.com/user-attachments/assets/d306aec1-8703-49a3-b644-583f03b79817" />

Nyt testaan, että sivusto on vielä muokattavissa testikayttajalla. Kirjauduin taas testikayttajalla sisään, ja avain tiedoston komennolla `nano /var/www/testisivu/index.html`, ja muokkasin tiedostoa hieman.

<img width="713" height="295" alt="kuva" src="https://github.com/user-attachments/assets/7c06b9ad-53f2-4c68-a89c-db992acb0968" />

Katsotaan näkyykö muutokset...

<img width="937" height="191" alt="kuva" src="https://github.com/user-attachments/assets/6aa44093-d849-4257-afef-5b01420e5a63" />

Muutokset näkyivä, joten tehtävä on onnistunut!








## Lähteet
- Ansible. Handlers: running operations on change. Luettavissa: https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html#notifying-handlers. Luettu: 12.11.2026.
- Ansible doc file
- Ansible doc-handers 
- Ansible-doc service
- Karvinen 2026. Luettavissa: https://terokarvinen.com/apache-ansible/. Luettu: 12.11.2026
- 

- 
