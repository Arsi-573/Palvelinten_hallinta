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

##### 



### Lähteet

https://terokarvinen.com/palvelinten-hallinta/#laksyt

https://terokarvinen.com/apache-ansible/

https://docs.ansible.com/projects/ansible/latest/playbook_guide/playbooks_handlers.html

