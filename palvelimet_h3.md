# h3 Demoni

#### X) Tiivistelmät materiaaleista
##### Apache installed with Ansible - quick notes 
https://terokarvinen.com/palvelinten-hallinta/#laksyt
- Package-File-Service-malli: Eli lyhyehkösti selitettynä: ensin asennetaan, sitten muokataan konfiguraatiotiedostoja ja lopuksi käynnistetään. 
- Ansiblen roolitus pitää huolen suoritettavista tehtävistä ja pitää ne erillään.
- Esim. Abache2:n kohdalla roolit voidaan jakaa kolmeen osaan:
  - ```iles/``` tiedostot joita käsitellään, esim. sivusto example.com.conf
  - ```handlers/``` esim. Apachen uudelleenkäynjistys
  - ```tasks/``` Ansiblen koodi, eli ns. suoritettavat tehtävät. 

##### Ansible Community Documentation
https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html
- Handlerit on suunniteltu toimimaan silloin, jos jossakin on tehty muutoksia.
- Notifylla kutsutaan handlereita ja ne suoritetaan sitten koko homman lopuksi. 

##### Ansiblen dokumentaatiot terminaalissa
- Enabled: Käynnistyykö ohjelma käynnistäessä kone.
- Name: Palvelun nimi.
- State: kertoo tilan, esim. käynnissä/ei käynnissä.
- EXAMPLES: Kohdassa on sitten esimerkeillä havainnollistettu noita ylempiä lisää.

<img width="299" height="254" alt="image" src="https://github.com/user-attachments/assets/55c44c94-eb55-4d96-b4a1-058b7397fa68" />

#### A) Apassi
###### Tehtävänanto: Asenna Apache 2 käsin. Weppisivun tulee näkyä palvelimen etusivulla. Sivun tulee olla tavallisen käyttäjän muokattavissa, ilman root- tai sudo-oikeuksia.
Apachen olinkin ehtninyt jo edellisessä Linux-palvelimet kurssissa asennella tälle virtuaalikoneelle, ja sivukin oli tehty pääkäyttäjällä juho jo aiemmin. 

<img width="575" height="116" alt="image" src="https://github.com/user-attachments/assets/a04d0558-67bc-4572-a343-c234cb7ae9b2" />

Nyt siis tämä kyseinen tiedosto tulisi vain saada myös samaan hakemistoon ansjuho- käyttäjälle siten, että tällä käyttäjällä on myös tarvittavat oikeudet kyseiseen tiedostoon. 

Lähdin painimaan tätä siten, että otin jo valmiiksi toisen terminaalin välilehden auki ja kirjauduin siinä ansjuho- käyttäjälle ssh:lla. Siirryin takaisin käyttäjän juho välilehdelle ja ajoin siinä uuden hakemiston ```publicsites/``` luonnin ansjuho käyttäjälle sudona ja samalla vauhdilla siirsin tarvittavan hakemistosisällön ansjuho- käyttäjälle.

<img width="557" height="38" alt="image" src="https://github.com/user-attachments/assets/221c148c-cad9-4a0d-8d68-636f38851b28" />

Tarkastin vielä ansjuho- käyttäjällä, että siirto onnistui komennolla ```ls -F```.

<img width="230" height="24" alt="image" src="https://github.com/user-attachments/assets/30c1de62-639d-45e8-aa84-655abdc7a153" />

Tämän jälkeen vielä vaihdoin omistajuuden, jotta täydet oikeiudetkin siirtyvät ansjuholle.

<img width="488" height="13" alt="image" src="https://github.com/user-attachments/assets/fc2b6ea6-96cc-4b2b-b830-0220098c31e4" />

Tarkastin vaihdon vielä komennolla ```sudo ls -la /home/ansjuho/publicsites/```.

<img width="347" height="60" alt="image" src="https://github.com/user-attachments/assets/df75b625-639f-4255-bbd2-f91bf0e1688f" />

Tein seuraavaksi ansjuho- käyttäjällä tiedoston ansjuho.conf ja täytin sen seuraavan kuvan osoittamalla tavalla hakemaan tiedot oikeasta hakemistosta. 

<img width="330" height="89" alt="image" src="https://github.com/user-attachments/assets/c1bb0418-fb87-4b9e-b4c2-312f993462e8" />

Tämän jälkeen siirsin tiedoston vielä apachen sites-available- kansioon ja otin sivun käyttöön, jonka jälkeen vielä käynnistin apachen uudelleen.

<img width="438" height="75" alt="image" src="https://github.com/user-attachments/assets/47d46f58-104b-4d02-bf75-8fc153631066" />

Kokeilin tämän jälkeen sivun päivitystä selaimessa ja odotusten mukaisesti sivu ilmoitti, ettei pääsyä ole ja oikeudet eivät riitä, joten ryhdyin painimaan apachelle tarvittavia oikeuksia hakea tietoa oikeasta hakemistosta.

<img width="432" height="37" alt="image" src="https://github.com/user-attachments/assets/d22b7558-d473-45fc-a758-72c02da6fe78" />

Kun oikeuksia oli yllä olevan kuvan mukaan leivottu, ajoin vielä ```sudo systemctl restart apache2``` ja kokeilin sivun päivittämistä uudelleen. 

<img width="214" height="60" alt="image" src="https://github.com/user-attachments/assets/d2d988c8-8125-464b-8d13-e65967d068cd" />

Nyt kun sivut toimi, kokeilin vielä ilman sudoa päivittää niiden sisältöä, että oikeudetkin tulee testattua, joten avasin tiedoston ```ansjuho/publicsites/juho/index.html``` uudestaan microlla ja heitin sinne sisältöä.

<img width="229" height="141" alt="image" src="https://github.com/user-attachments/assets/00b822cf-0713-409e-b81b-d04f49ccb5fd" />

Ajoin vielä ```sudo systemctl restart apache2``` ja sen jälkeen päivitin sivun selaimessa. Hyvin näyttäisi toimivan. 

<img width="214" height="106" alt="image" src="https://github.com/user-attachments/assets/d6a86f08-cf46-46d4-a844-eaf698f777b2" />

#### B) Moottorix
###### Tehtävänanto: Asenna Nginx käsin. Weppisivun tulee näkyä palvelimen etusivulla. Sivun tulee olla tavallisen käyttäjän muokattavissa, ilman root- tai sudo-oikeuksia. (Muista sammuttaa Apache ensin.)
Tein ensiksi siten, että laitoin apachen kiinni ja estin sen käynnistymisen bootissa.

<img width="568" height="74" alt="image" src="https://github.com/user-attachments/assets/3f31409b-5535-4ced-963c-9f7ae3cfa9fa" />

Kokeilin näiden jälkeen päivitellä sivua selaimessa ja melko odotettavastikin kyvykkyys yhteyden muodostamiseen oli kadoksissa.

<img width="542" height="292" alt="image" src="https://github.com/user-attachments/assets/efb8a75b-5485-4814-95d7-f71e98f64534" />

Asensin sitten NginX:n ja unohdin tietysti laittaa komennon loppuun ```-y```, joten jouduin nöyrtymään ja kirjoittamaan sen vielä uudelleen.

<img width="479" height="200" alt="image" src="https://github.com/user-attachments/assets/d0613dc2-12a4-4de3-8da6-be5bcd5c10f4" />



### Lähteet

https://terokarvinen.com/palvelinten-hallinta/#laksyt

https://terokarvinen.com/apache-ansible/

https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html

