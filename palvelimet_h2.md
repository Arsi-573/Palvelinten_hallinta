# h2 Voileipä

#### X) Tiivistelmät materiaaleista
##### Sudo without password https://terokarvinen.com/passwordless-sudo/
- Salasanaton sudo on hyödyllinen etähallinnassa, koska se mahdollistaa pääkäyttäjän komennot ilman salasanaa.
- Käyttäjät on hyvä lisätä ryhmään, jotta kaikkia ryhmän käyttäjiä koskee sama säännöstö salasanattomasta sudosta.
- Sudo- muokkauksen ajan on hyvö pitää auki ylimääräinen ikkuna, jossa root- käyttöoikeus auki, siltä varalta, että sudo- oikeudet hajoaa muokkauksen aikana.

Oma huomio: Kun ryhmä sudottomille käyttäjille on luotu, voidaan vain uudet käyttäjät lisätä ryhmään, niin heitä koskee sama salasanaton säännöstö kuin olemassaoleviakin käyttäjiä. JOkaista käyttäjää ei tarvitse sitten jatkossa käydä erikseen muuttamassa yksi kerrallaan. 

##### xkcd 149: Sandwitch https://xkcd.com/149/
- Sarjakuvassa henkilö pyytää toiselta voileipää, mutta saa vastaukseksi tylysti: "Teeppä itse oma leipäsi", kunnes voileipää pyytävä henkilö käyttää sudo-oikeuksia, jolloin vastaukseksi jää vain "OK".

Oma huomio: Tämä ei muuten toiminut kotona, vaikka toistelin sudoa ääneen useasti. Nukun ensi yön sohvalla.

##### Passwordless Sudo with Ansible https://terokarvinen.com/passwordless-sudo-with-ansible/
- Salasanattomasta sudosta tehdään manuaalisesti tehtävän säädön sijaan automaattista (IaC, Infrastructure as Code).
- Ansibleen luotava rooli ylläpitää sudoless- ryhmää, lisää ryhmän käyttäjille SSH-pääsyn ja mahdollistaa salasanattoman sudo-käytön.

Oma huomio: Automaatiolla eliminoidaan näppäily- ja kirjoitusvirheitä, niin säästetään myös aikaa ylimääräisiltä korjauluilta jälkikäteen. 

##### Ansiblen sisäänrakennettu dokumentaatio
- ansible-doc copy: Kopioi tiedostoja ja asettaa niiden käyttöoikeudet (permissions).
- ansible-doc apt: Hallitsee ohjelmistopaketteja (asennus, päivitys ja poisto) Debian- ja Ubuntu-pohjaisissa järjestelmissä.
- ansible-doc file: Käytetään tiedostojen, hakemistojen ja ominaisuuksien (kuten oikeuksien ja omistajuuden) asettamiseen tai niiden poistamiseen.
- ansible-doc user: Hallinnoi käyttäjiä ja niiden käyttöoikeuksia.
- ansible-doc authorized_key: Lisää tai poistaa SSH- avaimia käyttäjille.

#### A) Sudoless
###### Tehtävänanto: 
Tee ansiblea varten tunnus, jolla voi käyttää sudoa ilman salasanaa. Sekä ssh-kirjautuminen että sudon käyttö tulee olla ansbilea varten automatisoitu.

Aloitin luomalla käyttäjän ```sudo adduser ansjuho```, täyttämällä salasanan kahdesti ja jäötin muut kohdat käyttäjäluonnissa tyhjäksi. Tämän jälkeen vielä loin ryhmän ```sudo groupadd sudoless``` ja lisäsin käyttäjän ansjuho ryhmään ```sudo adduser ansjuho sudoless```. Kirjoitin aluksi käyttäjänimen väärin juhoans, joka näkyy kuvassa, mutta terminaali osasi huomauttaa tästä virheestä.

<img width="395" height="270" alt="image" src="https://github.com/user-attachments/assets/817a0133-17fb-4098-8295-8b95166d46cd" />

Avasin tämän jälkeen toisen välilehden terminaalissa ja kirjauduin sillä roottiin ```sudo -i```. Tämän jälkeen siirryin takaisin aikaisemmalle välilehdelle jatkamaan määrityksiä.

<img width="221" height="56" alt="image" src="https://github.com/user-attachments/assets/f03fd369-51a6-46b1-9103-966854ea6f7d" />

