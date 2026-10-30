# h1 Hei Ansiblen maailma

## x) Lue ja tiivistä

### SSH public key

- Artikkelissa annetaan ohjeet, kuinka voidaan kirjautua ilman salasanaa ssh:n julkisella avaimella.
- SSH asennuksen jälkeen, komennolla `sudo systemctl enable --now ssh` saadaan ssh saadaan käyttöön, sekä sen aloittaminen `--now ssh`.
- Kopiointi localhostille onnistuu `ssh-copy-id localhost`, tämä siirtää julkisen avaimen `.ssh/authorized_keys` tiedostoon.
- Myös muutamia vianmäärityskomentoja on asiakaspuolelle `ssh -v localhost`, joka tulostaa virheenkorjaus viestejä sekä palvelinpuolella `sudo journalctl --follow`, tämä taas tulostaa viimeisimmät lokitiedot. Viimeisenä `sudo systemctl status ssh`, joka näyttää, onko ssh missä tilassa tällä hetkellä.  

### Ansible

- Ansiblella kirjotat Infrastruktuuria koodina, ja se tekee muutoksia orjatietokoneille. Ansible toimii ssh:n kautta.
- Ansiblelle pitää lisätä host koneita, joita halutaan hallita. Tämä tapahtuu komennolla: `micro hosts.ini` (tässä Micro on tekstieditori ja hosts.ini on host kone)
- `site.yml` määrittelee, mitä rooleja ja asetuksia käyttäjät saavat.

## a) Paikallista. Asenna Debian 13 paikallisesti virtuaalikoneeseen.

Tässä linkki Debianin asennukseen: https://github.com/tormanenjanne/Palvelinten-Hallinta/blob/main/h0%20Oikeasti%20netiss%C3%A4.md

## b) Sshecrets. Asenna SSH-demoni käsin ja testaa se kirjautumalla SSH:lla. (localhost kelpaa)

Tunnin aikana suoritimme jo SSH:n asennuksen, mutta käydään se vielä vaiheittain läpi. Ensimmäiseksi päivitetään paketit `sudo apt-get update`, ja tämän jälkeen asennetaan ssh `sudo apt-get -y install ssh`(-y tässä komennossa vastaa kaikkiin kehotuskysymyksiin yes).

Seuraavaksi alustetaan, että SSH käynnistyy buuttauksen aikana sekä käsketään se käynnistyvän myös heti, tässä vielä komento `sudo systemctl enable --now ssh`.

Nyt voidaan testata, että SSH toimii komennolla `ssh localhost` ja kirjaudutaan ulos komennolla `exit`.
<img width="721" height="278" alt="kuva" src="https://github.com/user-attachments/assets/7d33684b-7bc9-4ddc-bf8a-13fe01a46006" />

Tässä kuvassa tosiaan kirjautuminen tapahtuu jo ilman salasanaa, koska siirsimme jo julkisen avaimen localhostille

## Pubkey. Automatisoi ssh-kirjautuminen julkisella avaimella. (localhost kelpaa)

