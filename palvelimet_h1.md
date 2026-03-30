# h1 Hei Maailma

#### X) Tiivistelmät materiaaleista
##### SSH public key - Login without password 
- Avaimen luonti onnistuu komennolla ```ssh-keygen```
- Salasanattoman kirjautumisen käyttöönotto komennolla ```ssh-copy-id localhost```
- Kirjautumisen testaus komennolla ```ssh localhost```

- Oma huomio: SSH:n avaimet on jopa useita satoja merkkejä pitkiä, joten niiden murtaminen on erittäin erittäin vaikeaa.

##### Hello Ansible
- Ansible on konfiguraatiohallintaan tarkoitettu työkalu, jossa määritykset ovat koodia.
- Kuvaan Ansiblelle haluamani lopputilanteen, ja Ansible ajaa määritykset ympäristön koneille, vain jos se on tarpeellista.
- Ansible ei vaadi erillisiä asennuksia kohdekoneelle, vaan hallinnointi onnistuu kokonaan SSH:n yli.

- Oma huomio: Ansiblen avulla voi hallita useita koneita yhtaikaa maantieteellisestä sijainnista riippumatta, kunhan Internet- yhteys toimii.

#### A) Sshecrets

Vaikka SSH-demonin asennus (ja muitakin tehtäviä) oli ehditty jo tehdä tunnilla, ajattelin että harjoitus tekee mestarin, joten ajattelin aloittaa "tyhjältä pöydältä". Minulla sattui onneksi olemaan yksi suurinpiirtein tyhjä virtuaalikone valmiina edelliseltä kurssilta (Linux-palvelimet), joten avasin sen ja kirjauduin sisään käyttöjärjestelmään. 
Avasin terminaalin ja ajattelin, että järkevin on testata onko SSH tullut mahdollisesti jo asennettua tälle kyseiselle koneelle, joten kirjoitin komennon ```systemctl status ssh```, joka kertoo lyhyesti SSH:n statuksen. Vastaus ei ollut yllättävä ja SSH:ta ei tältä kyseiseltä koneelta löytynyt.

<img width="407" height="45" alt="image" src="https://github.com/user-attachments/assets/9dd5b67a-b91f-49fb-bc4e-b274b1762627" />

Asensin tämän jälkeen SSH:n komennoilla ```sudo apt update``` ja ```sudo apt install ssh -y```. Asennuksen jälkeen testasin kirjautumisen komennolla ```ssh localhost```, kirjoitin vastauksen ```yes``` sisäänkirjauksen sitä kysyessä ja annoin salasanani ja totesin, että sisäänkirjaus toimi, nähdessäni tekstin tutut Debianinin takuuta koskevat viestit.

<img width="516" height="164" alt="image" src="https://github.com/user-attachments/assets/7de649d8-6750-4ab1-9cbb-c2625914ff68" />

Tämän jälkeen ajattelin vielä testata, että kirjaus on takuulla toiminut joten annoin komennon ```w``` ja huomasin, että kaksi käyttäjäjää on kirjautuneena. Kirjauduin localhostista ulos komennolla ```exit``` ja kokeilin sen jälkeen vielä uudelleen komentoa ```w```, jolloin huomasin, että SSH-uloskirjauksen jälkeen käyttäjämäärä oli muuttunut yhteen. 

<img width="539" height="134" alt="image" src="https://github.com/user-attachments/assets/95f01907-6809-4360-81ba-b8cee705bee1" />


#### B) Pubkey

