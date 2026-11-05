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


## Lähteet
- Karvinen 2026. Sudo without password. Luettavissa: https://terokarvinen.com/passwordless-sudo/. Luettu: 5.11.2026.
- Munroe 2026. Sandwitch. Luettavissa: https://xkcd.com/149/. Luettu: 5.11.2026.
- 
