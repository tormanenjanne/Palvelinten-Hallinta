# h0 Oikeasti netissä.md
Tehtävän tarkoituksena on asentaa Debian virtuaalikoneeseen, VPS, tulimuuri, ssh sekä web-palvelin. Myös muutamaan kirjalliseen tehtävään pitää vastata.

## H4 Tehtävät (Kirjallinen tehtävä)

- Tässä tehtävässä annetaan step-by-step ohjeet, miten saat luotua itsellesi virtuaalipalvelimen UpCloudiin.
- Vaikka ohjeet ovat suoraviivaiset, on hyvä pysähtyä miettimään, miten saat ssh-key luotua sekä kumpi ssh-avaimista on hyvä liittää UpCloud sivulle. (SSH-keygen on julkinen avain, joka liitetää sivustolle. Yksityinen avain jää käyttäjälle.)
- Virtuaalipalvelimen alkutoimet on mielestäni kerrottu selvästi, sekä ne on hyvin toteutettavissa. palomuurin asentaminen on kerrottu selkeästi sekä muistutettu siitä, että pitää avata ssh-portti, jotta päästään palvelimelle sisälle komennolla `ufw allow 22/tcp`.
- Myös Web-palvelimen ohjeet ovat selkeät, sekä tässä raportissa on myös mainittu, miksi komennolla `sudo ufw allow 80/tcp` haluttiin avata http-liikenne, jotta se toimisi kaikilla käyttäjillä selaimessa.
- Kokonaisuudessaan selkeä raportti, mutta jäin pohtimaan tuota http-liikenteen sallimista. Se ei välttämättä ole hyvä juttu, koska liikenne selväkielistä.

## Harjoitus 5: Nimekäs kohta-a (Kirjallinen tehtävä)

- Tässä käydään läpi, kuinka saadan julkinen domain nimi omalle palvelimelle. Palveluntarjoana käytettiin namecheappia 
- Namecheap tarjoaa monia erilaisia vaihtoehto halvasta kalliimpaan vaihtoehtoon domain nimelle.
- Itse olisin miettinyt, jos teen omalla nimellä domain nimen, olisinko laittanut vähän enemmän rahaa ja laittanut joko .com tai .fi päätteen.
- Ohjeistukset ovat tässä selkeät, sekä lopussa myös mainitaan siitä, miten saadaan osoitettua @ sekä www suoraan omalle palvelimille. Hyvät ohjeet, jos haluaa omalle domainille nimen.

## Virtuaalikoneen asentaminen sekä Debian käyttöjärjestelmä

Tästä minulla ei ole valitettavasti kuvia, koska olin tehnyt jo tämän perjantaina ennen kuin tehtävänanto oltiin annettu, mutta käyn tämän kirjallisesti läpi.

Ensimäisenä Virtuaaliboksiin laitoin virtuaalikoneen tiedot, jotka olivat:
- 60gb virtuaalinen kovalevy
- 4019mb muistia
- 4 cpu

Virtuaalikoneen annoin nimeksi palvelinten-hallinta-1, sekä laitoin cd-asemalle ISO-tiedoston, jossa oli Debian käyttöjärjestelmä. Lopputulokseksi sain luotua virtuualikoneen. Tässä ei itselläni tuottanut mitään ongelmia.

<img width="603" height="657" alt="kuva" src="https://github.com/user-attachments/assets/6df8ba91-8b95-43d1-9595-ddd8cb7868f2" />

Kuva 1. Koneen tiedot

