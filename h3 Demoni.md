# h3 Demoni

## x) Lue ja tiivistä

### Apache installed with Ansible

Artikkelissa käydään läpi, kuinka apache2 webpalvelin voidaan asentaa automaattisesti. 

- `tasks` hakemistossa on kokonaisuudessaan ansiblen koodi apachelle. Mikä ohjelma asennetaan, minne tiedosto kopioidaan ja mikä on sen lähde sekä kenelle kuuluu oikeudet. Myös huomautuksena annetaan, että Apache2 pitää käynnistää uudelleen.
- `file` Sisältää tiedoston, jossa määritellään tiedoston sijainti, omistajat sekä ryhmät, link kohta kuvaa sen, että tästä tehdään symboliinen linkki jos sitä ei ole luotu tai sitä muokataan, jos se on jo olemassa.
- `handlers` käynnistää palvelun uudestaan, jos asetuksissa on muutoksia esimerkiksi jos uusi päivitys on tullut tähän. Tämä vaatii `notify` komennon tasks hakemiston main.yml tiedostoon.
- Kaikki nämä hakemistot sijaitsevat Apache2 roolin hakemistossa. (Ansible doc file; Ansible doc handlers; karvinen 2026)

### Handlers

- Viittaan edellisen apache 2 tehtävään, jossa vähän avasin jo handlerissa: "käynnistää palvelun uudestaan, jos asetuksissa on muutoksia esimerkiksi jos uusi päivitys on tullut tähän. Tämä vaatii `notify` komennon tasks hakemiston main.yml tiedostoon."
- handleri sisällytetään tasks hakemistoon esimerkiksi main.yml tiedostoon, jonne lisätään komento `notify`. Notifyyn pitää määritellä ne palvelimet, jotka halutaan uudelleen käynistyvän, jos jotain muutoksia on tullut

### Service

Tämä moduuli toimii välityspalvelimena palvelunhallintamoduuleille, kuten esimerkiksi apache2.
- enabled: Tällä kehotetaan, pitääkö ohjelman startata käynnistyksen aikana. joko `enable` tai `state` atribuutti vähintää tarvitaan.
- name: pavelun nimi
- state: `started` käynnistää palvelun  ja `stopped` ei käynnistä. Nämä toteutuu vain tarvittaessa, jos on muutoksia. `restarted` polkaisee palvelimen käytiin aina uudestaan ja `reloaded` päivittää aina.

EXAMPLES: yksi esimerkki on tässä, jolla käynnistetään httpd, jos se ei ole jo käynnissä:
```markdown
- name: Start service httpd, if not started
  ansible.builtin.service:
    name: httpd
    state: started
```

- name: kerrotaan, mitä tässä kehotteessa tapahtuu
- ansible.builtin.service: Service moduuli
- name: mikä palvelin on kyseessä
- state: määritellään tila, eli tässä tapauksessa se käynnistetään. (ansible-doc service)
## Lähteet
- Ansible. Handlers: running operations on change. Luettavissa: https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html#notifying-handlers. Luettu: 12.11.2026.
- Ansible doc file
- Ansible doc-handers 
- Ansible-doc service
- Karvinen 2026. Luettavissa: https://terokarvinen.com/apache-ansible/. Luettu: 12.11.2026
- 

- 
