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


