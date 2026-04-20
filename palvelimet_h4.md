# h4 Pizza Fantasia

#### X) Tiivistelmät materiaaleista
##### 4.12.1 Size and Complexity of Some DSLs (112. Ominaisuuksien määrä.)
- Johtavien hallintatyökalujen kielet ovat erittäin monimutkaisia. Esimerkiksi Salt dokumentaatio on yli 20 000 riviä pitkä.
- Pelkkien funktioiden (Saltilla 510, Puppetilla 113) lisäksi kieliin on lisätty omia ohjausrakenteita ja kerroksia, kuten Jinja2-mallineita tai "virtuaalisia resursseja", jotka vaikeuttavat niiden hallintaa.
- DSL-kielet (Domain Specific Languages) eivät usein seuraa perinteisten ohjelmointikielten logiikkaa, mikä luo ylläpitäjille uusia, matalan tason abstraktiokerroksia omaksuttavaksi.

##### 4.12.2 Use of DSL Functions in Case Configuration (112-115. Mitä oikeasti käytetään.)
- Vaikka työkaluissa on satoja toimintoja, tuotantoympäristöjen (kuten Mozilla ja USGCB) analyysi osoittaa, että vain kourallinen funktioita kattaa lähes 90 % kaikista tarpeista.
- Suurin osa konfiguraatioista rakentuu "package-file-service" -mallin (ohjelman asennus, asetustiedosto ja palvelun käynnistys) sekä suorien exec-komentojen ympärille.
- Monet monimutkaiset DSL-rakenteet (kuten Puppetin anchor tai class) ovat käytössä vain siksi, että niillä hallitaan kielen omia sisäisiä rajoitteita, eivät varsinaista järjestelmän hallintaa.

##### 4.12.3.1 Dependencies Between Main Functions (115-117. Tärkeimmät rakennuspalikat.)
- Lähes kaikki järjestelmänhallinta voidaan palauttaa kahteen ydintoimintoon: tiedostojen muokkaamiseen (file) ja komentojen suorittamiseen (exec).
- Järjestelmän vakaus varmistetaan tarkistuksilla (esim. if not exists), jolloin muutoksia tehdään vain tarvittaessa; tämä vähentää ylläpitäjän tarvetta kirjoittaa monimutkaista ohjauslogiikkaa itse.

#### A) Räpylä
###### Tehtävänanto: Asenna itse valitsemasi demoni käsin. Jokin muu kuin tunnilla tai kotitehtävissä aiemmin asennettu, eli ei apache, ngninx eikä openssh-server. Kuten aina, testaa lopputulos.
Ajattelin itse kokeilla Caddya, koska se hoitaa myös SSL- sertifikaatin automaattisesti, jolloin saisin sivulle myös HTTPS:n. Ajoin siis komennot:
````
sudo get update
sudo apt install caddy
````

Ja tämän jälkeen poistin nginx:n käytöstä ja otin caddyn käyttöön. 
````
sudo systemctl stop nginx
sudo systemctl disable nginx
sudo systemctl status nginx
````

<img width="520" height="136" alt="image" src="https://github.com/user-attachments/assets/d40cae17-7a92-4c98-ba70-8e43a2616339" />

Tarkistin, että onhan Caddy käynnissä ```sudo systemctl status caddy```.

<img width="520" height="263" alt="image" src="https://github.com/user-attachments/assets/de5a72fb-c336-42c8-a26d-816355feb31e" />

Koska Caddy ei näemmä ollut aktiivisena jo, potkaisin sen käyntiin ```sudo systemctl start caddy``` ja otin statuksen uudelleen.

<img width="523" height="302" alt="image" src="https://github.com/user-attachments/assets/179b1fb6-6ea9-47b5-ad06-0d60ddb922bb" />

Ja nyt kun Caddy oli käytössä, kokeilin selaimessa mennä localhostiin, ja sivusto näytti toimivan ongelmitta.

<img width="473" height="266" alt="image" src="https://github.com/user-attachments/assets/7739d31c-1d17-45c7-9c38-47f1dc463dbe" />

Otin auki Caddyn tarjoaman ohjeen mukaan "Caddyfilen ```sudo micro /etc/caddy/Caddyfile``` ja kävin muuttamassa hakemiston polun ```root * /var/www/html```.

<img width="394" height="173" alt="image" src="https://github.com/user-attachments/assets/b90514ea-24ec-47bd-b912-42cf034fd409" />