Avasin ```sudoers.d/sudoless```- editorin, jotta saan ajettua salasanattoman sudon sudoless- käyttäjäryhmälle. Syötin editorissa ohjeistuksessa mainitun koodin pätkän ```%sudoless ALL = (ALL) NOPASSWD: ALL``` ja tallensin ja suljin nanoeditorin. 

<img width="446" height="72" alt="image" src="https://github.com/user-attachments/assets/9ea6f5ab-61ed-4e04-8411-14967300a1fd" />

Kokeilin tämän jälkeen toimiiko sudo ilman salasanaa käyttäjällä ansjuho kirjautumalla käyttäjälle SSH:lla ```ssh ansjuho@localhost``` ja vielä tässä vaiheessa salasanaa kysyttiin normaalisti. Käytin vielä varmuudeksi ohjeistuksessakin mainittua ```sudo -k```- komentoa, jotta terminaali unohtaisi, että ansjuho- käyttäjä on juuri sudo salasanaa käyttänyt ja tämän jälkeen kokeilin ```sudo echo moi```- komennolla toimiiko sudo ilman salasanaa, ja ilokseni voin todeta, että kyllä toimi!

<img width="566" height="209" alt="image" src="https://github.com/user-attachments/assets/42892103-d4f2-4f6a-be13-e81f373c1b92" />

#### B) Tunnuksen luonti Ansiblella
###### Tehtävänanto: 
Tee salasanaton, automaattisesti ssh:lla kirjautuva tunnus Ansiblella.

Aloitin luomalla tehtävänannon mukaisen hakimiston käyttäjällä ansjuho, käyttäen komentoa ```mkdir ansible.cfg``` ja sen alla ```mkdir -p roles/ansjuho/tasks``` ja vielä ```touch hosts.ini site.yml roles/ansjuho/tasks/main.yml```. Lisäksi tein vielä ```mkdir -p roles/world/tasks``` ja ```touch roles/world/tasks/main.yml```. Katsoin lopuksi hakemiston komennolla ```tree -F``` käyttäjän korihakemistossa ja huomasin, että nythän tuli tehtyä hölmösti, kun kaikki tekemäni hakemistot ja tiedostot oli menneet ```ansible.cfg/```- hakemiston alle. 

<img width="268" height="209" alt="image" src="https://github.com/user-attachments/assets/07b514ed-fc46-42f3-b180-79bcf5fb0255" />

Ryhdyin siis korjaustoimenpiteisiin, jossa siirtelen ensin ```ansible.cfg/```- hakemistossa olevat tavarat pykälää ylöspäin hakemistopuussa komennolla ```mv ansible.cfg/* .``` ja tarkistin pikaisesti, että komento toimi käyttämällä ```tree -F``` 

<img width="278" height="226" alt="image" src="https://github.com/user-attachments/assets/63999ef6-6c65-4da5-bc16-a1a1628680ad" />

Koska aikaisempi hakemistojen siirto oli toiminut toivotulla tavalla, täytyi enää poistaa hakemisto ```ansible.cfg/``` ja luoda samanniminen tiedosto tilalle. Käytin tähän komentoja ```rmdir ansible.cfg``` ja sen jälkeen vielä ```touch ansible.cfg```. Tarkistin vielä, että kaikki meni oikein tarkastamalla hakemistopuun uudelleen ```tree -F```.

<img width="262" height="240" alt="image" src="https://github.com/user-attachments/assets/fe87a5fc-4289-4d40-8aa3-a09451a2db5d" />

Tämä näytti toimineen toivotulla tavalla, joten pystyin siirtymään eteenpäin tehtävässä roolin luontiin. Koska en ollut ansjuho- käyttäjälle luonut vielä SSH-avainparia, tein avainparin komennolla ```ssh keygen -t ed25519``` löin enteriä 3 kertaa, että avaimen luonti oli valmis ja tulostin julkisen avaimen ```cat ~/.ssh/id_ed25519.pub```, ja kopioin sen erilliseen Debianissa auki olevaan tekstitiedostoon talteen myöhempää varten. 

<img width="494" height="299" alt="image" src="https://github.com/user-attachments/assets/7d47ae81-f3dd-4080-b76a-ab616187026e" />

Nyt kun SSH-avaimet oli kunnossa avasin seuraavaksi micron ```micro roles/ansjuho/tasks/main.yml``` ja syötin Teron ohjeistuksessa olevan rimpsun koodia omilla muokkauksillani. 

<img width="593" height="179" alt="image" src="https://github.com/user-attachments/assets/4105a232-7de1-4d43-a467-900db3dea34f" />

