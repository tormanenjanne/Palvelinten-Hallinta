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














## Lähteet

- Chacon, S. & Straub, B. 2014. Pro git. 2. uudistettu painos. What is Git?, 1.3 alaluku. Luettavissa: https://git-scm.com/book/en/v2. Luettu: 26.11.2026.
- Gitlab Docs s.a. Stage, commit, and push changes. Luettavissa: https://docs.gitlab.com/topics/git/commit/. Luettu: 26.11.2026.
- Gitlab Docs s.a. Common Git commands. Luettavissa: https://docs.gitlab.com/topics/git/commands/. Luettu: 26.11.2026.