Caddy muuten varsin käyttäjäystävällinen siinäkin mielessä, että jopa tässä Caddyfile- tiedostossa on hyvin ohjeistettu ja opastettu. Myös Caddyn oletussivu sisälsi ensimmäisenä ohjeita ja neuvoja. 

Vaan ei siitä sen enempää. Käynnistin Caddyn uudelleen ja sain selaimessa nyt apache2- oletussivun. Oletan, että tuo apache2- oletussivun HTML- tiedosto siis kummittelee jossain ```var/www```- hakemistossa ja pomppaa sieltä esiin siitä huolimatta, että apache2 itse on pimeänä. 

Tein kuitenkin uuden hakemiston ```sudo mkdir -p /var/www/caddy``` ja sinne avasin uuden tisdoston microlla ```sudo micro /var/www/caddy/index.html```. Kirjoitin sivulle sisällön ja muutin tämän nälkeen oikeudet, jotta sivut on muokattavissa myös peruskäyttäjällä. 

<img width="383" height="26" alt="image" src="https://github.com/user-attachments/assets/a8b9bcc8-7d55-43ab-9207-8e55bfdfcaa3" />

Avasin Caddyfilen uudelleen ja kävin muuttamassa hakemistopolun oikeaksi.

<img width="350" height="82" alt="image" src="https://github.com/user-attachments/assets/f0695e2c-f53f-4872-aaa1-1a167c26fd89" />

Potkaisin Caddyn uudelleenkäynnistyksellä ```sudo systemctl restart caddy``` ja kokeilin selaimessa päivittää sivun.

<img width="198" height="98" alt="image" src="https://github.com/user-attachments/assets/d28e4359-b497-49e0-b403-e96306b17caf" />

Kokeilin vielä, että oikeudet oli oikein ja kävin muokkaamassa sivua ilman sudoa ```micro /var/www/caddy/index.html``` ja kirjoitin sivulle lisää siältöä. 

<img width="484" height="163" alt="image" src="https://github.com/user-attachments/assets/11af30ac-c6c9-4c30-9c29-d3d92fbfd5bd" />

Lopuksi uudelleenkäynnistys Caddylle ja tarkistin sivun selaimessa. 

<img width="262" height="118" alt="image" src="https://github.com/user-attachments/assets/eb3426e0-c353-4896-9a54-997a68fc114e" />

#### B) Automaatti. 
###### Tehtävänanto: Automatisoi valitsemasi demonin asennus Ansiblella. 
Aloitin tekemällä hakemiston ja sen jälkeen tulostamalla puun.
````
mkdir -p /roles/caddy/tasks
tree
````

<img width="179" height="75" alt="image" src="https://github.com/user-attachments/assets/51f232e7-1df6-4628-93f5-00358fe4c9a5" />

Tein uuden main.yml- tiedoston ```/tasks```- hakemistoon microlla.
````
micro roles/caddy/tasks/main.yml
````

<img width="205" height="187" alt="image" src="https://github.com/user-attachments/assets/feff52c5-0f97-4953-9041-888c91f8a46b" />

Ajoin ansiblen playbookin laittamalla komennoksi ja katsoin, että lopputulos on OK.
````
ansible-playbook -i hosts.ini site.yml
````

<img width="521" height="151" alt="image" src="https://github.com/user-attachments/assets/b8ca3c97-378f-4d5a-9a8f-d0db9d3421ee" />

Caddyn taskit oli kaikki OK, joten tein vielä pikaiset testit kytkemällä caddyn pois päältä ja estämällä sen käynnistyksen ja tarkistamalla sen statuksen näillä:
````
sudo systemctl stop caddy
sudo systemctl disable caddy
sudo systemctl status caddy
````

<img width="521" height="237" alt="image" src="https://github.com/user-attachments/assets/710929b8-af02-4453-81ba-af04c57d18d4" />

Ajoin playbookin uusiksi ja toimi jälleen hienosti.

<img width="520" height="156" alt="image" src="https://github.com/user-attachments/assets/eace2776-1948-4f0a-8e47-6f61f848cd59" />

#### C) Asetus. 
###### Tehtävänanto: Muuta asetustiedostoa herralla (master, "control node") ja aja ansible uudestaan. Osoita, että asetukset tulivat käyttöön.
Tein ensin tätä varten hakemiston ```roles/caddy/files```, jonne loin vielä tiedoston ```Caddyfile```. 

