# h1 Hei Ansiblen maailma

## x) Lue ja tiivistä

- Artikkelissa annetaan ohjeet, kuinka voidaan kirjautua ilman salasanaa.
- SSH asennuksen jälkeen, komennolla `sudo systemctl enable --now ssh` saadaan ssh saadaan käyttöön, sekä sen aloittaminen `--now ssh`.
- Kopiointi localhostille onnistuu `ssh-copy-id localhost`, tämä siirtää julkisen avaimen `.ssh/authorized_keys` tiedostoon.
- Myös muutamia vianmäärityskomentoja on asiakaspuolelle `ssh -v localhost`, joka tulostaa virheenkorjaus viestejä sekä palvelinpuolella `sudo journalctl --follow`, tämä taas tulostaa viimeisimmät lokitiedot. Viimeisenä `sudo systemctl status ssh`, joka näyttää, onko ssh missä tilassa tällä hetkellä.  


## Lähteet

- MAN 2026. Manual page server. Luettavissa: https://man.openbsd.org/ssh. Luettu: 29.10.2026.
- MAN journal 2026. Linux manual page. Luettavissa: https://man7.org/linux/man-pages/man1/journalctl.1.html. Luettu: 19.10.2026. 
