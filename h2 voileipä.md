# h2 voileipä

## x) analyysi

### Sudo without password
- Luodaan ensimmäiseksi uusi käyttäjä sekä lisätään se sudoless ryhmään.
- Seuraavaksi avataan editori ja määritellään säännöt, tämän ryhmän koneille komennolla `%sudoless ALL = (ALL) NOPASSWD: ALL`. %sudoless ALL = (ALL) == % tarkoittaa tätä ryhmää eli sudoless ja kaikille koneille. (ALL) NOPASSWD: ALL == kaikille käyttäjille sääntö, ettei tarvitse käyttää salasanaa.
- Lopuksi testataan sen toimivuutta kirjautumalla ssh:lla sisään, ja esimerkiksi echo viestillä sen toimivuutta.

### Sandwitch
- Sarjakuvassa käydään sudo-oikeuden vahvuus.
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
- Group: Ryhmä, kenellä on oikeus käyttää tiedostoa
- mode: Mitkä oikeudet annetaan kohteelle, voi olla joko symboolisessa muodossa tai bitteinä (Ansible-doc copy)

<img width="715" height="390" alt="kuva" src="https://github.com/user-attachments/assets/77d30ced-4d43-4436-9dc0-44e8373d43dc" />


### ansible-doc apt
Paketinhallinta ohjelma
- name: Voidaan kirjoittaa, mitä tässä tapahtuu. Esimerkisi päivitetään ohjelman paketti versioon x.
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
- create_home: jos asetus on `true`, käyttäjälle luodaan kotihakemisto ellei sitä ole jo olemassa.
- comment: Kommenttikenttä
- groups: Lisäryhmä luettelo, jossa käyttäjä on myös mukana. Oletusarvoisesti käyttäjä on poistettu kaikista ryhmistä. `append` komennolla voidaan lisätä käyttäjä
- shell: mitä komento-ohjelmaa käyttäjä käyttää
- state: Käyttäjän tila
- system: `state=present` asetus määrittää käyttäjälle käyttäjätilin. (ansible-doc user)

<img width="724" height="442" alt="kuva" src="https://github.com/user-attachments/assets/e4855376-1695-40f6-878c-e7fb635ef067" />

### ansible-doc authorized_key
Lisätään tai poistetaan ssh-avain käyttäjältä
- user: Käyttäjänimi orjakoneella, jonka ssh-avaimia voidaan muuttaa.
- key: julkinen avain siirretään tähän kenttään. (ansible-doc authorized_key)

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

Seuraavaksi testasin kirjautua ssh:lla, mutta se kysyi tässä vielä salasanaa. kohdassa `sudo -k` (-k unohtaa tässä mahdolliset salasanat) ja testasin komennolla `sudo echo "testi"`, jotta tässä ei tulisi salasanan kyselyä, ja näinhän siinä kävi! Tämä tapahui käyttäen komentoa `sudo -K`, joka käyttää become salasanaa.

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
playbook ajo näyttää, että .

<img width="940" height="256" alt="kuva" src="https://github.com/user-attachments/assets/10db94b0-70b8-4377-bf9e-5c1f0a5bb03f" />


# c) package. asenna pari pakettia.

Lähdin asentamaan Googlauksen jälkeen pari kevyttä ohjelmaa htop ja ncdu. Htop on komentoriviohjelma, jolla voidaan tarkkailla ja hallita prosesseja. (linux.fi), ja ncdu ohjelman, joka näyttää miten käytän levytilaanin linuxissa (Stocker 2023).

Ensimmäisenä meidän pitää luoda uusi hakemisto ohjelmille, joten tein sen komennolla
```markdown
mkdir -p roles/paketit/tasks
```
Seuraavksi taas main.yml tiedosto tänne hakemistoon komennolla:
```markdown
micro roles/paketit/tasks/main.yml
```
Lopuksi piti hyödyntää ansible-doc atp tiedostoa, jotta saan sisennykset sekä ohjelmat asennettua. Tiedoston lopusta löytyi muutamia esimerkkejä, ja hyödynsin niitä muun muassa, miten asennetaan useampi paketti samaan aikaan.

<img width="374" height="158" alt="kuva" src="https://github.com/user-attachments/assets/a7d639f7-e8d7-4dfc-9925-ca211dd971cb" />


Lopuksi vielä lisäsin paketit käyttäjän site.yml tiedostoon samanlailla kuin edellisessä tehtävässä uusikayttaja.

<img width="416" height="184" alt="kuva" src="https://github.com/user-attachments/assets/78933855-0d4d-41fa-a8fb-d0f0c13eaea1" />

sitten vain ajo playbookilla.

<img width="644" height="103" alt="kuva" src="https://github.com/user-attachments/assets/4c531c69-bb9c-4585-b20e-36b226bdeace" />

jaahas.. virheilmoitus. Pienen päättelyn jälkeen, ilmeisesti ensimmäinen - name sekä state tilat ovat samassa sinnyksessä. Korjataan siten, että state on tuossa toisessa name kohdassa samassa sisennyksessä.

<img width="510" height="186" alt="kuva" src="https://github.com/user-attachments/assets/194c959c-f28a-46f5-a23c-cb5dab3d9b8b" />

Korjasin myös samalla tuon update_cache samalle riville, kokeilin uudelleen ajoa.

