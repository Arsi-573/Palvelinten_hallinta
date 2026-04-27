# h5 Gitar Hero

#### X) Tiivistelmät materiaaleista
##### Chacon and Straub 2014: Pro Git, 2ed: 1.3 Getting Started - What is Git?
- Git tallentaa tiedostot kokonaisina tilannevedoksina eli snapshoteina sen sijaan, että se seuraisi vain yksittäisiä rivimuutoksia.
- Toiminnot ovat paikallisia ja nopeita, sillä koko projektihistoria on omalla koneellasi eikä vaadi jatkuvaa verkkoyhteyttä.
- Kaikki sisältö varmistetaan tarkistussummilla, mikä takaa datan eheyden ja estää huomaamattomat muutokset.
- Työnkulku perustuu kolmeen tilaan: tiedostojen muokkaamiseen, valitsemiseen (staging) ja lopulliseen tallentamiseen (commit).
- Järjestelmä on suunniteltu niin, että kerran tallennetun tiedon hävittäminen on erittäin vaikeaa, mikä tekee kokeilusta turvallista.

##### Komennot
- ```git add --all```: Siirtää kaikki muutetut, poistetut ja uudet tiedostot työhakemistosta valmistelualueelle.
- ```git commit```: Tallentaa valmistelualueella olevat muutokset pysyväksi tilannevedokseksi (Snapshot) paikalliseen tietokantaan.
- ```git pull```: Noutaa muutokset etäpalvelimelta (esim. GitHub).
- ```git push```: Siirtää paikalliseen tietokantaan tallennetut uudet commitit etäpalvelimelle.

Lähteet:
Chacon, S. & Straub, B. (2014). Pro Git. Chapter 2: Git Basics. (https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository)
Git Cheat Sheet https://git-scm.com/cheat-sheet 


#### A) Online
###### Tehtävänanto: Tee uusi varasto GitHubiin (tai Gitlabiin tai mihin vain vastaavaan palveluun). Varaston nimessä ja lyhyessä kuvauksessa tulee olla sana "sunshine". Aiemmin tehty varasto ei kelpaa. (Muista tehdä varastoon tiedostoja luomisvaiheessa, esim README.md ja GNU General Public License 3) Edistyneemmille voi olla hauskaa etsiä ja kokeilla jokin muu palvelu kuin Github.

En pidä itseäni edistyneenä, mutta ajattelin silti kokeilla jotain muuta kuin GitHubia. Etsin hieman vaihtoehtoja ja päädyin sitten kuitenkin sellaiseen kuin CodeBerg, koska sen taustalla ei ole mikään suuryritys, joka veisi lopussa ne kuuluisat tuhkatkin pesästä. Sitä myös mainostettiin vanhan GitHubin näköiseksi, joten ajattelin, että ei se kai ihan hullu vaihtoehto voi käytettävyyden puolestakaan silloin kai olla. 

Kun sitten yritin avata Codebergin sivua https://codeberg.org, tuli hetken odottelun jälkeen melko tyhjentävä viesti:

<img width="448" height="296" alt="image" src="https://github.com/user-attachments/assets/3762c4c1-da37-4146-b363-3efa8d43f9c6" />

Kun sivusto ei vielä n. vartin odottelunkaan jälkeen toiminut, ajattelin, että "no ei sitten" ja kokeilin seuraavaksi sitten GitLabia. Tämäkään ei ihan ongelmitta mennyt, kun rekisteröityessä laitoin vahingossa sähköpostiosoitteeseeni yhden ylimääräisen kirjaimen, joka johti siihen, että en pystynyt tätä sähköpostia vahvistamaan. Kun sitten ajattelin, että eiköhän tästä apua kysymällä selvitä, niin GitLab halusi, että kirjautuisin sisään, jotta voisin kysyä ongelmaani apua. Tämähän ei tietysti ole lainkaan mahdollista, jos en ole sitä sähköpostiakaan saanut vahvistettua. 

Ajattelin kuitenkin, että periksi ei anneta näin helpolla, että teen vain uuden tilin oikealla sähköpostilla niin saan sen vahvistettua ja homma on sillä selvä. Vaan ei ollut, koska miksi olisi? Kokeilin kirjautua sitten sisään kummalla tahansa käyttäjänimellä, tai sähköpostilla, sivusto valitteli vain virheellistä käyttäjänimeä tai salasanaa. Varsin mielenkiintoista, mutta myös turhauttavaa. Tässä vaiheessa oli siis selkeästi oikea aika unohtaa koko GitLab ja siirtyä elämässä eteenpäin kohti uusia pettymyksiä.  

Päädyin onnekseni ihan vain muuten vaan kokeilemaan Codebergin sivua uudestaan, ja kuinka ollakkaan, sehän toimi! Iloitsin maltillisesti tästä, koska seuraavaksi oli vaiheessa sivustolle rekisteröinti, joka ainakin äskettäin GitLabilla osoittautui täysin ylitsepääsemättömäksi esteeksi. 