<img width="172" height="113" alt="image" src="https://github.com/user-attachments/assets/8bcba240-3954-444d-b547-f7f340055c02" />


<img width="345" height="40" alt="image" src="https://github.com/user-attachments/assets/cfd044e1-3347-4beb-817c-daad476867f8" />

Seuraavaksi tein handlereille oman hakemiston ja sinne oman ```main.yml``` tiedoston, niin saadaan Caddy uudelleen käynnistettyä, jos jokin on muuttunut. 
````
mkdir roles/caddy/handlers
micro roles/caddy/handlers/main.yml
````

<img width="170" height="137" alt="image" src="https://github.com/user-attachments/assets/5d71454f-81a8-4491-a666-44e14f6d97af" />


<img width="188" height="49" alt="image" src="https://github.com/user-attachments/assets/8a0ff2c2-f4d4-4597-81d7-39ac27a29d01" />

Seuraavaksi tuli lisätä tasksiin tämä uusi handlerikin tulee ottaa sitten playbookissa huomioon, niin avasin ```tasks/main.yml``` uudelleen ja lisäsin sinne alla näkyvän pätkän muiden jatkoksi.

<img width="239" height="101" alt="image" src="https://github.com/user-attachments/assets/78af6869-e279-438c-8247-ddfde7b31610" />

Tallensin ja suljin tiedoston ja ajoin playbookin uudelleen testatakseni.

<img width="520" height="233" alt="image" src="https://github.com/user-attachments/assets/d9659009-9475-45c8-b2e9-8e7ad41afec4" />

Sekä Caddyfilen kopiointi, että caddyn restartti näytti muuttuneen. Ajoin vielä testausmielessä ```curl localhost``` ja sain aikaisemmin Caffyfileen lisätyn tekstin.

<img width="234" height="23" alt="image" src="https://github.com/user-attachments/assets/9865d35e-f833-4f12-b91f-3be5c18efa6c" />

Uusi kierros playbookkia ei muuttanut enää mitään, joten totesin homman toimivaksi. 

#### D) Paikka remonttiin.
###### Tehtävänanto: Riko jotain asetuksia. Voit esimerkiksi poistaa demonin 'sudo apt-get purge foobar' (purge poistaa myös asetustiedostoja), poistaa asennuksen yhteydessä tulevan kansion (sudo rm -r /etc/foobar/ # vaarallista) tms. Ja sitten ajaa ansible-roolisi uudestaan ja todeta, että se korjaa tilanteen.
Ajoin tehtävänannossa ohjeistetusti
````
sudo apt-get purge caddy -y
````

Ja katsoin vielä varmuudeksi statuksen
````
sudo systemctl status caddy
````

<img width="499" height="224" alt="image" src="https://github.com/user-attachments/assets/4cfbb945-8b2a-42d2-b095-7426788f6f32" />

Caddy näyttää poistuneen keskuudestamme, joten ajoin playbookkia uudestaan ja sain odotetusti ```caddyn asennus```- kohdassa ```changed: [localhost]```, eli asennus oli siis tehty. Ajoin uudestaan ```sudo systemctl status caddy```, niin sain vielä varmuuden, että caddy on käynnissä ja toimii. 

<img width="519" height="152" alt="image" src="https://github.com/user-attachments/assets/443c1421-33c2-4d20-b841-1aeb321d4e80" />

#### E) Idempotentti.
###### Tehtävänanto:  Osoita, että tilasi on idempotentti.
Koska olin edellisessä kohdassa juuri ajanut playbookin kertaalleen, ajoin sen idempotenssin osoitukseksi kertaalleen uudistaan ja katsoin, että kaikki kohdat on vihreällä ja OK. 

Kuvassa meni teksti hieman pieneksi, mutta tarkkasilmäinen huomannee, ettei vihreän lisäksi ole kuin python3:n räikeän violetti varoitusteksti.

<img width="274" height="340" alt="image" src="https://github.com/user-attachments/assets/5a1b4fd0-4bb4-47be-867a-72cb0c2fff25" />


### Lähteet

https://terokarvinen.com/palvelinten-hallinta/#laksyt

Karvinen 2023: Configuration Management of Distributed Systems over Unreliable and Hostile Networks

Googlen Gemini LLM käytetty apuna microlla tehtävien tiedostojen täyttämiseen.
