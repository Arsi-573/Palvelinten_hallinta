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

