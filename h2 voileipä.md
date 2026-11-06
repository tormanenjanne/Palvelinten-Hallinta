# h2 voileipä

## x) analyysi

### Sudo without password
- Luodaan ensimmäiseksi uusi käyttäjä sekä lisätään se sudoless ryhmään.
- Seuraavaksi avataan editori ja määritellään säännöt, tämän ryhmän koneille komennolla `%sudoless ALL = (ALL) NOPASSWD: ALL`. %sudoless ALL = (ALL) == % tarkoittaa tätä ryhmää eli sudoless ja kaikille koneille. (ALL) NOPASSWD: ALL == kaikille käyttäjille sääntö, ettei tarvitse käyttää salasanaa.
- Lopuksi testataan sen toimivuutta kirjautumalla ssh:lla sisään, ja esimerkiksi echo viestillä sen toimivuutta.

### Sandwitch
- Sarjakuvassa käydään sudo oikeuden vahvuus.
- Ensimmäiseksi sohvalla istuva kaveri kysyy tee minulle voileipä, mutta toinen kaveri vastaa tee itse.
- Seuraavaksi sohvalla istuva kaveri käskee sudolla tee minulle voileipä, ja täten toinen kaveri tekee sen.
- Tässä nähdään sudon vahvuus, eli root-oikeuksilla saadaan asiat tapahtumaan olisi tilanne mikä tahansa.

### Passwordless Sudo with Ansible
- Suositus on luoda ensimmäisenä manuaalisesti käyttäjä ennen automatisointia sekä ansible pitää toimia.
- `tree` komennolla voidaan nähdä hakemistorakenne ja selvittää, mitä tiedostoa haluamme muuttaa. Tässä tapauksessa `main.yml`, joka on uuden käyttäjän asetukset sekä määritykset.
- Ensimmäisellä ajolla ei ole sudo ilman salasanaa, joten `become: true` pitää lisätä site.yml lohkoon. Lopuksi ajetaan playbook ja sudo komento annetaan lisäyksellä -K.


### Ansible doc-copy
- ansible doc-copy: Kopioi tiedoston/hakemiston paikalliselta/etäkoneelta haluamallesi etäkoneelle.
- dest: etäkoneen/orjakoneelle polku
- src: Lokaalin koneen polku orjakoneelle
- Owner: Tiedoston omistaja
- Group: Käyttäjät, ketkä kuuluvat ryhmään
- mode: Mitkä oikeudet annetaan kohteelle (Ansible-doc copy)

<img width="715" height="390" alt="kuva" src="https://github.com/user-attachments/assets/77d30ced-4d43-4436-9dc0-44e8373d43dc" />


### ansible-doc apt
Paketinhallinta ohjelma
- name: Listaa paketin nimestä sekä mikä versio on käytössä.
- State: paketin tila voi olla: `latest`== viimeisin versio paketista, `absent`==poistaa paketin, `present`== asentaa paketin, jos sitä ei ole vielä.
- Update_cache: Suorittaa saman komennon kuin `apt-get update`. Tämä komento voi olla mukana pakettien asennuksessa tai jälkeenpäin. Oletuksena ei päivitetä välimuistia. (Ansible-doc apt)

<img width="723" height="433" alt="kuva" src="https://github.com/user-attachments/assets/0a320e1c-d322-4b20-8dfe-e4cc92a12b2f" />


### ansible-doc file
Voidaan määrittää tiedostojen, hakemistojen tai symboolisten linkkien kohteet.
- path: polku halutun tiedostolle, jota haluamme kontrolloida.
- recurse: Määrittää rekursiivisesti tiedoston/hakemiston sisällön.
- src: Polku tiedostolta linkkiin.
- state: Tiedoston eri tiloja
- owner: Tiedoston omistaja
- group: Ryhmä, jolla on omistusoikeudet tiedostolle
- mode: Mitkä oikeudet on olemassa. (ansible-doc file)

<img width="725" height="498" alt="kuva" src="https://github.com/user-attachments/assets/7f221493-829b-4239-a107-b3cb874f888c" />