Tallensin tiedoston ja suljin Micron ja kokeilin pikaisesti, että sudo toimii ilman salasanaa käyttäjällä ansjuho käyttämällä ```logout``` ja sen jälkeen kirjautumalla uudestaan sisään ```ssh ansjuho@localhost``` (joka sisäänkirjausvaiheessa kysyy vielä salasanaa) ja tein seuraavaksi ```sudo -k``` ja ```sudo echo moi```- komennot, jotta voin olla varma, ettei salasanaa kysytä. Testi onnistui moitteetta, joten siirryin eteenpäin. 

<img width="521" height="175" alt="image" src="https://github.com/user-attachments/assets/bcc8caef-ebb9-4ee3-b79b-6618128ec002" />

Kävin seuraavaksi avaamassa Microssa site.yml- tiedoston ``` micro site.yml``` ja syötin sinne Teron ohjeistuksen mukaiset koodit alla olevan kuvan mukaan.

<img width="122" height="53" alt="image" src="https://github.com/user-attachments/assets/ec0cbc60-47c7-47ff-983e-b018e43af27f" />

Kokeilin tämän jälkeen ajaa komennon ```ansible-playbook -i hosts.ini site.yml --ask-become-pass``` testausmielessä, ja sain virheilmoituksen "Failed to connect to the host via ssh". Tajusin tässä yhteydessä, että en ollut muuttanut ```hosts.ini```- tiedostoa lainkaan, joten avasin, sen ja syötin sinne kuvan mukaisen sisällön.

<img width="206" height="20" alt="image" src="https://github.com/user-attachments/assets/4633a11d-64a8-4638-b86a-20e8d94038c7" />

Tällä muutoksella Ansible suorittaa kaikki komennot suoraan järjestelmässä, eikä yritä tehdä mitään verkon yli. 

Ajoin ```ansible-playbook -i hosts.ini site.yml --ask-become-pass```- komennon uudelleen ja sain uuden errorin, joka kertoi, ettei valittua hakemistoa ```etc/sudoers.d``` ole olemassa. 

<img width="503" height="36" alt="image" src="https://github.com/user-attachments/assets/21bcf593-db53-4281-a7a2-c1d935a66195" />

Mietin hetken aikaa itsekseni, että mistä tämä virhe mahdollisesti voisi johtua ja avasin tiedoston Microssa uudelleen ```micro roles/ansjuho/tasks/main.yml```, johon polun olin aiemmin syöttänyt. Päädyin loppuviimein kopioimaan polun ja kysymään Googlen Gemini- kielimallilta, että keksiikö se mikä polussa on mahdollisesti vikana, ja vastaukseksi melko nopeasti sainkin, että polkuni alusta puuttuu ```/```- merkki. Korjasin polun, tallensin tiudoston ja suljin Micron. 

<img width="213" height="19" alt="image" src="https://github.com/user-attachments/assets/7b980efc-753c-44e5-8564-2b927cd2eb36" />

Kokeilin jälleen uudelleen ```ansible-playbook -i hosts.ini site.yml --ask-become-pass```- komentoa testatakseni toimivuuden ja tällä kertaa vältyttiin virheilmoituksilta. 

<img width="598" height="285" alt="image" src="https://github.com/user-attachments/assets/60c27afa-5538-40d9-8828-0908242a4f21" />

#### C) Pakettien asennus
###### Tehtävänanto:
Asenna kaksi pakettia ansiblella.

Oman muistin mukaan pakettien asennusta ei oltu vielä ehditty tunneilla käydä tarkemmin läpi, mutta päätin kuitenkin tarttua tehtävään. Skriptit tulee siis lisätä main.yml- tiedostoon, joten avasin sen ```micro roles/ansjuho/tasks/main.yml```. Ajattelin, että ainakin Micro ja Curl on hyvä löytyä aina joka koneelta, niin lisäsin ne skriptiin ja laitoin loppuun vielä ```update-cache: yes```, jotta asennuksessa haetaan aina uusin saatavilla oleva versio asennettavosta ohjelmista. 

<img width="253" height="61" alt="image" src="https://github.com/user-attachments/assets/441cd365-84a9-4c8d-9627-e3a5b08ed0ba" />

Ajoin tämän jälkeen testin ```ansible-playbook -i hosts.ini site.yml -K``` ja tulostusksessa ei tullut virheitä. 

<img width="541" height="255" alt="image" src="https://github.com/user-attachments/assets/dcc72c33-36cf-4f79-9694-85fdebd48215" />

