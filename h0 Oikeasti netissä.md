# h0 Oikeasti netissä.md
Tehtävän tarkoituksena on asentaa Debian virtuaalikoneeseen, VPS, tulimuuri, ssh sekä web-palvelin. Myös muutamaan kirjalliseen tehtävään pitää vastata.

## H4 Tehtävät (Kirjallinen vastaus)

- Tässä tehtävässä annetaan step-by-step ohjeet, miten saat luotua itsellesi virtuaalipalvelimen UpCloudiin.
- Vaikka ohjeet ovat suoraviivaiset, on hyvä pysähtyä miettimään, miten saat ssh-key luotua sekä kumpi ssh-avaimista on hyvä liittää UpCloud sivulle. (SSH-keygen on julkinen avain, joka liitetää sivustolle. Yksityinen avain jää käyttäjälle.)
- Virtuaalipalvelimen alkutoimet on mielestäni kerrottu selvästi, sekä ne on hyvin toteutettavissa. palomuurin asentaminen on kerrottu selkeästi sekä muistutettu siitä, että pitää avata ssh-portti, jotta päästään palvelimelle sisälle komennolla 'ufw allow 22/tcp'.
- Myös Web-palvelimen ohjeet ovat selkeät, sekä tässä raportissa on myös mainittu, miksi komennolla 'sudo ufw allow 80/tcp' haluttiin avata http-liikenne, jotta se toimisi kaikilla käyttäjillä selaimessa.
- Kokonaisuudessaan selkeä raportti, mutta jäin pohtimaan tuota http-liikenteen sallimista. Se ei välttämättä ole hyvä juttu, koska liikenne selväkielistä.
- 