### ansible-doc user
Hallitaan käyttäjiä sekä oikeuksia
- name: Käyttäjän nimi
- create_home: jos asetus on `false`, käyttäjälle luodaan kotihakemisto ellei sitä ole jo olemassa.
- comment: Kommenttikenttä
- groups: Lisäryhmä luettelo, jossa käyttäjä on myös mukana. Oletusarvoisesti käyttäjä on poistettu kaikista ryhmistä. `append` komennolla voidaan lisätä käyttäjä
- shell: mitä komento-ohjelmaa käyttäjä käyttää
- state: Käyttäjän tila
- system: `state=present` asetus määrittää käyttäjälle käyttäjätilin. (ansible-doc user)

<img width="724" height="442" alt="kuva" src="https://github.com/user-attachments/assets/e4855376-1695-40f6-878c-e7fb635ef067" />

### ansible-doc authorized_key
Lisätään tai poistetaan ssh-avain käyttäjältä
- user: Käyttäjänimi orjakoneella, jonka ssh-avaimia voidaan muuttaa.
- key: julkisen ssh-avaimen siirtäminen käyttäjälle esimerkiksi Github kautta. (ansible-doc authorized_key)

<img width="724" height="428" alt="kuva" src="https://github.com/user-attachments/assets/43b257ee-b806-4729-b4dd-ce212374a2d3" />

## a) sudoless. Tee ansiblea varten tunnus, jolla voi käyttää sudoa ilman salasanaa. Sekä ssh-kirjautuminen että sudon käyttö tulee olla ansbilea varten automatisoitu.

Ensimmäiseksi tein teron ohjeistuksien mukaan uuden käyttäjän, loin uuden käyttäjänryhmän `groupadd`, ja lisäsin testikayttajan sudoless ryhmään. Komennot löytyvät tästä:


```markdown
sudo adduser testikayttaja
sudo groupadd sudoless
sudo adduser testikayttaja sudoles
```
<img width="948" height="334" alt="kuva" src="https://github.com/user-attachments/assets/8a51596c-f604-4d47-bf31-64597624669f" />

Avasin myös uuden terminaalin, ja käytin komentoa sudo -i, jotta pääsen root-käyttäjksi. Tällä terminaalilla voimme korjata sudon, jos rikomme sen. Jatketaan normaalinkäyttäjän terminaalissa.

Seurvaaksi lisättiin sääntö sudoless ryhmään `sudo visudo /etc/sudoers.d/sudoless`, ja komento sääntöön oli tämä:

```markdown
%sudoless ALL = (ALL) NOPASSWD: ALL
```
- %sudoless = tarkoittaa ryhmää.
- ALL = kaikille ryhmän koneille.
- (ALL) = mille käyttäjille.
-  NOPASSWD: ALL == ensimmäinen on promti, ettei tarvitse salasanaa ja all kuvastaa tätä kaikille.

<img width="719" height="204" alt="kuva" src="https://github.com/user-attachments/assets/afcf6445-e554-4d1a-a13b-7ad1db3a8b57" />

Seuraavaksi testasin kirjautua ssh:lla, mutta se kysyi tässä vielä salasanaa. kohdassa `sudo -k` (-k unohtaa tässä mahdolliset salasanat) ja testasin komennolla `sudo echo "testi"`, jotta tässä ei tulisi salasanan kyselyä, ja näinhän siinä kävi! Tämä tapahui käyttäen komentoa `sudo -k`.

<img width="722" height="279" alt="kuva" src="https://github.com/user-attachments/assets/7e3d1e86-5833-4da3-bd1a-1a49ba1b0032" />

Tilanteen korjaamiseen, lisätään vielä julkinenavain testikayttajalle. Ensimmäiseksi kirjaudutaan ulos testikayttajalta, ja sitten lisätään julkinen ssh-avain komennolla `ssh-copy-id testikayttaja@localhost`. Tästä tulee ilmoitus vielä, että avaimen lisääminen on onnistunut.


<img width="940" height="227" alt="kuva" src="https://github.com/user-attachments/assets/04a2224e-ec26-4a31-8612-cf71fd5de197" />

