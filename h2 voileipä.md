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
- State: Asentaa viimeisimmän version paketista.
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




## Lähteet
- Karvinen 2026. Sudo without password. Luettavissa: https://terokarvinen.com/passwordless-sudo/. Luettu: 5.11.2026.
- Munroe 2026. Sandwitch. Luettavissa: https://xkcd.com/149/. Luettu: 5.11.2026.
- 