#### D) Tiedoston kirjoitus Ansiblella
###### Tehtävänanto:
Kirjoita orjalle useamman rivin mittainen tiedosto Ansiblella. Määrittele sen omistaja, omistava ryhmä ja oikeudet. Käytä oikeuksille oktaalinumeroa, esim. "0600". Kerro, mitä oikeudet ovat symbolisessa muodossa, esim. "-rwxr--r--". Selitä, mitä kukin käyttäjä saa tehdä tuolle tiedostolle.

Tämä tehdään samalla tavalla ```main.yml```- tiedostoon lisäämällä sitä varten teksti aikaisempien muutosten jälkeen. Avataan siis tiedosto klikkaamalla kahdesti näppäimistössä ylöspäin, jotta komento ```micro roles/ansjuho/tasks/main.yml``` tulee näkyviin ja ryhdytään työstämään iskemällä Enter. 

Syötin Microssa tiedoston jatkoksi suraavassa kuvassa näkyvän pätkän tekstiä

<img width="356" height="122" alt="image" src="https://github.com/user-attachments/assets/5d978ca4-3917-416c-97b8-65236c0ec904" />

Ensiksi siis luodaan tiedosto ```ansible-testitiedosto.txt``` ja sen sisältöön merkataan ensin ```|```, joka kertoo Ansiblelle, että seuraavat kirjoitukset ovat monirivinen merkkijono. Loppuun määrittelin omistajan ansjuho, omistavan ryhmän ansjuho ja oikeudet 0600 (symbolisessa muodossa ```-rw-------```, eli ansjuho: read, write, ja muilla ei oikeuksia).

Tämän jälkeen jällein ajoin ```ansible-playbook -i hosts.ini site.yml -K```, ja erroreilta vältyttiin tälläkin kertaa. Katsoin vielä tuon äsken luodun tiedoston ```ansible-testitiedosto.txt``` oikeudet komennolla ```ls -l /etc/ansible-testitiedosto.txt```.

<img width="509" height="25" alt="image" src="https://github.com/user-attachments/assets/15f502d8-df0c-420c-a90e-11f368b5fa16" />

#### E) Jotain muuta
###### Tehtävänanto:
Näytä esimerkki ansiblen käskystä, jota ei ole vielä käsitelty kurssilla tai kotitehtävissä. Voit ottaa jonkun muun modulin kuin apt, file, copy, user tai authorized_key. Tai voit käyttää ominaisuutta, jota ei vielä ole demonstroitu. Jos tiivistystehtävässä x on mainittu ominaisuuksia, joita ei tunneilla tai läksyissä kokeiltu, nekin kelpaavat.

Ajattelin, että ensiksi olisi tosiaan hyvä valita jokin suhteellisen hyödyllinen moduuli, joten ajattelin, että käyttäisin sitten Network moduulia (inspiraatiota haettu: https://www.geeksforgeeks.org/devops/ansible-module/). Lisätään siis taas ```main.yml``` tiedostoon jatkoa avaamalla se ```micro roles/ansjuho/tasks/main.yml```. 

<img width="284" height="64" alt="image" src="https://github.com/user-attachments/assets/9ece0d59-e3f1-4d53-a249-5f47f1ce99e0" />

```get_url``` hakee HTTP/HTTPS- protokollien yli tavaraa ulkoisesta verkosta, tällä saadaan testattua verkkoyhteyden toimivuus suoraan playbookista. Koska kohdesivu on laitettu nimimuodossa, niin tulee sitten samalla testattua, että myös DNS toimii. ```debug``` varmistaa vielä testin päätyttyä, että homma toimii niinkuin pitääkin. 

Ajoin lopuksi vielä testin ```ansible-playbook -i hosts.ini site.yml -K``` ja tarkastin, että myös uusimmat lisäykset toimii ja kuten tulosteesta voi lukeakin, kaikki on tältä erää kunnossa.

<img width="494" height="295" alt="image" src="https://github.com/user-attachments/assets/770a283a-d1d0-4e2f-82ec-bb87b4562068" />

### Lähteet

https://terokarvinen.com/palvelinten-hallinta/#h2-voileipa

https://terokarvinen.com/passwordless-sudo/

https://terokarvinen.com/passwordless-sudo-with-ansible/

https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_reuse_roles.html

https://www.geeksforgeeks.org/devops/ansible-module/

Google Gemini kielimallia käytetty ongelmanratkaisussa apuna. 
