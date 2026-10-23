# h0 Oikeasti netissä.md
Tehtävän tarkoituksena on asentaa Debian virtuaalikoneeseen, VPS, tulimuuri, ssh sekä web-palvelin. Myös muutamaan kirjalliseen tehtävään pitää vastata.

## H4 Tehtävät (Kirjallinen vastaus)

- Tässä tehtävässä annetaan step-by-step ohjeet, miten saat luotua itsellesi virtuaalipalvelimen UpCloudiin.
- Vaikka ohjeet ovat suoraviivaiset, on hyvä pysähtyä miettimään, miten saat ssh-key luotua sekä kumpi ssh-avaimista on hyvä liittää UpCloud sivulle. (SSH-keygen on julkinen avain, joka liitetää sivustolle. Yksityinen avain jää käyttäjälle.)
- Virtuaalipalvelimen alkutoimet on mielestäni kerrottu selvästi, sekä ne on hyvin toteutettavissa. palomuurin asentaminen on kerrottu selkeästi sekä muistutettu siitä, että pitää avata ssh-portti, jotta päästään palvelimelle sisälle komennolla `ufw allow 22/tcp`.
- Myös Web-palvelimen ohjeet ovat selkeät, sekä tässä raportissa on myös mainittu, miksi komennolla `sudo ufw allow 80/tcp` haluttiin avata http-liikenne, jotta se toimisi kaikilla käyttäjillä selaimessa.
- Kokonaisuudessaan selkeä raportti, mutta jäin pohtimaan tuota http-liikenteen sallimista. Se ei välttämättä ole hyvä juttu, koska liikenne selväkielistä.

## Harjoitus 5: Nimekäs kohta-a (Kirjallinen)

- Tässä käydään läpi, kuinka saadan julkinen domain nimi omalle palvelimelle. Palveluntarjoana käytettiin namecheappia 
- Namecheap tarjoaa monia erilaisia vaihtoehto halvasta kalliimpaan vaihtoehtoon domain nimelle.
- Itse olisin miettinyt, jos teen omalla nimellä domain nimen, olisinko laittanut vähän enemmän rahaa ja laittanut joko .com tai .fi päätteen.
- Ohjeistukset ovat tässä selkeät, sekä lopussa myös mainitaan siitä, miten saadaan osoitettua @ sekä www suoraan omalle palvelimille. Hyvät ohjeet, jos haluaa omalle domainille nimen.

### lähteet

Niemi 2025. H4 Tehtävät. Luettavissa: https://github.com/AONCODES/linux-course/blob/main/h4.md. Luettu: 23.10.2026.
Partanen 2025. Lokaalinen koneen tiedot. Luettavissa: https://partanennn.github.io/linux-servers/practices/fifth-week.html. Luettu: 23.10.2026
