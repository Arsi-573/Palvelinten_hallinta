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

Aloitin luomalla avainparin komennolla ```ssh-keygen``` ja löin enteriä kolmesti.

<img width="423" height="232" alt="image" src="https://github.com/user-attachments/assets/2c27aa07-b256-4264-a583-e38f33b737d0" />

Ajattelin käydä katsomassa, että avain tuli luotua joten käytin komentoa ```cat ~/.ssh/id_ed25519.pub``` ja sain julkisen avaimeni näin tulostettua terminaaliin.

<img width="502" height="23" alt="image" src="https://github.com/user-attachments/assets/ac757991-ad6b-4fb4-bd7c-2decaf391333" />

Kopioin SSH-avaimeni localhostille komennolla ```ssh-copy-id localhost``` ja kokeilin tämän jälkeen (myös terminaalin kehotuksesta) kirjautua sisään localhostiin komennolla ```ssh localhost```. 

<img width="563" height="254" alt="image" src="https://github.com/user-attachments/assets/9b7f71be-ffc1-488d-847e-39fef3a20316" />

Ja näin salasanaa ei enää sisäänkirjautuessa localhostille SSH:n yli tarvita. 

#### C) Hei Ansible 

Asensin seuraavaksi Ansiblen komennolla ```sudo apt install ansible -y``` ja loin hakemiston ```/ansibles``` komennolla ```mkdir ansibles```. 

Siirryin luomaani hakemistoon ja loin siellä uuden tiedoston ```hosts.ini``` käyttämällä komentoa ```micro hosts.ini```. Tajusin heti painettuani Enteria, että eihän tässä koneessa varmasti ole microa asennettuna, mutta yllätyinkin positiivisesti huomatessani, että no onhan se!

<img width="233" height="33" alt="image" src="https://github.com/user-attachments/assets/7a6b90cd-3648-4fb0-aa3d-86eb36f63086" />

Kirjoitin tunnilla nähdyn sisällön tiedostoon Microlla

```
[web]
juho@localhost
```

Tämän jälkeen loin seuraavaksi tiedoston ```site.yml``` microlla ja täytin sen tunnilla nähdyllä tavalla

<img width="101" height="43" alt="image" src="https://github.com/user-attachments/assets/c371c2f1-0e08-4331-b5b6-28620b7536bc" />

Kokeilin tämän jälkeen, että olen oikeilla jäljillä, joten syötin tunnilla käydyt komennot ```head *``` ja ```ansible-playbook site.yml```. Lopputulos vastasi oppitunnilla näkemiäni virheviestejä, joten ajattelin, että tässä vaiheessa ei kannata huolestua!

<img width="563" height="244" alt="image" src="https://github.com/user-attachments/assets/dcbbc53e-c5b8-4237-b560-4d37efe5db25" />

Katsoin ensin hakemiston ansibles sisällön siinä ollessani ```ls -F```, ja tein tämän jälkeen sinne kansion ```roles``` komennolla ```mkdir roles``` ja tarkistin uudestaan, että tällä kertaa se myös löytyy hakemiston alta syöttämällä uudestaan ```ls -F```. 

<img width="155" height="45" alt="image" src="https://github.com/user-attachments/assets/3b342968-ed4a-4a47-9fe3-b6c8b04d68a3" />

Tämän jälkeen loin microlla tiedoston käyttämällä komentoa ```micro roles/hello/tasks/main.yml``` ja syötin sinne tunnilla nähdyn sisällön.

<img width="201" height="26" alt="image" src="https://github.com/user-attachments/assets/0cd90556-3642-4492-bfe6-21e8c8930884" />

Tallensin ja suljin tiedoston ja kokeilin tämän jälkeen jälleen, olenko oikeilla jäjillä komennolla ```ansible-playbook site.yml -i hosts.ini```. Virhetekstit vastasivat samaan aikaan tunnilla nähtyjä, joten päättelin jälleen olevani vielä toistaiseksi oikeilla jäljillä.

<img width="563" height="170" alt="image" src="https://github.com/user-attachments/assets/0a98526d-3d8f-46dc-a377-33c021f909cb" />

Seuraavaksi tuli siis saada tämä kysymään salasanaani, joten käytin komentoa ```ansible-playbook site.yml -i hosts.ini --ask-become-pass```, ja syötin salasanani. Tulosti näytti kohdassa ```PLAY RECAP```, että yksi muutos on tehty, joten tässäkin kohtaa olin mielestäni edelleen oikeilla jäljillä.

<img width="562" height="197" alt="image" src="https://github.com/user-attachments/assets/14988071-674d-4f97-b2cc-a347f5e64bed" />

Kirjauduin seuraavaksi localhostille SSH:lla, siirryin hakemistoon ```/tmp``` katsoin sen sisällön ```ls``` ja syötin tämän jälkeen ```cat hello-ansible```. Lopputuloksena sain tulosteen viimeisimpänä luodun tiedoston Content- kohdasta ```Heippa Ansiblen maailma!``` 

<img width="501" height="275" alt="image" src="https://github.com/user-attachments/assets/fbd8ef0e-cd5a-45d6-b879-f53b93c77a64" />

Totesin, että kaikki toimii, kuten pitääkin ja meni tunnilla käytyjen oppien mukaan mukavan helposti. Lopuksi suljin yhteyden komennolla ```exit```.

### Lähteet

https://terokarvinen.com/palvelinten-hallinta/#h1-hei-ansiblen-maailma

https://terokarvinen.com/ssh-public-key-login-without-password/

https://terokarvinen.com/hello-ansible/

Oma tunnilla käyttämä toinen virtuaalikone toimi myös hyvänä lähteenä! 
