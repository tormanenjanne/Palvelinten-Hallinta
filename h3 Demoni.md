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


## a) Asenna Apache2 käsin

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

Muutokset näkyivä, joten tehtävä on onnistunut! Tässä vielä kuva, jossa näkyy omistajuudet, eli testikäyttäjällä on kirjoitus sekä luku oikeudet. (Digitalocean 2022; Karvinen 2026)


<img width="724" height="219" alt="kuva" src="https://github.com/user-attachments/assets/5486a7b6-144a-4284-93d4-edb4a17c1bca" />

## b) Moottorix, asenna Nginx käsin.

Asensin ensimmäisenä Nginx komennolla ` sudo apt install nginx`, ja tämän jälkeen Teron ohjeistuksien mukaan, apache2 piti sulkea. Käytin tässä komentoa `sudo systemctl stop apache2` ja käynnistin nginx komennolla `sudo systemctl start nginx`. Lopuksi vielä testasin komennolla ` systemctl status apache2` ja `systemctl status nginx`.

<img width="709" height="352" alt="kuva" src="https://github.com/user-attachments/assets/da2ac870-f923-4227-9462-4e8ce58691b0" />

Kuten huomaamme kuvasta, Apache2 active kohta = dead ja nginx on = running, eli nginx on päällä ja apache2 on pois päältä.

sekä localhost osoite toimii kanssa. Tässä piti vähän kaivella oikeaa osoitetta, mutta se löytyi /var/www/html`, ja oikea osoite oli tämä: http://localhost/index.nginx-debian.html


<img width="862" height="336" alt="kuva" src="https://github.com/user-attachments/assets/ff6eb746-345f-4b08-b156-a6ce9c619b30" />

Seuraavaksi loin hakemiston `sudo mkdir /home/testikayttaja/nginxtestisivu`, sekä loin tiedoston tänne komennolla `sudo mkdir /home/testikayttaja/nginxtestisivu`.


<img width="826" height="180" alt="kuva" src="https://github.com/user-attachments/assets/0922f13f-83bd-4ae5-8c70-f19a8429e647" />

Seuraavaksi oikeudet.

```markdown
chmod ugo+x /home/testikayttaja/
chmod ugo+x /home/testikayttaja/nginxtestisivu/
chmod ugo+r /home/testikayttaja/nginxtestisivu/index.html
sudo chown -R testikayttaja:testikayttaja /home/testikayttaja/nginxtestisivu
```

nämä antavat oikeudet execute oikeudet hakemistoihin sekä tiedostoon lukuoikeus. lopussa vaihdoin omistajaksi testikayttaja.

Seuraavaksi loin asetustiedoston komennolla `sudo nano /etc/nginx/sites-available/nginxtestisivu`, ja lisäsin tällaisen sisällön.

<img width="905" height="282" alt="kuva" src="https://github.com/user-attachments/assets/7f09a708-e841-4aa4-8dde-0a7464556731" />

Seuraavaksi ajoin komennon `sudo ln -s /etc/nginx/sites-available/nginxtestisivu /etc/nginx/sites-enabled/`, jolla luodaan symbolinen linkki, ja lopuksi komento `sudo nginx -t`, jotta ei ole mitään virheitä 

<img width="606" height="56" alt="kuva" src="https://github.com/user-attachments/assets/bad25aeb-4d1d-47c2-be53-c5f7b06f3f70" />

lopuksi `sudo systemctl restart nginx`, ja sivusto toimii!


<img width="956" height="168" alt="kuva" src="https://github.com/user-attachments/assets/5ea9924b-cb88-48ee-a265-611566ca3cd8" />


<img width="960" height="167" alt="kuva" src="https://github.com/user-attachments/assets/9e8623ec-147e-41dd-8b7b-177b9d937fd4" />

Lopuksi tein testikäyttäjällä testauksen, saanko ilman sudo-oikeuksia tehtyä muutoksia. Kirjauduin sisään, ja komennolla `nano /home/testikayttaja/nginxtestisivu/index.html ` pääsin muokkaamaan sivustoa. Alhaalla vielä kuva tästä, sekä cat komennolla näytettävä sisältö.

<img width="953" height="172" alt="kuva" src="https://github.com/user-attachments/assets/5f64327c-81e1-4349-bb90-89e7b22d0b95" />

<img width="919" height="194" alt="kuva" src="https://github.com/user-attachments/assets/2a61d976-a64d-41c6-b74f-8fafda2cef8e" />












## Lähteet
- Ansible. Handlers: running operations on change. Luettavissa: https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html#notifying-handlers. Luettu: 12.11.2026.
- Ansible doc file
- Ansible doc-handers 
- Ansible-doc service
- Digitalocean 2022. Install Apache Web Server on Debian: Full Guide. Luettavissa: https://www.digitalocean.com/community/tutorials/how-to-install-the-apache-web-server-on-debian-11. Luettu 13.11.2026.
- Karvinen 2026. Luettavissa: https://terokarvinen.com/apache-ansible/. Luettu: 12.11.2026
