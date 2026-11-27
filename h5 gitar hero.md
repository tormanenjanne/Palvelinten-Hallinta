# h5 Gitar Hero

## x) Lue ja tiivistä

### What is git?

- Git ajattelee dataa hieman eritavalla, kun tallennat tai commitat Gitissä, se ottaa tästä snapshotin. Se ei varsinaisesti tallenna tiedostoa uudelleen, vaan se tekee linkin edelliseen indettiseen tiedostoon.
- Se toimii myös paikallisesti, eli se ei tarvitse palvelinta toimiakseen. Tällä näet projektiesi historian myös heti. Tämä on yksi Gitin vahvuuksissa, että voit tehdä omia projekteja offlinessa.
- Gitissä on myös eheyttä, ja se hyödyntää sha-1 hashia.
- Kolme tärkeintä tilaa on:
- **modified** = Olet muuttanut tiedostoa, mutta et ole committanut sitä tietokantaan
- **staged** =  Olet merkannut muokatun tiedoston nykyiseen versioon. Ja se on valmiina committamaan snapshotiksi.
- **commited** = Tiedosto on turvallisessti tallennettu tietokantaan. (Chacon & Straub, 1.3 alaluku) 

### Gitin käyttö on lähinnä 'git add --all && git commit; git pull && git push'

- git add --all: Tällä voidaan lisätä tiedostoja valmistelualueelle (staging area). `--all` komento lopussa kertoo siitä, että se muutokset koko Git-arkistoon.
- git commit: Tämä siirtää valmistelualueen (staging area) sinun repositoriin.
- git pull: Ottaa viimeisimmät muutokset projektista, kun käyttäjä on viimeksi kloonannut tämän.
- git push: Päivittää paikallisesta repositorista etärepositoriin. (Gitlab Docs s.a)

## a) Online. Tee uusi varasto GitHubiin

Aloitin tehtävän luomalla uuden repositoryn, ja laitoitn repositoryn nimeksi `sunshine` sekä lisätietoihin saman. Varasto on myös julkinen, sekä `README.md` tiedosto luotiin myös automaattisesti. Lisenssiksi valitsin `GNU General Public License v3.0`.

<img width="693" height="666" alt="kuva" src="https://github.com/user-attachments/assets/94bf58d5-e92b-4db4-9209-bea74b152b02" />

## b) Dolly. Kloonaa edellisessä kohdassa tehty uusi varasto itsellesi, tee muutoksia omalla koneella, puske ne palvelimelle, ja näytä, että ne ilmestyvät weppiliittymään.

Tehtävän vinkeissä oli, että kannattaa käyttää julkista avainta varastoa kloonatessa, ja kirjautumisen ssh-avaimella. Olin tehnyt tämän jo tunnin aikana, eli hakemistosta `cd .ssh`, löytyi avaimeni sekä julkisen avaimen tunnisti siitä, että siinä oli `.pub` lopussa. Tämän jälkeen siirsin julkisen avaimeni kopioimalla se Githubin asetuksiin --> ssh and gpg keys. Täältä valitsi ssh, ja liitin avaimeni tänne.

Jos haluan kloonata tämä uuden varaston, minun pitää ottaa varastosta code-kohdast ssh, ja komennolla `git clone git@github.com:tormanenjanne/sunshine.git` saadaan kloonattua varasto. Gitin olin myös jo asentanut aikaisemmin tunnnilla.

<img width="862" height="481" alt="kuva" src="https://github.com/user-attachments/assets/f5581cc7-1210-455a-b566-9f36c55c0fed" />

Samalla menin suoraan hakemiseen `cd /home/janne/sunshine`, jotta pystyn tekemään varastossa muutoksia. Ajattelin lisätä tänne uuden tiedoston komennolla `micro testing.md`, ja lisäsin sinne hieman tekstiä.

<img width="819" height="159" alt="kuva" src="https://github.com/user-attachments/assets/fa59e2f1-d0a8-4cc5-b6b5-aa9980f9948a" />

Seuraavaksi käytin komentoa `git add .` ja sen jälkeen commitin komennolla `git commit testing.md`, ja lisäsin sinne hieman lisätietoja, mikä on tämän tiedoston tarkoitus