Lopuksin testasin vielä, että pääsin kirjautumaan ssh:lla ilman salasanaa komennolla `ssh testikayttaja@localhost`. Lopputulos oli onnistunut, ja testikayttajalla onnistui ssh:lla kirjautuminen ilman salasanaa.


<img width="946" height="226" alt="kuva" src="https://github.com/user-attachments/assets/eff962e8-7c99-4494-89c4-d4d92ee25f6a" />


## b) antero. Tee salasanaton, automaattisesti ssh:lla kirjautuva tunnus Ansiblella

Aloitin tehtävän menemällä janne käyttäjällä ansible hakemistoon. Tämän jälkeen katsoin komennolla `tree -F` hakemistorakennetta. Päätin luoda ensimmäiseksi luoda hakemistorakenteen komennolla 'mkdir -p roles/uusikayttaja/tasks', jossa saadaan kaikki yläkansiot myös luotua.

<img width="698" height="493" alt="kuva" src="https://github.com/user-attachments/assets/c5ed0e7a-948c-4d78-8ed7-331d884b9af6" />

Seuraavaksi luodaan main.yml tiedosto komennolla, jonka jälkeen luodaaan koodina tarvittavat tiedot.

```markdown
micro roles/uusikayttaja/tasks/main.yml
```

<img width="891" height="759" alt="kuva" src="https://github.com/user-attachments/assets/21dc2e10-ffbc-4a91-ba66-a2b0f759f799" />

- Group: nimi sekä ryhmä tila. tällä hetkellä present eli se pitää luoda, jos tätä ei ole olemassa
- User: nimi, tila (suorittaa saman toiminnon mitä ryhmässä), ryhmät mihin ryhmiin käyttäjä lisätään
- authorized_key: käyttäjänimi sekä julkinenavain siirretään tänne
- copy: kopioidaan tiedot, mihin ollaan aikaisemmin luotu salasanaton komento, ja content kohtaa tämä kyseinen komento. Seuraavaksi määritellään oikeudet tässä tapauksessa omistajalle sekä ryhmälle.

Julkisenavaimen tiedot kävin hakemassa ssh kansiosta, ja tiedoston lopussa pitää olla .pub, jotta tiedetään, että kyseessä on julkinen avain. Tämän avasin komennolla `cat ja/tiedostonimi.pub`. Lopuksi siirsin copy pastella julkisen avaimen uusikayttajan main.yml tiedostolle.

Seuraavaksi lisäsin `uusikayttaja` hosts.ini tiedostoon ja vielä Teron ohjeistuksien mukaan lisäsin site.yml tiedostoon vielä `become:true` ja roles `- uusikayttaja`

<img width="365" height="147" alt="kuva" src="https://github.com/user-attachments/assets/34c34ac8-169b-4127-b0ff-30fe1319aa35" />

Nyt testataan ajoa, mielestäni kaikki pitäisi olla nyt kunnossa..


<img width="947" height="480" alt="kuva" src="https://github.com/user-attachments/assets/a019e893-550a-4ad1-8271-f54353320c06" />
playbook ajo.

<img width="940" height="256" alt="kuva" src="https://github.com/user-attachments/assets/10db94b0-70b8-4377-bf9e-5c1f0a5bb03f" />
root oikeudet kuva











## Lähteet
- Ansible doc-copy. Luettu: 5.11.2026
- Ansible-doc apt. Luettu: 5.11.2026
- Ansible-doc file. Luettu: 5.11.2026
- Ansible-doc user. Luettu: 5.11.2026
- Ansible-doc authorized_key. Luettu: 5.11.2026
- Karvinen 2026. Sudo without password. Luettavissa: https://terokarvinen.com/passwordless-sudo/. Luettu: 5.11.2026.
- Karvinen 2026. Passwordless Sudo with Ansible. Luettavissa: https://terokarvinen.com/passwordless-sudo-with-ansible/#. Luettu: 5.11.2026.
- Munroe 2026. Sandwitch. Luettavissa: https://xkcd.com/149/. Luettu: 5.11.2026.
- 
