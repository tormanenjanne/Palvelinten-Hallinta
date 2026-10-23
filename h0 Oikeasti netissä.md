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



### lähteet

Karvinen 2024. Install Debian on Virtualbox - Updated 2024. Luettavissa: https://terokarvinen.com/2021/install-debian-on-virtualbox/. Luettu: 23.10.2026.
Heinonen 2025. VirtualBox Guest Additions. Luettavissa: https://hhmoodle.haaga-helia.fi/pluginfile.php/4751440/mod_resource/content/1/VirtualBox%20Guest%20Additions%20guide.pdf. Luettu: 23.10.2026.
Niemi 2025. H4 Tehtävät. Luettavissa: https://github.com/AONCODES/linux-course/blob/main/h4.md. Luettu: 23.10.2026.
Partanen 2025. Lokaalinen koneen tiedot. Luettavissa: https://partanennn.github.io/linux-servers/practices/fifth-week.html. Luettu: 23.10.2026