<img width="827" height="215" alt="kuva" src="https://github.com/user-attachments/assets/c88a10e5-f62a-42d5-9f51-5b8ed33f8d68" />

Lopuksi käytin komentoja `git pull` ja sitten `git push`, joka työntää tämän tiedoston minun uudelle varastolle.

<img width="751" height="287" alt="kuva" src="https://github.com/user-attachments/assets/33db4ebd-9ff8-406f-a5d4-ec7d64546eaa" />

Kävin vielä kurkistamassa, että tiedosto oli varmasti tullut Githubiin, ja siellähän se näkyi.

<img width="893" height="373" alt="kuva" src="https://github.com/user-attachments/assets/439524ec-0d67-4b39-8da9-bb99d9edb60a" />

Tämän tehtävän komennot tulivat ulkomuistista. Tuli tehtyä edellisellä oppitunnilla useamman kerran tämä sama homma, joten komennot oli hyvin muistissa.

## c) Doh! Tee tyhmä muutos gittiin, älä tee commit:tia. Tuhoa huonot muutokset ‘git reset --hard’. Huomaa, että tässä toiminnossa ei ole peruutusnappia

Aloitin tehtävän avaamalla uudelleen Microssa testing.md tiedoston, ja lisäsin tänne kirjaimia.

<img width="932" height="333" alt="kuva" src="https://github.com/user-attachments/assets/ca465d6b-1bf8-4bcb-a8b0-c050a775d1eb" />

Lopuksi perehdyin hieman `git reset --hard` komentoon. Komento siis palauttaa edelliseen versioon kaikista tiedostoista, jotka ollaan viimeksi commitettu. (git s.a)

Huomasin vielä, että git status komennolla nähdää se, onko jotain tiedostoja muokattu tai lisätty ylipätään onko jotain muutoksia tehty.

<img width="765" height="184" alt="kuva" src="https://github.com/user-attachments/assets/cd9d0be1-5377-4fd6-b617-9c71b4dde490" />

Lopuksi käytin komentoa `git reset --hard`, ja jos olen oikein ymmärtänyt, tämä pitäisi poistaa testing.md tiedoston kirjaimet, jotka olen lisännyt sinne.


Ja näin siinä myös kävikin.

<img width="934" height="239" alt="kuva" src="https://github.com/user-attachments/assets/ae06cd84-5dee-4dc7-a1b6-458f97dcab90" />

## d) Tukki. Tarkastele ja selitä varastosi lokia. Näytä myös, mitä muutoksia tiedostoihin on tehty. Tarkista, että nimesi ja sähköpostiosoitteesi näkyy haluamallasi tavalla ja korjaa tarvittaessa.

Komennolla `git log --patch` nähdään lokitietoja, ja mitä muutoksia on tehty tiedostoihin.

Oman nimeni ja sähköpostini olen lisännyt jo tunnilla, ja tämä tuli siinä vaiheessa, kun halusin tehdä ensimmäisen commitin. Tässä käytin näitä komentoja tietojen lisäämiseen:

```markdown
git config --global user.email "bhp934@myy.haaga-helia.fi"
git config --global user.name "Janne Tormanen"
```

Näillä komennoilla sain lisättyä sähköpostini sekä nimeni.

Kellonajat kertovat siitä, mitä muutoksia on tehty milloinkin. Huomaamme, että alempi committi näistä on se, kun loin uuden varaston lisäsin sinne lisenssin **GNU General Public License v3.0**. ja siinä on sähköpostina oma sähkäpostini.

Ylhäällä oleva committi kertoo siitä, kun olin tein tehtävässä b) uuden tiedoston. Commitin ylhäällä on tiiviste, joka laskettu commitin aikana. sähköposti sekä nimeni ovat kunnossa, joten niihin ei tarvitse tehdä muutoksia.


<img width="904" height="714" alt="kuva" src="https://github.com/user-attachments/assets/9bd8f3ef-efab-4d96-a18b-7d43623cff8e" />

## e) Gitanbile. Laita Ansible-kansio versionhallintaan. Tee jokin muutos, aja ansiblella, tallenna versio (commit).