<img width="702" height="114" alt="kuva" src="https://github.com/user-attachments/assets/dd6e1581-e774-4e6f-8da0-a0270c772dcf" />

Nyt näytti ainakin tapahtuvan muutoksia, tarkistetaan vielä, että ohjelmat on asentunut.

komennolla `htop` aukesi htop ohjelma sekä komennolla `ncdu` aukesi myös tämä ohjelma, tässä on vielä kuvat. 
<img width="712" height="262" alt="kuva" src="https://github.com/user-attachments/assets/c0f95ff5-235c-4271-8f68-b847dbf9d56a" />

<img width="726" height="404" alt="kuva" src="https://github.com/user-attachments/assets/91a06c74-0f32-4fb3-a950-3822d3df5f54" />

Tehtävä oli siis onnistunut!

## d) file

Lähdin taas luomaan uuden hakemistorakenteen komennolla:
```markdown
mkdir -p roles/files/tasks
```

Sekä main.yml tiedoston komennolla:
```markdown
micro roles/files/tasks/main.yml
```

Tehtävän tekemisessä hyödynsin ansible-doc copy dokumenttia sekä spaceliftin artikkelia, jossa oli kerrottiin pipesta "|" yaml:ssa, jossa voidaan hyödyntää rivinvaihtoa. (ansible-cod copy; spacelift 2025)

<img width="493" height="213" alt="kuva" src="https://github.com/user-attachments/assets/46de5cd5-e78c-49aa-834e-e9a21186223c" />

Lopuksi ajoin playbooksin, ja tuloksesta näkyy, että muuttaja on tullut.
<img width="934" height="152" alt="kuva" src="https://github.com/user-attachments/assets/8d2a13c7-a585-47bf-85c1-495b39d7238f" />


testasin vielä, että tiedosto näkyy `ls -l` komennolla ja `cat tiedosto.txt` sain näkyviin tiedoston, ja sen sisällön.

<img width="560" height="290" alt="kuva" src="https://github.com/user-attachments/assets/28e70f12-f194-46cf-aafb-62139c44ec3a" />

Tässä tiedostossa on siis omistajalla luku ja kirjoitus mahdollisuudet. Ryhmillä sekä muilla ei ole mitään oikeuksia. symboolinen muoto on `-rw------- `.

## e) jotain muuta

otin vaihtoehdoksi ansible.builtin.debug, joka tulostaa tässä tapauksessa tekstiä Playbookin aikana. Tämä voi olla oivallinen debuggaukseen, joka ilmoittaa esimerkiksi jotain tekstiä Playbook ajossa, jos haluaa etsiä vikaa, miksi joku ei toimi (ansible-doc debug)

Toistin siis samaa kaava, ja loin uuden tiedostorakenteen sekä main.yml

```markdown
mkdir -p roles/debug/tasks
```
```markdown
micro roles/debug/tasks/main.yml
```

<img width="704" height="160" alt="kuva" src="https://github.com/user-attachments/assets/66c5b023-eff3-4c0f-aa75-502eca0abd36" />

Lopuksi vielä lisätään `debug` site.yml tiedostoon.

<img width="318" height="207" alt="kuva" src="https://github.com/user-attachments/assets/c2ec1f50-b5a8-4ee2-b90f-134aefeefb4a" />

Sitten taas playbook ajoa

<img width="945" height="207" alt="kuva" src="https://github.com/user-attachments/assets/df8fe6e9-d3a3-4298-b99e-bb09050e7cd1" />

Kuten näemme kuvasta, viesti on tullut onnistuneesta tehtävän ajosta. Toisaalta tässä voisi myös hyödyntää jotain muuttujia, jos haluaisin tarkemmin selvittää jotain. Mutta tässä nyt on vain esimerkki siitä, että ansible toimii!

## Yhteenveto

Ajallisesti tähän meni varmaan noin 8-9 tuntia taukojen kanssa. Aluksi toiminta oli todella hidasta, sekä meni aikaa tajuta sisennykset sekä rakenne yleisesti ansiblessa. Tehtävien teko nopeutui huomattavasti loppua kohden. 

## Lähteet
- Ansible doc-copy. Luettu: 5.11.2026
- Ansible-doc apt. Luettu: 5.11.2026
- Ansible-doc file. Luettu: 5.11.2026
- Ansible-doc user. Luettu: 5.11.2026
- Ansible-doc authorized_key. Luettu: 5.11.2026https://www.networkworld.com/article/972132/using-the-linux-ncdu-command-to-view-your-disk-usage.html. Luettu: 6.11.2026.
- Stocker, Sandra 2026.  Using the Linux ncdu command to view your disk usage. Luettavissa: 
- Karvinen 2026. Sudo without password. Luettavissa: https://terokarvinen.com/passwordless-sudo/. Luettu: 5.11.2026.
- Karvinen 2026. Passwordless Sudo with Ansible. Luettavissa: https://terokarvinen.com/passwordless-sudo-with-ansible/#. Luettu: 5.11.2026.
- Munroe 2026. Sandwitch. Luettavissa: https://xkcd.com/149/. Luettu: 5.11.2026.
- spacelift 2025. How to Add Comments in a YAML File [Tutorial]. Luettavissa: https://spacelift.io/blog/yaml-comments. Luettu: 6.11.2026.