Debian asennukseen käytin Teron ohjeistuksia (lähde: https://terokarvinen.com/2021/install-debian-on-virtualbox/) Tässä vaiheittain miten tein itse tämän:
- kieli oli Suomi.
- Näppäimistön asetukset oli Finnish ja Default (Näin toimii ääkköset).
- Sitten oli tarkoitus tyhjentää kovalevy (erase disk).
- Lopuksi annoin nimeni (jannetormanen), käyttäjäni, jolla kirjaudun sisään (janne), tietokoneen nimi (palvelinten-hallinta-1) ja salasanan.
- Lopuksi oli yhteenveto, ja itse asennuksessa meni noin 20-30 minutttia.

Asennuksen jälkeen oli tarkoitus vielä komennolla terminaalissa `sudo apt-get update`, joka pävittää paketit ja tämän jälkeen `sudo apt-get -y dist-upgrade`, jolla asennettiin viimeisimmät versiot sovelluksista.

Lopuksi vielä varmistin, että sain copy pasten toimimaan näiden ohjeistuksien mukaisesti(Lähde: https://hhmoodle.haaga-helia.fi/pluginfile.php/4751440/mod_resource/content/1/VirtualBox%20Guest%20Additions%20guide.pdf.) Näitä ohjeistuksia noudattamalla, sain lopputuloksen, jossa copy paste toimi normaalisti.

En ole vielä uskaltanut laittaa palomuuria päälle --> koska edellisellä kerralla koneeni ei enään käynnistynt, kun kirjauduin sisään. Teen tämän varmaan myöhemmässä vaiheessa uudelleen.

## VPS vuokrataa virtuaalinen yksityispalvelin

Tämän vaiheen tein myös oppitunnilla luokan edessä opettajan avustuksella, joten otan screenshotteja sivustolla, miten tämä tehtäisiin. Palveluna valitisin suomalaisen UpCloudin.

Ensimmäisessä vaiheessa valitaan, mistä maasta haluamme serverimme. Valitsin suomen.

<img width="961" height="703" alt="kuva" src="https://github.com/user-attachments/assets/92c6c773-6a8e-4d7a-9138-6a9c8c727592" />

Seuraavan oli vuorossa valita, mitkä ovat palvelinkooneen spektit. Valitsin kaikista halvimman vaihtoehdon (CPU Core: 1, Memory: 1GB, Storage: 10GB, Price 3.00€/kuukaudessa), koska opettaja mainitsi tunnilla, että tämä varmasti riittää meidän tarpeisiimme ainakin alkuun.

<img width="947" height="335" alt="kuva" src="https://github.com/user-attachments/assets/d0e72ab6-01c5-4c32-ac09-ee89f1b6b098" />

Tämän jälkeen seuraavat kohta **Strorage** sekä **Automated Backups** jätettiin tyhjäksi.

<img width="838" height="787" alt="kuva" src="https://github.com/user-attachments/assets/46f590c3-7a12-4a47-9548-e8769e72d151" />

Käyttäjärjestelmäksi valittiin Debian 13 Trixie sekä **Network** kohdassa luotiin myös julkinen IPv4 ja IPv6-osoitteet, myös **Utility Network** kohtaan **yes**.

<img width="608" height="914" alt="kuva" src="https://github.com/user-attachments/assets/30c19ca0-e8cc-47f4-b2d5-93b5c7c3cc41" />

jatka vielä ssh...

##  Alkutoimet. Tee virtuaalipalvelimellesi sudo-käyttäjä omalla nimellä (ei root). Asenna tulimuuri - ensin reikä ssh:lle. Päivitä kaikki ohjelmat ja boottaa tarvittaessa.

Seuraavaksi kokeilin nyt uudestaan, sainko palomuurin toimimaan, ja pääsen kirjautumaan normaalisi virtuaalikoneelleni. Komentona käytin `sudo apt-get -y install ufw` ja tänän jälkeen
`sudo ufw enable`

<img width="491" height="61" alt="kuva" src="https://github.com/user-attachments/assets/2722a375-4028-4e73-a738-cebe7b63c7c1" />

Nyt sain palomuurin toimimaan, ja se on aktiivisena. En tosiaan tiedä, mitä kävi silloin aikasemmin, kun asensin palomuurin ja virtuaalikoneeni ei mennyt eteenpäin kirjautumiskohdasta. Tässä vielä kuva jossa näkyy, että virtuaalikoneessani on palomuuri päällä. Komennollaa `sudo ufw status` näkyy, että palomuurin status on **enable**

<img width="549" height="66" alt="kuva" src="https://github.com/user-attachments/assets/7ac0eafd-658f-417e-b8c7-c65ef459e94d" />


Seuraavana oli tarkoitus asentaa omalle virtuaalikoneelleni ssh, jotta saan yhteyttä virtuaalipalvelimeeni. ssh-asennus tapahtui komennolla `sudo apt-get -y install ssh`, ja tämän jälkeen komennolla `sudo systemctl enable --now ssh` saadaan ssh päälle heti käynnistäessä. Tässä kuva vielä, jossa ssh on laitettu päälle käynnistyksen yhteydessä.

<img width="722" height="79" alt="kuva" src="https://github.com/user-attachments/assets/6620b161-05b1-4a2b-b1b0-6c729ee03c75" />

Testasin myös vielä, toimiiko ssh komennolla `ssh localhost` ja kirjautuminen onnistui! Kirjauduin vielä ulos ssh:sta komennolla `exit`.

Tämän jälkeen loin SSH-avaimet, jotta pääsen kirjautumaan tällä virtuaalikoneella minun virtuaalipalvelimelleni. Komennolla `ssh-keygen` luodaan avainparit ja painamalla muutaman kerran entteriä, saadaan luotua julkinen sekä yksityinen avainpari. Näin ollaan saatu luotua avainparit meidän käyttäjälle.

Seuraavaksi siirretään julkinen avainpari suoraan meidän virtuaalipalvelimelle, jotta pääsemme kirjautumaan sisään. komennolla ``cat ~/.ssh/id_ed25519.pub` saadaan näkyviin meidän ssh-julkinen avainpari ja liitetään se UpCloudiin. Tässä vielä kuva, kun se on liitettynä tuonne, mutta en uskaltanut näyttää sisältöä.

<img width="662" height="344" alt="kuva" src="https://github.com/user-attachments/assets/3f32608e-652b-49d2-a33d-20dd8a683707" />

Tein uuden virtuaalipalvelinkoneen, koska en saanut liitettyä vanhaa uutta ssh-avainta... jatketaan tästä eteenpäin. Ensiksi avataan portti ssh- yhteydelle ennenkuin laitamme palomuurin päälle. Ensimmäisenä asennetaan palomuuri komennolla `sudo apt-get update` ja sitten `sudo apt install ufw`. seuraavaksi sitten `sudo ufw allow 22/tcp` ja `sudo ufw enable`. Tällä varmistetaan, että ei ssh-yhteys ei kaadu.

<img width="722" height="154" alt="kuva" src="https://github.com/user-attachments/assets/19de7183-fa94-438f-a757-b85546e371bb" />

Tämän jälkeen tein käyttäjän, joka ei ole root. syötin terminaalissa `adduser janne ` ja samalla sudo oikeudet `sudo adduser janne sudo`. Tämän jälkeen loin uuden kansion, johon liitän myöhemmin ssh avaimen `mkdir -p /home/janne/.ssh` ja sitten oikeudet `chmod 700 /home/aatu/.ssh`. Lopuksi nanossa lisätään julkinen avain yhdelle riville ` nano /home/janne/.ssh/authorized_keys`. (Tämä avain pitää kopioida .ssh kansiosta id_ed255519.pub)

Viimeisenä asennetaan oikeudet komennolla: `chmod 600 /home/aatu1/.ssh/authorized_keys` ja omistajuus: `chown -R aatu:aatu /home/aatu/.ssh`.

Lopputulokseksi pääsin kirjautumaan janne käyttäjällä, tässä vielä kuva:

<img width="696" height="190" alt="kuva" src="https://github.com/user-attachments/assets/72165704-1431-4b65-b58d-9b02b29e2e0b" />

Seuraavana vielä lukitaan root-oikeudet komennolla `sudoedit /etc/ssh/sshd_config` ja muokkasin riviltä **PermitRootLogin yes** --> **PermitRootLogin no**, ja lopuksi vielä `sudo service ssh restart`

## Web-palvelin Apache2 asennus

Kirjauduin ssh:lla janne@iposoite minun virtuaalipalvelimelle ja päivitin paketit `sudo apt update` ja asensin Apache2 komennolla `sudo apt-get install apache2`. Vielä lopuksi pitää sallia http-portin pääsy komennolla `sudo ufw allow 80/tcp` ja `sudo ufw enable`. Lopputulokseksi sain Apachen sivun näkymään `http://oma ip-osoite`. Tässä vielä kuva:

<img width="891" height="786" alt="kuva" src="https://github.com/user-attachments/assets/f265596a-23d0-41dc-bad3-8297627bf9af" />




### lähteet

Karvinen 2024. Install Debian on Virtualbox - Updated 2024. Luettavissa: https://terokarvinen.com/2021/install-debian-on-virtualbox/. Luettu: 23.10.2026.

Heinonen 2025. VirtualBox Guest Additions. Luettavissa: https://hhmoodle.haaga-helia.fi/pluginfile.php/4751440/mod_resource/content/1/VirtualBox%20Guest%20Additions%20guide.pdf. Luettu: 23.10.2026.

Niemi 2025. H4 Tehtävät. Luettavissa: https://github.com/AONCODES/linux-course/blob/main/h4.md. Luettu: 23.10.2026.

Partanen 2025. Lokaalinen koneen tiedot. Luettavissa: https://partanennn.github.io/linux-servers/practices/fifth-week.html. Luettu: 23.10.2026

Karvinen 2017 Luettavissa: https://terokarvinen.com/2017/first-steps-on-a-new-virtual-private-server-an-example-on-digitalocean/. Luettu: 24.10.2026