Tehtävän aloittaessa käytin tekoälyä minua ymmärtämään, miten tämä pitäisi aloittaa, ja sain jutun juonesta kiinni. Aloitin tehtävän menemällä ansible hakemistoon, ja siellä käytin komentoa `git init`. Git init komento luo tyhjä git-arkiston. (git-init s.a)

<img width="813" height="227" alt="kuva" src="https://github.com/user-attachments/assets/8ed0e60e-bb8c-4351-9683-050e4c7dbb1e" />

Seuraavaksi käytin komentoa `git add .`, joka siirtää nämä hakemistot ja tiedostot staging-aluelle (git-add s.a) `git statuksella` huomaamme, että tiedostot on siirretty nyt staging -tilaan.

<img width="691" height="413" alt="kuva" src="https://github.com/user-attachments/assets/47de7c8e-8aee-4278-86d3-cf6fc63a73ce" />

seuraavaksi ajoin `git commit`, ja lisäsin selityksen, mitä tässä oikein tapahtuu.

<img width="924" height="534" alt="kuva" src="https://github.com/user-attachments/assets/fb434375-42c2-41e3-b80f-2a574371cee7" />

Tämän jälkeen lähdin muuttamaan Mariadb:n asetuksia, jossa alkuperäinen wait_timeout oli 200 sekuntia, muutin sen 300 sekuntiin.

<img width="945" height="260" alt="kuva" src="https://github.com/user-attachments/assets/fc507b88-9fef-48bc-868b-eb69ba55dae9" />

Seuraavaksi lähdin tallentaan muutokset staging-alueelle, komennolla `git add .`, ja `git commit` komennolla, tallensin muutokset varastooni, ja kerroin mistä oli kyse.

<img width="943" height="408" alt="kuva" src="https://github.com/user-attachments/assets/a7c7252a-0460-4dc1-9ab7-fd52bb7ca6da" />

Lopuksi ajoin `ansible-playbook site.yml -K`, jotta nähdään, onko muutoksia tullut Mariadb:hen, ja muutoksia on tullut!

<img width="944" height="198" alt="kuva" src="https://github.com/user-attachments/assets/b9eee2f8-7d12-40c8-959f-422be3e9c5fa" />

Jouduin palamaan vielä edelliseen tehtävään, jonka olin tehnyt, koska en muistanut komentoa, millä sain tarkastettua wait_timeoutin. Komento oli siis `SHOW GLOBAL VARIABLES LIKE 'wait_timeout';` (Törmänen 2026). Huomammee Mariadb:ssä, että muutokset ovat astuneet voimaan, ja **wait_timeout** aika on muuttunut 200 sekunnista --> 300 sekuntiin.

<img width="643" height="151" alt="kuva" src="https://github.com/user-attachments/assets/e874bca8-8ebc-4fa8-adf7-62b67c3a4898" />

Lopputulema oli onnistut, osa komennoista tuli ulkomuistista kun sain tehtävänannosta kiinni sekä käytin edellisen raportin komentoa muistutuksena, miten sain wait_timeoutin näkyviin Mariadb:ssä.



## Lähteet
- ChatGPT tekoäly tehtävän e) avaamiseen.
- Chacon, S. & Straub, B. 2014. Pro git. 2. uudistettu painos. What is Git?, 1.3 alaluku. Luettavissa: https://git-scm.com/book/en/v2. Luettu: 26.11.2026.
- Git s.a. Git-reset. Luettavissa: https://git-scm.com/docs/git-reset. Luettu: 27.11.2026.
- Gitlab Docs s.a. Stage, commit, and push changes. Luettavissa: https://docs.gitlab.com/topics/git/commit/. Luettu: 26.11.2026.
- Gitlab Docs s.a. Common Git commands. Luettavissa: https://docs.gitlab.com/topics/git/commands/. Luettu: 26.11.2026.
- Git s.a. git-add. Luettavissa: https://git-scm.com/docs/git-add. Luettu: 27.11.2026.
- Git s.a. git-init. Luettavissa: https://git-scm.com/docs/git-init. Luettu: 27.11.2026.
- Törmänen, j. 2026. h4 Pizza Fantasia. Luettavissa: https://github.com/tormanenjanne/Palvelinten-Hallinta/blob/main/h4%20Pizza%20Fantasia.md. Luettu: 27.11.2026