Edellisessä harjoituksessa teimme avainparit, joten tässä vaiheessa meillä ei ole siihen tarvetta. Ohjeet tähän löytyy edellisestä raportista, jos avainpareja ei olla vielä luotu(https://github.com/tormanenjanne/Palvelinten-Hallinta/blob/main/h0%20Oikeasti%20netiss%C3%A4.md)

Julkisen avaimen kopiointi localhostille onnistuu komenolla `ssh-copy-id localhost`, ja lopuksi komennolla `ssh localhost`, testataan, että tämä toimii. (Edellinen kuva kuvastaa sen, että kirjautuminen onnistuu ilman salasanaa).

## d) Hei Ansible. Tee hei maailma ansiblella ja kokeile sitä SSH:n yli. (localhost kelpaa)

Ansible asennus tapahtuu komennolla `sudo apt-get install ansible micro bash-completion tree`, joka asentaa Ansiblen lisäkisi myös micro tekstieditorin, Bashin sekä tree, joka näyttää puumaisessa muodossa tiedostorakenteesi.

Seuraavaksi tehdään kansio, jossa on meidän käyttäjämme. Kansio luodaan `mkdir ansible/` ja tämän jälkeen siirrytään kansioon `cd ansible/`. Tämän jälkeen micro tekstieditorissa luodaan ini-tiedosto, jonne lisäämme localhost käyttäjän komennolla `micro hosts.ini`. Lisäämällä localhostin varmistetaan, että se varmasti on siellä komennolla `cat hosts.ini`. Tässä kuvassa näemme kokonaisuudessaan, mitä tein äsken.

<img width="708" height="158" alt="kuva" src="https://github.com/user-attachments/assets/c7cdf51c-5e2b-499a-87cd-fbfd2ae88528" />

`ansible all -a 'uptime' -i hosts.ini`(-a antaa avain=arvo muodossa, eli se antaa käskyn mitä suoritetaan tässä tapauksessa `uptime` kertoo kauanko järjestelmä on ollut päällä ja -i mihin tiedostoon tämä komento suoritetaan, jossa sijaitsee localhost kone). Lähde: `man ansible` ja `man uptime`.

<img width="720" height="183" alt="kuva" src="https://github.com/user-attachments/assets/2d0ccc2c-928d-408d-8656-5c47573d0e8c" />

### Python virhe pois

Jos haluamme python versiosta valittamisen pois muokkaamme Microssa `hosts.ini` tiedostoa näin:
- lisäämme kolmannelle riville: `[all:vars]`
- neljännelle riville: `ansible_python_interpreter=/usr/bin/python3`
<img width="446" height="82" alt="kuva" src="https://github.com/user-attachments/assets/a28164f4-3c6c-42d0-8706-3ee819a5a828" />

Seuraavaksi ajamme uudestaan komennon `ansible all -a 'uptime' -i hosts.ini`, ja Pythonin virheilmoitusta ei tule enään uudestaan.

<img width="694" height="70" alt="kuva" src="https://github.com/user-attachments/assets/bc3401d5-ccc9-4b20-8c82-0b809cf93b6a" />

### Pelkän hostin käyttäminen

Luomme configure tiedosto Microssa `micro ansible.cfg`, ja lisäämme nämä:

- Ensimmäiselle riville: [defaults]
- Toiselle riville: inventory = hosts.ini

<img width="302" height="54" alt="kuva" src="https://github.com/user-attachments/assets/06840b87-eb6b-4620-85ff-a624e9a32a9e" />


Tämä siis kertoo, että komentoa -i ei tarvitse enään käyttää, koska se on nyt määritetty toiselle riville komennolla `inventory = hosts.ini`. Lopuksi testaamme, että tämä toimii ilman `-i` atribuuttia komennolla `ansible all -a "uptime"`.

<img width="605" height="65" alt="kuva" src="https://github.com/user-attachments/assets/ff506b31-df64-400e-ab1f-58c0ca7ef9f3" />

### Site.yml

Seuraaksi luomme roolit tietokoneille Microssa. Komennolla `micro site.yml`, luotiin tiedosto, johon lisäämme nämä tiedot:

- Ensimmäinen rivi: hosts: all
- Toinen rivi: roles:
- kolmas rivi: - hello

<img width="251" height="56" alt="kuva" src="https://github.com/user-attachments/assets/fbe396cd-ac8c-42b0-89b2-dbc998c48519" />

kun suoritimme pelikirjan `ansible-playbook site.yml`, huomaamme virheen, jossa ilmoitetaan, että `hello` viestiä ei näy, koska sitä ei olla vielä luotu. site.yml tiedosto kuintenkin toimii, joka on hyvä juttu tässä vaiheessa.

<img width="719" height="174" alt="kuva" src="https://github.com/user-attachments/assets/65c5a69a-d18e-4623-bbd4-bbfde70eb10e" />

### Luodaan ensimmäiselle roolille tiedosto

Luodaan kansio `mkdir -p roles/hello/tasks/`, ja tämän jälkeen Microssa `micro roles/hello/tasks/main.yml`.

- Ensimmäinen rivi: - copy
- Toinen rivi: dest: /tmp/hello-ansible
- Kolmas rivi: content: "Hello world"

<img width="339" height="68" alt="kuva" src="https://github.com/user-attachments/assets/8c1fd04d-2928-4927-a9fd-90711a91d37e" />

Seuraavaksi ajetaan playbook komennolla `ansible-playbook site.yml`. Tässä huomaamme, että olen jo ajanut aikaisemmin tämän tiedoston, joten kohdassa `TASK [hello : copy]` ei ole `change: [localhost]` vaan `ok: [locahost]`

<img width="715" height="234" alt="kuva" src="https://github.com/user-attachments/assets/0e82f9ce-b598-4f54-80cf-67f93bd0bb22" />

Lopuksi huomaamme vielä, että komento toimii myös ssh:lla `ssh localhost 'cat /tmp/hello-ansible'
`, ja se tulostaa `hello world`.

<img width="623" height="38" alt="kuva" src="https://github.com/user-attachments/assets/25e14233-c269-4b1a-9600-37b0b3762061" />

Tätä tehtävää tehdessä hyödynsin Karvinen 2026, Hello Ansible arkittelia mukautuen muutamia komentoja lopussa, jotta sain tulosteeksi `Hello world`.


## Lähteet

- MAN 2026. Manual page server. Luettavissa: https://man.openbsd.org/ssh. Luettu: 29.10.2026.
- MAN journal 2026. Linux manual page. Luettavissa: https://man7.org/linux/man-pages/man1/journalctl.1.html. Luettu: 19.10.2026.
- Karvinen 2026. SSH public key - Login without password. Luettavissa: https://terokarvinen.com/ssh-public-key-login-without-password/. Luettu: 30.10.2026.
- Karvinen 2026. Hello Ansible. Luettavissa: https://terokarvinen.com/hello-ansible/. Luettu: 30.10.2026.