<img width="631" height="314" alt="image" src="https://github.com/user-attachments/assets/db6726df-073d-4aba-990f-2d36516ee4f5" />

Tällä kertaa sain tilin tehtyä ja sähköpostiosoitteenkin varmennettua, joten pääsin varsinaiseen tehtävän tekoon pienten alkuhankaluuksien jälkeen.

<img width="641" height="178" alt="image" src="https://github.com/user-attachments/assets/82dac288-9bac-478a-9bf3-74870c5b3efc" />

Sivusto tosiaan muistuttaa GitHubia melko paljon, joten klikkasin sivun oikeasta ylälaidasta löytyvää "+"- merkkiä ja sieltä "new repository". 

<img width="359" height="308" alt="image" src="https://github.com/user-attachments/assets/9ee7657a-0816-4cda-a7b5-a748cdef65f5" />

Kun kaikki kohdat oli täytetty, kuten yllä olevassa kuvassa näkyy, skrollasin alas ja klikkasin "Create repository". 

<img width="629" height="293" alt="image" src="https://github.com/user-attachments/assets/1faa59ff-caa2-4f62-b7dd-1b784241adb7" />

#### B) Dolly
###### Tehtävänanto: Kloonaa edellisessä kohdassa tehty uusi varasto itsellesi, tee muutoksia omalla koneella, puske ne palvelimelle, ja näytä, että ne ilmestyvät weppiliittymään. 

Kävin ensin poimimassa oman julkisen SSH- avaimen terminaalista ```cat .ssh/id_ed25519.pub``` ja lisäsin sen Codebergissa siirtymällä oman profiilin asetuksiin sivun oikeasta ylälaidasta --> Settings --> SSH/GPG keys. Klikkasin Manage SSH keys- kohdassa nappia Add key. 

<img width="575" height="158" alt="image" src="https://github.com/user-attachments/assets/7df25eca-34f6-4283-a11e-3ab16cd079e2" />

Tein seuraavaksi terminaalissa uuden hakemiston ```mkdir codeberg``` ja sen alle vielä toisen hakemiston ```mkdir sunshineberg``` 

<img width="278" height="64" alt="image" src="https://github.com/user-attachments/assets/f47c6b63-41bc-4432-80ce-a4e56216be84" />

Ajoin seuraavaksi repon kloonauksen terminaalissa
````
git clone git@codeberg.org:Juissi/SunshineBerg.git .
````

<img width="520" height="101" alt="image" src="https://github.com/user-attachments/assets/dee3842d-0e94-4f00-a627-2be81533637e" />

Tein tämän jälkeen tekstitiedoston microlla
````
micro sunshinecode.txt
````

<img width="227" height="50" alt="image" src="https://github.com/user-attachments/assets/428cbb20-ec2d-4808-be89-0dbf9ffda4f8" />

Tallensin ja suljin editorin ja ajoin sen jälkeen
````
git add --all
git commit
````

Lisäsin kommentin editorissa ja tallensin ```Ctrl + O``` ja suljin tiedoston ```Ctrl + X``` 

<img width="467" height="137" alt="image" src="https://github.com/user-attachments/assets/ff7ac401-d86a-4c15-afd8-feca789dbf6b" />

<img width="395" height="76" alt="image" src="https://github.com/user-attachments/assets/741c83de-c1a4-4728-8b29-2981e7995eaf" />

Ajoin tämän jälkeen terminaalissa 
````
git push origin main
````

<img width="408" height="112" alt="image" src="https://github.com/user-attachments/assets/ff753bef-0a15-41f6-a02e-94594630ac00" />

Katsoin vielä selaimessa Codebergin sivulta, että tämä oli onnistunut ja äskettäin luotu tekstitiedosto oli näöhtävissä.

<img width="578" height="94" alt="image" src="https://github.com/user-attachments/assets/6b7ea44f-f3e7-4991-b84b-9c62e306bca1" />

#### C) Doh! 
###### Tehtävänanto: Tee tyhmä muutos gittiin, älä tee commit:tia. Tuhoa huonot muutokset ‘git reset --hard’. Huomaa, että tässä toiminnossa ei ole peruutusnappia.

Avasin äsken luodun tiedoston uudestaan
````
micro sunshinecode.txt
````

Ja tein sinne seuraavaksi tyhmän muutoksen:

<img width="258" height="45" alt="image" src="https://github.com/user-attachments/assets/7d4695ab-5595-4a4a-8841-08dc638ecbb5" />

Ajoin tämän jälkeen 
````
git status
````

<img width="465" height="126" alt="image" src="https://github.com/user-attachments/assets/9ce4b45c-1beb-4177-b749-80459d6ddde8" />

Ja kuten kuvassa näkyy, tiedostoa on muokattu, mutta status on, ettei sitä ole vielä julkaistu. Seuraavaksi siis tämä tyhmä muutos ajetaan pois KOVAA komentamalla
````
git reset --hard
````
<img width="433" height="26" alt="image" src="https://github.com/user-attachments/assets/814b3f1e-ba08-43d2-9df7-a6f21aa35334" />

