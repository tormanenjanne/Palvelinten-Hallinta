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



## Lähteet

- Chacon, S. & Straub, B. 2014. Pro git. 2. uudistettu painos. What is Git?, 1.3 alaluku. Luettavissa: https://git-scm.com/book/en/v2. Luettu: 26.11.2026.
- Gitlab Docs s.a. Stage, commit, and push changes. Luettavissa: https://docs.gitlab.com/topics/git/commit/. Luettu: 26.11.2026.
- Gitlab Docs s.a. Common Git commands. Luettavissa: https://docs.gitlab.com/topics/git/commands/. Luettu: 26.11.2026.
