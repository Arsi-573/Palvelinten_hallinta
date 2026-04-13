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



### Lähteet

https://terokarvinen.com/palvelinten-hallinta/#laksyt

https://terokarvinen.com/apache-ansible/

https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html