Avasin tiedoston vielä uudestaan microlla, ja katsoun, että myrskypilvet olivat poissa ja tyhmät muutokset hävinneet. 

<img width="221" height="39" alt="image" src="https://github.com/user-attachments/assets/aa4113b0-c23d-486a-bcc9-0441e4d67433" />

#### D) Tukki
###### Tehtävänanto: Tarkastele ja selitä varastosi lokia. Näytä myös, mitä muutoksia tiedostoihin on tehty. Tarkista, että nimesi ja sähköpostiosoitteesi näkyy haluamallasi tavalla ja korjaa tarvittaessa.

Otin lokin näkyviin komentamalla
````
git log
````

<img width="518" height="163" alt="image" src="https://github.com/user-attachments/assets/d2207429-891f-4827-9f7b-3cd3f12c56b2" />

Lokissa näkyy:
- Tekijä (juho <juho@juho.com>)
- Päivämäärä ja kellonaika
- Commit- viesti

Ja jotta saadaan vielä tehdyt muutokset näkyviin selkeämmin, annoin komennon 
````
git log -p
````

<img width="520" height="269" alt="image" src="https://github.com/user-attachments/assets/dbd13a61-a764-4d5c-a8f8-0c368228915e" />

Ja näin myös tehdyt muutokset saadaan näkyviin korotettuna vihreällä "+"- merkki etuliitteenä. Jos olisi poistettu jotakin, niin nämä näkyisi puolestaan punaisella ja "-"- merkki edessä. 

Tarkastin vielä nimeni ja sähköpostiosoitteen komennoilla 
````
git config user.name
git config user.email
````

<img width="371" height="50" alt="image" src="https://github.com/user-attachments/assets/f4a30b46-e17e-4efb-8149-4c2eefe88731" />

Ja nämä näytti olevan jo valmiiksi OK, joten jätin muutokset tekemättä niiden osalta. Toki oikeastihan tuo sähköpostiosoite ei johda mihinkään, mutta jos se täytyy muuttaa vielä jossakin vaiheessa oikeaksi niin se onnistuu helposti
````
git config --global user.email "oikea.osoite@juhonmeili.com"
````

#### E) Gitanbile
###### Tehtävänanto: Laita Ansible-kansio versionhallintaan. Tee jokin muutos, aja ansiblella, tallenna versio (commit).

Siirryin oman kotihakemiston kautta yhteen omista Ansible- hakemistoistani (näitä oli useampia, kun oli tehtävissä tullut tehtyä, joten valitsin vain ensimmäisen). Ajoin tässä hakemistossa
````
git init
git add --all
git commit
````

<img width="519" height="249" alt="image" src="https://github.com/user-attachments/assets/25fd92f4-4168-464c-8fb0-28841fb87e7f" />


ja lisäsin tämän jälkeen kommentit editorissa.

<img width="472" height="172" alt="image" src="https://github.com/user-attachments/assets/bf9d005d-a015-4115-b98b-7484f8024804" />

Jotta saisin ansiblella tehtyä jonkin muutoksen, niin avasin ```main.yml```- tiedoston microssa
````
micro roles/hello/tasks/main.yml
````

<img width="287" height="53" alt="image" src="https://github.com/user-attachments/assets/a5dd967b-1c56-4a6f-bf2f-e7212f6a1e83" />

Tiedosto oli sen verran tyhjä, että tämä oli tosiaan ihan ensimmäisiä tehtäviä näemmä mitä oli tullut tehtyä. Ei sillä niin väliä kaiketi, joten tein tähän muutoksia lisäämällä ```debug```- kohdan ja siihen liittyvän viestin. 

<img width="356" height="102" alt="image" src="https://github.com/user-attachments/assets/9bd57b98-14d6-4b84-bdc2-619ed7a336ac" />

Ajoin ansiblen playbookin 
````
ansible-playbook site.yml -i hosts.ini --ask-become-pass
````

<img width="518" height="302" alt="image" src="https://github.com/user-attachments/assets/b930f31e-69d1-40f6-a13a-5ec445240f2f" />

Ajoin tämän jälkeen jälleen
````
git add --all
git commit
````

Kävin välissä jälleen lisäämässä kommentin tehdyistä muutoksista

<img width="434" height="112" alt="image" src="https://github.com/user-attachments/assets/e1768596-a1d4-43b2-94d3-3e186d0514f5" />

ja laitoin lopuksi 
````
git push
````

Tällä kertaa push ei sitten onnistunutkaan, kun olin ihan tyystin unohtanut yhdistää tämän ansible- kansion codebergiin. 

<img width="516" height="137" alt="image" src="https://github.com/user-attachments/assets/d29ef30b-7e77-4728-9b2b-95ec135b9afa" />

Tilanteen saa onneksi korjattua nopeasti luomalla uuden repon Codebergiin nimellä "Ansibles". 
